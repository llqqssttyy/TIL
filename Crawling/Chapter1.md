# 상명대학교 공지사항 크롤러 만들기  

## Chapter 1.
<br/>

## 왜 갑자기 크롤러를?
교수님께서 데이터셋 구축 도구 제작(Chat Prompt 프로젝트)를 제안하실 때 '이번 기회에 크롤링 배워봐~'라며 꼬셨(?)는데  <br/>
여차저차 결국 크롤링은 하지 않고 넘어갔다.  

Chat Prompt 프로젝트 최종 미팅 후에 3일을 퍼져 있다가 이대로는 안돼!! 하는 마음에 <br/>
프론트엔드 혼자서 할 수 있는 토이프로젝트가 없을까.. 생각했고, 해당 프로젝트를 구상했다.  

대학교 1학년 때 들었던 말 중 기억에 남는 것이 있다.  
'대학은 갠플이야. 공지는 네가 찾아봐야 해'  
맞는 말이다. 누가 알려주길 바라면 안되지. 근데.. 봐야 할 사이트가 하나가 아니다. 😢  
대부분의 공지사항은 공식 사이트에 올라오지만 외에도 학과 공지사항은 학과 홈페이지에, 인턴십, 공모전 및 비교과 프로그램은 SW중심사업단 홈페이지에 올라오니  
일단 기본적으로 4개는 봐야 하는 것이다.  
이런 상황에서 바빠서 며칠 정도 공지사항 확인을 못하면 동계 인턴십 프로그램을 놓치는 불상사가 발생하기도 한다(절대 경험담이다)

이런 불편함을 해결하기 위해 학교 공지사항을 크롤링하여 매주 평일 저녁에 이메일로 보내주는 프로그램을 만들기로 했다.

<br>

## 크롤링이란?  
크롤링이랑 web상에 존재하는 contents를 자동적으로 탐색하는 작업이다.  
비슷한 의미의 단어로 웹 스크래핑(web scraping)이 있는데  
특정 웹 사이트나 페이지에서 원하는 데이터를 자동으로 추출해 내는 작업을 말한다.  
내가 구현하고자 하는 프로그램은 스크래핑에 가깝지만  
대중적으로 크롤링이란 단어를 많이 사용하므로 크롤링으로 통일하여 언급할 것이다.  
  
<br>

크롤링에 대해서 처음 알았을 때  아무리 웹사이트에 올라온 정보라지만 이를 수집하는 것이 위법은 아닐까? 하는 걱정이 있었다.  
그러다 [합법적으로 '웹 크롤링'하는 방법(상)](https://yozm.wishket.com/magazine/detail/877/)과 [합법적으로 '웹 크롤링'하는 방법(하)](https://yozm.wishket.com/magazine/detail/878/)를 읽고 걱정을 접을 수 있었다.  
요약하자면 크롤링 자체는 우리가 일반적으로 웹 사이트를 이용하는 프로세스와 같기 때문에 위법행위가 아니다.  
다만 아래의 경우 위법행위로 간주될 수 있으니 주의해야 한다.
1. 수집한 데이터를 상업적으로 이용하는 경우
2. 웹 크롤링을 통해 상대 서버에 문제를 일으킨 경우  

<br/>
위의 경우가 아니더라도, robots.txt에 Disallow로 명시되어 있다면 하지 않는 것이 좋다.  

내가 크롤링하려는 페이지는 robotx.txt를 확인해도 모두 OK였다!
<br/>
<br/>


## 크롤링 방식
크롤링하는 방식은 크게 4가지가 있다.

- 서비스에서 제공하는 오픈 API 활용
- HTML 태그 정보를 기반으로 원하는 내용 부분 수집
- 서버와 브라우저 간의 네트워크를 분석하여 필요한 데이터 수집
- 사람이 브라우저를 직접 동작하는 것처럼 프로그램을 만들어 수집

나는 Javascript로 크롤링할 때 많이 사용되는 `cheerio`라는 라이브러리를 사용할 건데,  
[공식 사이트](https://cheerio.js.org/)에 의하면 cheerio는 HTML 태그를 기반으로 정보를 수집하는 방식을 사용하고 있다.  
<br/>
그렇다면 자연스럽게 cheerio가 뭘까 궁금할 것이다!😎(궁금하다고 해줘요🥹)  
다음 챕터에서는 프로젝트 세팅과 함께 설치해야 할 라이브러리에 대해서 다룰 것이다.
