---
layout: post
title: Qtum 코드 분석을 위한 IDE셋팅
categories: Qtum
tags: BlockChain 
---


###Qtum 코드 분석을 위한 IDE 셋팅

---
환경

  os:ubuntu 18.04 (VM)
  
  ide: Clion
  
  
  
---
시작전 Qtum manual에 있는 방법으로 빌드 환경 구성합니다.

1.step

https://github.com/qtumproject/qtum 에가 서 퀀텀 소스를 포그 해옵니다.

포크후 
Git에서 클론을 받습니다.
```bash
 $ git clone https://github.com/heejoonshin/qtum.git --recursive
 
 ```
 
2.step
Qtum은 autotools로 빌드환경 설정이 되어있습니다.
```bash
 $ ./autogen.sh
 $./configure
 ```
 명령어를 입력하면 Makefile이 생성됩니다.
 이 make파일로 Clion에서 Debug모드로 컴파일을 할 수 있습니다.
 
3.step
Makefile이 생성된 것을 확인한 후
Clion을 실행시킨 후 Import Project메뉴를 선택 하여 qtum 프로젝트를 열면 자동으로 Cmake가 작성됩니다.

![qtum_setting_import.PNG]({{ site.url }}/assets/qtum_setting_import.PNG)

해당 화면이 보이면 Ok버튼 누르면 Clion 자동으로 인덱스를 생성하면서 Cmake파일을 생성합니다.


3.step

불행하게도 Clion이 생성한 Cmake파일로 빌드가 되지 않않습니다. 2번째 스텝에서 만들어 주었던 MakeFile로 빌드 할 수 있도록 Cmake파일을 수정해야 합니다.

첫 번째로 add_executable에 있는 항목을 제거하고 
```cmake
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
add_custom_target(qtum COMMAND make -C ${qtum_SOURCE_DIR} -j 6
        CLION_EXE_DIR=${PROJECT_BINARY_DIR})
```
다음의 내용을 추가합니다.

위 내용은 [Clion에서 make파일이용해 빌드하는 방법](https://stackoverflow.com/questions/26918459/using-local-makefile-for-clion-instead-of-cmake) 을 참고 하였습니다.

위 과적을 거친후 Clion이 CmakeLists를 리로드 하면서 빌드를 하는 것을 볼 수 있습니다.

4.step

실행 할 수 있도록 Working directory와 Executable을 설정해줍니다.

![qtum_setting_config.PNG]({{ site.url }}/assets/qtum_setting_config.PNG)

Wroking directory는 해당 qtum project를 적어 주면 되고, Executable은 프로젝트 하위에 src의 qtumd를 찾아 넣어주면 됩니다.


5.finish

드디어 qtum을 디버그 할 수 있는 환경이 셋팅 되었습니다.

![qtum_setting_debug.png]({{ site.url }}/assets/qtum_setting_debug.png)

디버그 하는 모습입니다.
위에 test를 찍은 이유는 퀀텀이 제대로 동작하는지 확인 해보기 위해 제가 작성한 코드입니다.

---

다음 포스트에는 퀀텀을 분석한 내용을 올리 차례대로 올리겠습니다.
