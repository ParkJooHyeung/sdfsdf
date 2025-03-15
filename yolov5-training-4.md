apiVersion: batch/v1
kind: Job
metadata:
  name: yolov5-train-job
spec:
  template:
    spec:
      initContainers:
        - name: dataset-init
          image: minio/mc
          env:
            - name: MINIO_URL
              value: "http://223.194.32.63:9000"
            - name: MINIO_BUCKET
              value: "kube"
            - name: DATASET_PATH
              value: "/datasets/coco128"
            - name: MINIO_ACCESS_KEY
              value: "WfrnjAjZQKWNo7AEMCoH"
            - name: MINIO_SECRET_KEY
              value: "EbjmhHFdLYjfSbGHUcpg8voebSz6agHctog1sxQ3"
          command: ["sh", "-c"]
          args:
            - |
              echo "🔧 Setting up MinIO client..."
              mc alias set minio $MINIO_URL $MINIO_ACCESS_KEY $MINIO_SECRET_KEY
              sleep 1

              echo "📂 Downloading dataset from MinIO..."
              mkdir -p $DATASET_PATH/images
              mkdir -p $DATASET_PATH/labels

              mc cp minio/$MINIO_BUCKET/coco128/coco128.yaml $DATASET_PATH/coco128.yaml || exit 1
              mc cp --recursive minio/$MINIO_BUCKET/coco128/images/train2017/ $DATASET_PATH/images/ || exit 1
              mc cp --recursive minio/$MINIO_BUCKET/coco128/labels/train2017/ $DATASET_PATH/labels/ || exit 1

              echo "✅ Dataset download complete."
          volumeMounts:
            - name: dataset-volume
              mountPath: /datasets

      containers:
        - name: yolov5-trainer
          image: ultralytics/yolov5:latest
          env:
            - name: MINIO_URL
              value: "http://223.194.32.63:9000"
            - name: MINIO_BUCKET
              value: "kube"
            - name: DATASET_PATH
              value: "/datasets/coco128"
            - name: MLFLOW_TRACKING_URI
              value: "http://223.194.32.63:6060"
            - name: MLFLOW_EXPERIMENT_NAME
              value: "yolov5_20240310"
            - name: YOLO_EPOCHS
              value: "10"
            - name: YOLO_IMG_SIZE
              value: "640"
            - name: YOLO_BATCH_SIZE
              value: "4"
            - name: YOLO_WORKERS
              value: "0"
            - name: YOLO_OPTIMIZER
              value: "Adam"
            - name: MINIO_ACCESS_KEY
              value: "WfrnjAjZQKWNo7AEMCoH"
            - name: MINIO_SECRET_KEY
              value: "EbjmhHFdLYjfSbGHUcpg8voebSz6agHctog1sxQ3"
          command: ["bash", "-c"]
          args:
            - |
              echo "🚀 Installing dependencies..."
              apt-get update && apt-get install -y wget mc python3-pip
              pip install mlflow pandas pyyaml boto3

              echo "🚀 Setting up MinIO client..."
              mc alias set minio $MINIO_URL $MINIO_ACCESS_KEY $MINIO_SECRET_KEY
              sleep 1

              echo "🚀 Starting YOLOv5 training..."
              python3 train.py --data $DATASET_PATH/coco128.yaml --weights yolov5s.pt --epochs $YOLO_EPOCHS --img-size $YOLO_IMG_SIZE --batch-size $YOLO_BATCH_SIZE --workers $YOLO_WORKERS --optimizer $YOLO_OPTIMIZER || exit 1

              echo "✅ Training complete."

              mkdir -p ./datasets/outputs
              EXP_DIR=$(ls -td /usr/src/app/runs/train/exp* | head -1)
              EXP_NAME=$(basename $EXP_DIR)
              echo "📂 Using EXP_DIR=$EXP_DIR"

              echo "📂 Checking training output..."
              ls -lh $EXP_DIR

              echo "📡 Uploading results to MinIO under /yolov5/$EXP_NAME/..."
              mc alias set minio $MINIO_URL $MINIO_ACCESS_KEY $MINIO_SECRET_KEY

              # MinIO에 디렉터리 업로드
              mc cp -r "$EXP_DIR" "minio/$MINIO_BUCKET/yolov5/$EXP_NAME/"

              echo "✅ Training and upload process finished!"

              echo "✅ Running MLflow logging..."
              python3 - <<EOF
              import mlflow
              import os
              import yaml
              import pandas as pd

              mlflow.set_tracking_uri(os.getenv("MLFLOW_TRACKING_URI"))
              mlflow.set_experiment(os.getenv("MLFLOW_EXPERIMENT_NAME"))

              EXP_DIR = "$EXP_DIR"

              with mlflow.start_run():
                  hyp_path = os.path.join(EXP_DIR, "hyp.yaml")
                  if os.path.exists(hyp_path):
                      with open(hyp_path, "r") as f:
                          hyperparams = yaml.safe_load(f)
                          mlflow.log_params(hyperparams)

                  results_csv_path = os.path.join(EXP_DIR, "results.csv")
                  if os.path.exists(results_csv_path):
                      df = pd.read_csv(results_csv_path)
                      for col in df.columns:
                          mlflow.log_metric(col, df[col].values[-1])

                  best_model_path = os.path.join(EXP_DIR, "weights", "best.pt")
                  if os.path.exists(best_model_path):
                      mlflow.log_artifact(best_model_path)

                  for file in ["results.png", "confusion_matrix.png", "F1_curve.png"]:
                      file_path = os.path.join(EXP_DIR, file)
                      if os.path.exists(file_path):
                          mlflow.log_artifact(file_path)

                  print("✅ MLflow logging completed!")
              EOF

              echo "✅ Training, MinIO upload, and MLflow logging finished!"
          volumeMounts:
          - name: dataset-volume
            mountPath: /datasets
          - name: dshm
            mountPath: /dev/shm
          resources:
            limits:
              memory: "12Gi"
              cpu: "4"
            requests:
              memory: "8Gi"
              cpu: "2"

      restartPolicy: Never
      volumes:
      - name: dataset-volume
        emptyDir: {}
      - name: dshm
        emptyDir:
          medium: Memory
          sizeLimit: 10Gi
  backoffLimit: 1
