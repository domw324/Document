# Prologue

##### 버전관리의 필요성과 수동 관리의 한계
소프트웨어를 만드는 것 자체 뿐 아니라, 소프트웨어를 유지/관리하는 것도 중요하다. 또한 초기 개발 단계를 지나 추가적인 개발 단계를 거쳐갈 때, 각 시기에 따라 호환성의 문제가 발생하는 경우가 있다. 이를 위해 **"버전"**을 관리해야 하지 않을까?

물론 SW를 구성하는 모든 프로젝트의 버전을 하나하나 관리할 수는 있다. 그런데 대규모 프로그램의 경우에는 사실상 모든 프로젝트를 직접 관리하는 것은 정말로 힘들 것으로 생각 된다. ~~공부하고 개발하기도 바쁜데 어떻게 매번 프로젝트 버전만 고치고 있을까~~

##### 자동 버전 관리 플러그인

현재 C++개발자로서 주로 MicroSoft Visual Studio를 사용하여 개발하고 있기 때문에 MS VS에서 구동 가능한 자동 버전 변경 플러그인을 적용하고자 한다. 소개할 기능은 **Automatic Versions 2**이다. ~~직접 공부하고 직접 적용해보면서 적을 계획이기 때문에 글의 마지막에는 "적용 실패"가 나올 수도 있다는 점이 함정이라면 함정.~~

# Automatic Versions 2
## 소개
이 프로그램의 Full Name은 **Automatic Versions for Visual Studio** 정도가 될 것 같다. 말 그대로 MS Visual Staudio 프로젝트의 버전을 자동으로 설정하는 **Plugin**이다.

이 플러그인은 **Full .Net** , **.Net Standard** , **.Net Core** 에 대한 C# 및 VB 프로젝트의 Assembly, AssemblyFile, AssemblyInfo, ClickOne 및 패키지 버전을 자동으로 증가시킨다.

## 특징
### 설정 적용
- 프로젝트의 구성(Debug/Release) 수준보다 더 세밀하게 설정 할 수 있다.
- 일반적으로, 솔루션 수준에서 혹은 모든 솔루션의 전역 사용자 기본값에서 구성 할 수 있다.

### 설정 적용 옵션
설정을 적용 할 수 있는 각 레벨에는 세 가지 옵션을 사용할 수 있다.

![설정 옵션](/images/AV2_Setting_type.png)

##### Inherit from Solution
- Inherit / Not Set 두 가지 옵션
- Inherit : 각 버전 타입은 상위 레벨 혹은 전역 기본값으로부터 설정 값을 상속받는다.
- Not Set : 해당 버전 유형으로 작업하지 않도록 함.(Automatic Versions 사용X)
##### Custom System.Version
- **Major.Minor.Bulid.Revision** 형식에 대해 버전 증가
##### Custom Semantic Version (BETA/PRO)
- **Major.Minor.Patch-Prerelease** 형식의 옵션.
- 이 설정은 AssemblyInfo(Full .Net) 및 패키지(.Net Standard / Core)에만 사용 가능


## 설치
##### 다운로드
[Visual Studio | Marketplace : Automatic Version 2 Download](https://marketplace.visualstudio.com/items?itemName=PrecisionInfinity.AutomaticVersions) 에서 다운로드 할 수 있다.
(Visual Studio 공식 홈페이지 - Marketplace - Automatic Versions 2)