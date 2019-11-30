---
layout: default
title: Flutter 설치하기
parent: Flutter
nav_order: 1
---
# Flutter
플러터로 프로젝트를 만들려면 먼저 2가지 준비물이 필요하다.  

1. Flutte SDK
2. IDE (VSCode, AndroidStudio)
  
두가지를 지원하지만 현재 VScode를 사용함으로 VSCode 기준으로 설명한다.  
## Flutter SDK 설치
아직 Flutter SDK를 설치하지 않았다면, 아래 링크에 들어가 플러터 공식 문서 대로 설치하자.

[Flutter SDK Install](https://flutter.dev/docs/get-started/install?source=post_page-----edbe44a178c5----------------------)  

설치를 한 후에 flutter doctor를 실행 해보고 무엇이 빠져있는거 확인해보자.  
만약 환경변수가 안되어있다면 flutter/bin을 등록하면 된다.
![](https://github.com/labyrintos71/labyrintos71.github.io/tree/master/assets/images/flutterdoctor.png)  

## VSCode 플러그인 설치
VSCode 플러그인 탭에 가서 flutter, rainbow brackets을 설치하자.
이후 ctrl + shitf + P 를 누르고 FLutter: New project를 선택한다.  
프로젝트 이름, path를 설치하면 완료된다.