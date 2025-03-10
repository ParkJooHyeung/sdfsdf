# **타이틀**
```
# H1
## H2
### H3
#### H4
##### H5
###### H6
```
> 출력결과

# H1
## H2
### H3
#### H4
##### H5
###### H6

#**강조**
```
**진하게**
<u>밑줄</u>
~~취소~~
_이탤릭_
```
> 출력결과

**진하게**  
<u>밑줄</u>  
~~취소~~  
_이탤릭_  

#**문장 줄바꿈**
```
문장
줄바꿈
안함

문장  (space 2개)
줄바꿈  (space 2개)
함
```
> 출력결과

문장
줄바꿈
안함

문장  
줄바꿈  
함

#**블럭(인용) 들여쓰기**
```
> 블럭 1단계
>> 블럭 2단계
```
> 출력결과

> 블럭 1단계
>> 블럭 2단계

#**목록**
```
* 사과
* 오렌지
  + 딸기 (tap)
  + 파인애플 (tap)
- 포도
- 바나나
1. 참외
2. 수박
```
> 출력결과

* 사과
* 오렌지
  + 딸기
  + 파인애플
- 포도
- 바나나
1. 참외
2. 수박

#**Code Block**
~~~
  ```python
  import something
  a = 10
  print(a)
  if a>10:
    print(a)
  ```
~~~
```
문장내 `code` 삽입
```
> 출력결과
  ```python
  import something
  a = 10
  print(a)
  if a>10:
    print(a)
  ```
  문장내 `code` 삽입

  #**하이퍼링크**
  ```
    [바로가기](http://google.com)
    <http://google.com>
  ```
> 출력결과

[바로가기](http://google.com)  
<http://google.com>

#**수평선**
```
  ***
  수평선

  ----
  (3개이상)
```
> 출력결과
  ***
  수평선

  ----

#**표**
```
  | 1열 | 2열 | 3열 |
  | :------- | ----: | :---: |
  | 왼쪽 | 오른쪽 | 가운데 |
  | 정렬하기 | 정렬하기 | 정렬하기 |
  | 테스트 | 테스트 | 테스트 |
  ```
>출력결과

  | 1열 | 2열 | 3열 |
  | :------- | ----: | :---: |
  | 왼쪽 | 오른쪽 | 가운데 |
  | 정렬하기 | 정렬하기 | 정렬하기 |
  | 테스트 | 테스트 | 테스트 |

  #**그림**
  ```
    ![이미지설명문구](https://www.kw.ac.kr/ko/img/symbol01_01.jpg)
  ```
>출력결과

![이미지설명문구](https://www.kw.ac.kr/ko/img/symbol01_01.jpg)

#**체크박스**
```
(todo list 같은 것에 좋다)
  * [ ] 비어있는 체크박스
  * [x] 체크된 체크박스
```
> 출력결과

  * [ ] 비어있는 체크박스
  * [x] 체크된 체크박스

#**수식**
```
(LaTex 문법 참조 [링크1](https://ko.wikipedia.org/wiki/%EC%9C%84%ED%82%A4%EB%B0%B1%EA%B3%BC:TeX_%EB%AC%B8%EB%B2%95), [링크2](https://velog.io/@d2h10s/LaTex-Markdown-%EC%88%98%EC%8B%9D-%EC%9E%91%EC%84%B1%EB%B2%95), [링크3](https://itpro.tistory.com/115), [링크4](https://huni0318.github.io/blog/blog-etc/2020-12-21-markdown-tutorial2/))

$$
\begin{aligned}
f(x)&=ax^2+bx+c\\
g(x)&=Ax^4
\end{aligned}
$$
```
> 출력결과

$$
\begin{aligned}
f(x)&=ax^2+bx+c\\
g(x)&=Ax^4
\end{aligned}
$$

```
문장내 $\frac{1+s}{s(s+2)}$ 삽입
$$\frac{1+s}{s(s+2)}$$
```
>출력결과

문장내 $\frac{1+s}{s(s+2)}$ 삽입
$$\frac{1+s}{s(s+2)}$$
```
$\displaystyle\lim_{s\to\infty}{s^2}$
$\displaystyle\sum_{i=0}^{\infty}{(y_i-t_i)^2}$
```
> 출력결과

$\displaystyle\lim_{s\to\infty}{s^2}$
$\displaystyle\sum_{i=0}^{\infty}{(y_i-t_i)^2}$
```
$\begin{matrix}1&2\\3&4\\ \end{matrix}$
$\begin{pmatrix}1&2\\3&4\\ \end{pmatrix}$
$\begin{bmatrix}1&2\\3&4\\ \end{bmatrix}$
$\begin{Bmatrix}1&2\\3&4\\ \end{Bmatrix}$
$\begin{vmatrix}1&2\\3&4\\ \end{vmatrix}$
$\begin{Vmatrix}1&2\\3&4\\ \end{Vmatrix}$
```
(`&`로 열을 구분, `\\`로 행을 구분)
> 출력결과

$\begin{matrix}1&2\\3&4\\ \end{matrix}$
$\begin{pmatrix}1&2\\3&4\\ \end{pmatrix}$
$\begin{bmatrix}1&2\\3&4\\ \end{bmatrix}$
$\begin{Bmatrix}1&2\\3&4\\ \end{Bmatrix}$
$\begin{vmatrix}1&2\\3&4\\ \end{vmatrix}$
$\begin{Vmatrix}1&2\\3&4\\ \end{Vmatrix}$

#**실습과제**

* EX1_P1.pdf 참조

---
#**P1**

#**실습 - Readme.md 파일 만들기**
---
By 이기백  
전기공학머신러닝실습 첫 번째 실습과제  

##Technologies Used

  * Google Colab
  * Jupyter Notebook
  * Markdown

##Description

이 과제에서는 Open source 공유시 활용되는 일반적인 Reade.me 파일 양식을 흉내내어 직접 markdown 파일을 작성해보고 markdown의 기본 문법을 복습합니다.  
이 문서는 Google Colab에서 Markdown 문법을 이용해 작성되었습니다.

##Setup/Insallation Requirements
  * Google 계정 생성 및 _Colab_ 로그인
  * 새로운 노트 생성
    + 새로운 노트의 파일명을 **학번_이름_EX1_P1.ipynb로 변경**
  * 해당 노트 파일의 결과를 조교가 확인

##Image example
  * 이미지 띄워보기 연습
    ![이미지설명문구](https://www.job-post.co.kr/news/photo/202108/32712_30571_3610.jpg)

##Table example
  * 표 그리기 연습

| 품목 | 단가 | 개수 | 금액 |
| :------- | ----: | :---: | ----: |
| 브로콜리 | 1,500 | 5 | 7,500 |
| 가지 | 1,000 | 10 | 10,000 |
| 아스파라거스 | 1,000 | 15 | 15,000 |

##Equation example
* 수식 표시 연습
$$
\begin{aligned}
f(x) = \int_{0}^{10} \frac{2x^2 + 4x + 6}{x - 2} \,dx
\end{aligned}
$$

##Known Bugs
* 알려진 <u>버그</u> 없음

##License
><http://google.com>

Copyright (c) 2022 Ki-Baek Lee
