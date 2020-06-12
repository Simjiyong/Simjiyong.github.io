---
title: "ViewModel 개요"
categories: Android
toc: true
toc_sticky: true
toc_label: "ViewModel 개요"
---

# 내가 이해한 ViewModel

[앱 아키텍처 가이드](https://developer.android.com/jetpack/docs/guide)에 따르면 Activity 나 Fragment와 같은 UI 기반 클래스는 UI 및 운영체제 상호작용을 처리하는 로직만 포함해야 한다고 나와있다. 

따라서, UI 관련 데이터를 관리하기 위한 모델을 만들고 이를 이용해 앱의 데이터 처리를 담당해야한다.

UI 상태를 유지하기 위한 옵션에는 

- **ViewModel**
- **Saved instance state**
- **Persistent storage** 

이렇게 3가지가 있다.

그 중 **ViewModel**에 대해서 알아보려고 한다.

## ViewModel 개요

**ViewModel** 클래스는 UI 관련 데이터를 저장하고 관리하도록 설계되었다. **ViewModel** 클래스를 사용하면  UI 데이터에 빠르게 액세스할 수 있으며 회전, 창 크기 조정 및 일반적으로 발생하는 기타 구성 변경 시 네트워크 또는 디스크에서 데이터를 다시 가져오는 것을 피할 수 있다. 

하지만 시스템에 의해 중단되거나 사용자가 Activity나 Fragment를 종료할 때, 개발자가 finish()를 호출할 경우에는 **ViewModel**은 시스템에 의해 자동으로 폐기되기 때문에 UI 관련 데이터를 불러올 수 없다.

즉, **ViewModel**은 사용자가 애플리케이션을 사용하는 동안 UI 관련 데이터를 저장, 관리하는데 사용하는 것이 적합하다.





## ViewModel 간단한 예

```kotlin
// #1
class MainActivity : AppCompatActivity() {

   lateinit var scoreViewModel: ScoreViewModel
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        scoreViewModel = ViewModelProvider(this).get(ScoreViewModel::class.java)
        displayForTeamA(scoreViewModel.getScoreTeamA())
        displayForTeamB(scoreViewModel.getScoreTeamB())
    }
	...
}

// #2
class MainActivity : AppCompatActivity() {

   val scoreViewModel: ScoreViewModel by viewModels()
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        displayForTeamA(scoreViewModel.getScoreTeamA())
        displayForTeamB(scoreViewModel.getScoreTeamB())
    }
    ...
}

```

`scoreViewModel = ViewModelProvider(this).get(socreViewModell::class.java)`

or

`val scoreViewModel: ScoreViewModel by viewModels()`

와 같은 문법으로 **ViewModel**을 적용할 수 있다.

[전체코드](https://github.com/Simjiyong/ViewmodelSampleApp)

