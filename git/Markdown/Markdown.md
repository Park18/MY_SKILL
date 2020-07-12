# 마크다운(Markdown) 작성법
## 1. 마크다운
### 1.1. 마크다운이란?
[마크다운(Markdown)](https://ko.wikipedia.org/wiki/%EB%A7%88%ED%81%AC%EB%8B%A4%EC%9A%B4, "위키백과")은 텍스트 기반의 마크업언어로 2004년 존그루버에 의해 만들어졌으며 쉽게 쓰고 읽을 수 있으며 HTML로 변환이 가능하다. 특수 기호와 문자를 이용한 매우 간단한 구조의 문법을 사용하여 웹에서도 보다 빠르게 컨텐츠를 작성하고 보다 직관적으로 인식할 수 있다. 마크다운이 최근 각광받기 시작한 이유는 [깃허브](https://github.com, "깃허브") 덕분이다. 깃허브의 저장소(Repository)에 관한 정보를 기록하는 README.md는 깃허브를 사용하는 사람이라면 누구나 가장 먼저 접하게 되는 마크다운 문서이다. 마크다운을 통해서 설치방법, 소스코드 설명, 이슈 등을 간단하게 기록하고 가독성을 높일 수 있다는 강점이 부각되면서 점점 여러 곳으로 퍼져나가게 된다.

### 1.2. 마크다운 장단점

#### 1.2.1. 장점
1. 간결하다.
2. 별도의 도구없이 작성가능하다.
3. 다양한 현태로 변환이 가능하다.
4. 텍스트로 저장되기 때문에 용량이 적어 보관이 용이하다.
5. 지원하는 프로그램과 플랫폼이 다양하다.

#### 1.2.2. 단점
1. 표준이 없다.
2. 표준이 없기 때문에 도구에 따라서 변환방식이나 생성물이 다르다.
3. 모든 HTML 마크업을 대신하지 못한다.

--------------------------------------------------------------------------------------------- 
<!------------------------------------------ 절취선 ----------------------------------------->

## 2. 마크다운 문법

### 2.1. 제목(Headers)
* 큰제목(H1): 문서 제목

```
큰제목 (H1)
===========
```

큰제목 (H1)
===========

* 작은제목(H2): 문서 부제목

```
작은제목(H2)
-----------
```

작은제목(H2)
-----------

* 글머리: 1 ~ 6

```
# Header - H1
## Header - H2
### Header - H3
#### Header - H4
##### Header - H5
###### Header - H6
####### Header - H7
```
# Header - H1
## Header - H2
### Header - H3
#### Header - H4
##### Header - H5
###### Header - H6
####### Header - H7

H7 부터는 지원하지 않는다.

### 2.2. 인용 블록(BlockQuote)
인용 블록을 사용하기 위해서는 ```>``` 를 사용한다.

```
> First BlockQuote
>   > Second BlockQuote
>>> Third BlockQuote
```
> First BlockQuote
>   > Second BlockQuote
>>> Third BlockQuote

블록 인용 문자를 반드시 떨어트릴 필요는 없다.

```
> ### This is H3
> * List
>   ```
>   code
>   ```
```

> ### This is H3
> * List
>   ```
>   code
>   ```

인용 블록 안에 다른 마크다운 요소를 포함할 수 있다.

### 2.3. 목록
* 순서가 있는 목록(번호)
```
1. First
2. Second
3. Third
```
1. First
2. Second
3. Third

현재까지는 어떤 번호를 입력해도 순서는 내림차순으로 정의된다.
```
1. First
3. Third
2. Second
```
1. First
3. Third
2. Second

* 순서가 없는 목록(글머리기호 ```*```, ```+```, ```-``` 지원)
```
* 1
    * 2
        * 3
+ 1
    + 2
        + 3
- 1
    - 2
        - 3
```
* 1
    * 2
        * 3
+ 1
    + 2
        + 3
- 1
    - 2
        - 3

혼합해서 사용하는 것도 가능하다.
```
* 1
    - 2
        + 3
            + 4
```
* 1
    - 2
        + 3
            + 4

### 2.4. 코드
4개의 공백 또는 하나의 탭으로 들여쓰기를 만나면 변화되기 시작하여 들여쓰지 않은 행을 만날 때 까지 변환이 계속된다.

#### 2.4.1. 들여쓰기
```
This is normal paragraph:

    This is a code block.

end code block.
```
> 실제로 적용하면, 다음과 같이 표현된다.

----

This is normal paragraph:

    This is a code block.

end code block.

----

> 만약 개행을 하지 않으면 인식이 제대로 되지 않는 문제가 발생한다.

----

This is normal paragraph:
    This is a code block.
end code block.

----

#### 2.4.2. 코드블록

코드블록은 다음과 같이 2가지 방식으로 사용된다.

* ```<pre><code>{code}</code></pre>``` 

```
<pre>
<code>
public class Test {
    public static void main(Sring[] args) {
        System.out.println("Hello, Java World");
    }
}
</code>
</pre>
```

----

<pre>
<code>
public class Test {
    public static void main(Sring[] args) {
        System.out.println("Hello, Java World");
    }
}
</code>
</pre>

----

* 코드블록 (" ``` ")

<pre>
<code>
```
public class Test {
    public static void main(Sring[] args) {
        System.out.println("Hello, Java World");
    }
}
```
</code>
</pre>

----
```
public class Test {
    public static void main(Sring[] args) {
        System.out.println("Hello, Java World");
    }
}
```
----

### 2.5. 수평선 ```<hr/>```
아래 줄은 모두 수평선을 마든다. 페이지 나누기 용도로 많이 사용된다.

```
* * *

***

****

- - -

---

----

_ _ _

___

____
```

* * *

***

****

- - -

---

----

_ _ _

___

____

### 2.6. 링크
* 참조링크

```
[link keyword][id]

[id]: URL "링크 설명(선택)"

// code
Link: [Google][googlelink]

[googlelink]: https://google.com "Go google"
```
Link: [Google][googlelink]

[googlelink]: https://google.com "Go google"

* 외부링크
```
사용방법: [Title](Link)
적용예: [Google](https://google.com)
```
[Google](https://google.com)   
[Google](https://google.com, "google link")

* 자동연결

```
일반적인 URL 혹은 이메일주소인 경우 적절한 형식으로 링크를 형성한다.

* 외부링크: <http://eaxmple.com/>
* 이메일링크: <address@example.com>
```

* 외부링크: <http://eaxmple.com/>
* 이메일링크: <address@example.com>

### 2.7. 강조

```
기울기
* *single asterisks*
* _single underscores_

두껍게
* **double asterisks**
* __double underscores__
 
취소선
* ~~canclline~~

밑줄
* <u>underline</u>
```

기울기
* *single asterisks*
* _single underscores_

두껍게
* **double asterisks**
* __double underscores__
 
취소선
* ~~canclline~~

밑줄
* <u>underline</u>

``` 
문장 중간에 사용할 경우에는 *띄어쓰기*를 하는 것이 좋다.
문장 중간에 사용할 경우에는 *띄어쓰기* 를 하는 것이 좋다.
```

문장 중간에 사용할 경우에는 *띄어쓰기*를 하는 것이 좋다.   
문장 중간에 사용할 경우에는 *띄어쓰기* 를 하는 것이 좋다.

## 2.8. 이미지

```
![대체 텍스트](/right/path/to/img.jpg)
![대체 텍스트](/wrong/path/to/img.jpg)
```

![대체 텍스트](/git/마크다운(Markdown)/resource/img/whale.jpg)
![대체 텍스트](/!git/마크다운(Markdown)/resource/img/whale.jpg)

## 2.9. 줄바꿈
3칸이상 띄어쓰기 "   " 또는 ```<br>``` 을 사용하면 줄이 바뀐다.

```
* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸이상을 띄어쓰기 또는 <br>을 해야한다.
이렇게

* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸이상을 띄어쓰기 또는 br>을 해야한다.___ <!-- 띄어쓰기 -->

* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸이상을 띄어쓰기 또는 br>을 해야한다.<br>
```

* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸이상을 띄어쓰기 또는 ```<br>``` 을 해야한다.
이렇게

* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸이상을 띄어쓰기 또는 ```<br>``` 을 해야한다.   
이렇게

* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸이상을 띄어쓰기 또는 ```<br>``` 을 해야한다.<br>
이렇게

## 2.10. 표(Table)

```<table>``` 태그로 변환된다.



--------------------------------------------------------------------------------------------- 
<!------------------------------------------ 절취선 ----------------------------------------->

# 참고 자료
* [참고1](https://gist.github.com/ihoneymon/652be052a0727ad59601)
* [참고2](https://sergeswin.com/1013/)
* [참고3](https://heropy.blog/2017/09/30/markdown/)
* [참고4](https://steemit.com/kr/@inspiredjw/61xdeb)