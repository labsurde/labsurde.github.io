---
layout: post
title: "Trying Sourcetrail, a Tool to visualize C++ source code"
date:   2021-01-21 17:00:00 +0900
categories: C++
---

SOLID에 충실한 20줄 미만의 많은 자잘한 클래스가 나오는 코드의 경우
나의 분석 능력이 많이 떨어진다는 것을 알게 되었다. 뭔가 visual 하게 볼 수 있으면 더 빠르게 분석할 수 있지 않을까? Doxygen은 뭔가 끊어지는 부분이 많아 좀 불만족스러워서 (옵션을 다르게 주면 더 좋아질지도 모르겠지만) 다른 툴을 찾아보게 되었다.

# Sourcetrail
- GNU 3 라이센스이다.
- 설명 비디오 https://youtu.be/Cfu6f0uyzc8 에 보면 UML 은 아니고, 뭔가 object/class 와 sequence diagram의 복합인듯한 모습니다. 다이어그램을 누르면 코드도 보여주는게 괜찮아보인다.

## 설치
- https://github.com/CoatiSoftware/Sourcetrail/releases 에서 다운받아 설치.
- https://www.sourcetrail.com/documentation/#GettingStarted 를 따라한다.
- C++ 을 선택하고 두가지 중 하나로 한다.
  - CMake 프로젝트인 경우 `CMAKE_EXPORT_COMPILE_COMMANDS` flag 를 사용해서 `compile_commands.json` 생성해서 하는 방법
  - 또는 empty project를 선택해서 수작업으로 소스 디렉토리, include 디렉토리 등을 넣는 방법.
- 야심한 밤이라, 귀찮아서 후자로 일단 해봤다. global include directory 등도.. Linux 프로젝트인데 지금 Windows 환경이라 패스.
- source code는 회사에서 작년에 진행했던 아래 프로젝트를 사용.
  - https://github.com/Samsung/ONE
- Create 를 했더니 indexing을 하고 에러가 막 나오는데, (예를 들어 헤더를 못찾겠어.. 등) 걍 무시.
- 에러가 나는 클래스나 파일들은 제대로 로드하지 못하는데, 예를 들어 <cassert> 를 못찾겠어... 뭐 이런 에러들이다. 제대로 하면 다 로드될 것 같다.

## 스크린샷
<img src="/20210121-01.png">

## 장점
- dependency 가 매우 잘 나타나고 코드레벨에서도 잘 보여준다.
- 코드가 같이 보이는것 참 좋은 거 같다.
- 대신 그림 부분이 확확 바뀌어 좀 정신이 없긴하다. But, backspace 를 누르면 이전 화면으로 간다.
- 내일 Linux 용으로 받아서 다시 해봐야 겠다.
