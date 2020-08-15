---
title: "멀티 스레딩 & 콜백 기본"
categories: Android
toc: true
toc_sticky: true
toc_label: "멀티스레딩 & 콜백"
---

# Multi-threading & callbacks primer

모바일 장치에는 프로세서가 있으며 요즘 대부분의 장치에는 각각 동시에 프로세스를 실행하는 여러 하드웨어 프로세서가 있다. 이를 **multiprocessing** 이라고 한다.

프로세서를 보다 효율적으로 사용하기 위해 운영체제는 응용 프로그램이 프로세스 내에서 둘 이상의 실행 스레드를 만들 수 있도록 할 수 있다. 이를 **multi-threading** 이라고 한다.

<img src="https://developer.android.com/courses/extras/images/multi-threading-1.png" alt="image" style="zoom: 67%;" />

한 번에 여러 권의 책을 읽고, 각 장이 끝날 때마다 책을 번갈아 가며 결국 모든 책을 읽는다고 생각할 수 있지만, 동시에 한 권 이상의 책을 읽을 수는 없다.

이러한 모든 스레드를 관리하려면 infrastructure가 필요하다.

![image](https://developer.android.com/courses/extras/images/multi-threading-2.png)

스케줄러는 우선 순위와 같은 사항을 고려하고 모든 스레드가 실행되고 완료되도록한다. 책이 선반에서 먼지가 쌓이도록 하지 않지만 책이 너무 길거나 기다릴 수 있는 경우 선택되기 까지 시간이 걸릴 수 있다. (starvation 방지?)

디스패처는 스레드를 설정한다. 즉, 읽어야하는 책을 보내고 그에 대한 context를 지정한다. context를 별도의 특수한 reading room으로 생각할 수 있다. 일부 context는 UI 작업에 가장 적합하고 일부는 I/O 작업을 처리하는데 특화되어 있다.

사용자용 application은 보통 main 스레드가 있다. main 스레드는 포그라운드에서 실행되고 백그라운드에서 실행될 수 있는 다른 스레드를 디스패치 할 수 있다.

Android에서 main 스레드는 UI에 대한 모든 업데이트를 처리하는 single 스레드 이다. UI 스레드라고도 하는 main 스레드는 모든 클릭 핸들러와 기타 UI 및 라이프 사이클 콜백을 호출하는 스레드이기도 하다. UI 스레드는 default 스레드다. 앱이 명시적으로 스레드를 전환하거나 다른 스레드에서 실행되는 클래스를 사용하지 않는 한 앱에서 수행하는 모든 작업은 main 스레드에 있다.

UI 스레드는 훌륭한 UX를 보장하기 위해 원활하게 실행되어야한다. 앱이 눈에 띄는 일시 중지 없이 사용자에게 표시되려면 main 스레드가 16ms이상마다 또는 초당 약 60프레임으로 화면을 업데이트해야한다. 이 속도에서 인간은 프레임변경이 매끄럽다고 인식한다. 많은 프레임과 적은 시간이 필요하기 때문에 Android에서는 UI 스레드를 차단하지 않는 것이 중요하다.  차단한다는 것은 데이터베이스와 같은 것이 업데이트를 완료할 때까지 기다리는 동안 UI 스레드가 아무 작업도 하지 않음을 의미한다.

<img src="https://developer.android.com/courses/extras/images/multi-threading-3.png" alt="image" style="zoom: 67%;" />

인터넷에서 데이터 가져오기, 대용량 파일 읽기 또는 데이터베이스에 데이터 쓰기와 같은 작업은 16ms 이상 걸린다. 따라서 main 스레드에서 작업을 수행하기 위해 코드를 호출하면 앱이 일시 중지되거나 끊기거나 멈출 수 있다. 그리고 메인 스레드를 너무 오랫동안 차단하면 앱이 충돌하고 "Application not responding"(ANR)을 표시 할 수 도 있다.



## Callbacks

메인 스레드에서 작업을 완료하는 방법에 대한 몇가지 옵션이 있다.

main 스레드를 차단하지 않고 장기 실행 작업을 수행하는 한 가지 패턴은 **callback**이다. callback을 사용하면 백그라운드에서 장기 실행 작업을 시작할 수 있다. 작업이 완료되면 argument로 제공된 callback이 호출되어 main 스레드의 코드에 결과를 알린다.

**Callback**은 몇 가지 단점이 있다. callback을 많이 사용하는 코드는 읽기 어렵고 코드를 추론하기가 더 어려워질 수 있다. 코드가 sequential한 것처럼 보이지만 callback 코드는 나중에 비동기 시간에 실행되기 때문이다. 또한 callback은 exception과 같은 일부 언어 기능의 사용을 허용하지 않는다.



## Coroutines

코틀린에서 코루틴은 장기 실행작업을 우아하고 효율적으로 처리하기위한 솔루션이다. 코틀린 코루틴을 사용하면 콜백 기반 코드를 순차 코드로 변환할 수 있다. 순차적으로 작성된 코드는 일반적으로 읽기가 더 쉽고 exception과 같은 언어 기능을 사용할 수도 있다. 결국 코루틴과 콜백은 같은 일을 한다. 오래 실행되는 작업에서 결과가 나올 때까지 기다렸다가 계속 실행한다.

[출처]
Multi-threading & callbacks primer, Android Developers, 2020년08월15일 접속, https://developer.android.com/courses/extras/multithreading#callbacks