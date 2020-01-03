# Prologue

##### 버전관리의 필요성과 수동 관리의 한계
소프트웨어를 만드는 것 자체 뿐 아니라, 소프트웨어를 유지/관리하는 것도 중요하다. 또한 초기 개발 단계를 지나 추가적인 개발 단계를 거쳐갈 때, 각 시기에 따라 호환성의 문제가 발생하는 경우가 있다. 이를 위해 "**버전**"을 관리해야 하지 않을까?  
물론 SW를 구성하는 모든 프로젝트의 버전을 하나하나 관리할 수는 있다. 그런데 대규모 프로그램의 경우에는 사실상 모든 프로젝트를 직접 관리하는 것은 정말로 힘들 것으로 생각 된다. ~~공부하고 개발하기도 바쁜데 어떻게 매번 프로젝트 버전만 고치고 있을까~~

##### 자동 버전 관리 플러그인

현재 주로 MicroSoft Visual Studio를 사용하여 개발하고 있기 때문에 MS VS에서 구동 가능한 자동 버전 변경 플러그인을 적용하고자 한다. 소개할 기능은 "**Automatic Versions 2**"이다.  
~~직접 공부하고 직접 적용해보면서 적을 계획이기 때문에 글의 마지막에는 "적용 실패"가 나올 수도 있다는 점이 함정이라면 함정. 현재 사내에서 진행하는 프로젝트가 C++기반 인데,, 이건 C#과 Visual Basic을 위해 만들어졌다고 하니까... 안될 것 같은 느낌적인 느낌이다.~~

# Automatic Versions 2
## 소개
이 프로그램의 Full Name은 **Automatic Versions for Visual Studio** 정도가 될 것 같다. 말 그대로 MS Visual Staudio 프로젝트의 버전을 자동으로 설정하는 **Plugin**이다.  
이 플러그인은 **Full .Net** , **.Net Standard** , **.Net Core** 에 대한 C# 및 VB 프로젝트의 Assembly, AssemblyFile, AssemblyInfo, ClickOne 및 패키지 버전을 자동으로 증가시킨다.

이 문서는 [Visual Studio :: Marketplace :: Automatic Version 2](https://marketplace.visualstudio.com/items?itemName=PrecisionInfinity.AutomaticVersions)를 한국어로 번역한 내용을 담고 있습니다.

## 특징
### 설정 적용
- 프로젝트의 구성(Debug/Release) 수준보다 더 세밀하게 설정 할 수 있다.
- 일반적으로, 솔루션 수준에서 혹은 모든 솔루션의 전역 사용자 기본값에서 구성 할 수 있다.

### 설정 적용 옵션
(설정을 적용 할 수 있는)각 버전 레벨에는 세 가지 옵션을 사용할 수 있다.

![img_설정_옵션](/images/AV2_Setting_type.png)

##### Inherit from Solution
- Inherit / Not Set 두 가지 옵션
- Inherit : 각 버전 타입은 상위 레벨 혹은 전역 기본값으로부터 설정 값을 상속받는다.
- Not Set : 해당 버전 유형으로 작업하지 않도록 함.(Automatic Versions 사용X)
##### Custom System.Version
- **Major.Minor.Bulid.Revision** 형식에 대해 버전 증가
##### Custom Semantic Version (BETA/PRO)
- **Major.Minor.Patch-Prerelease** 형식의 옵션.
- 이 설정은 AssemblyInfo(Full .Net) 및 패키지(.Net Standard / Core)에만 사용 가능

### Version Type
##### Assembly Version
AssemblyInfo.cs./vb 파일에서 AssemblyVersion 속성을 업데이트한다.  
이 값은 .Net에서 **strong naming**이 사용된 경우 로드 할 라이브러리 버전을 판별하는 데 사용된다.
```
// 버전 번호는 다음 코드로 알 수 있다.
var version = System.Reflection.Assembly.GetExecutingAssembly().GetName().Version;
Console.WriteLine(version); // -> "1.1.2.10"
```
> What's the **Strong Named Assembly** ?  
Assembly에 대한 고유한 ID를 만들어 Assembly Crash(어셈블리 충돌)를 방지할 수 있다.  
Assembly와 함께 배포된 Public key에 대응하는 Private key와 Assembly 자체를 사용하여 생성된다. Assembly에는 구성하는 모든 파일의 **이름**과 **Hash가 포함된 Assembly Manifest**가 포함되어 있다. 즉, 같은 Assembly는 같은 strong name을 가지게 된다.  
[Link | MicroSoft :: Strong Named Assembly](https://docs.microsoft.com/ko-kr/dotnet/standard/assembly/strong-named)

> What's the **Assembly Manifest** ?  
모든 Assembly는 정적/동적 여부에 상관없이, 구성 요소들이 서로 어떻게 연관되는지를 설명하는 데이터 컬렉션을 포함한다. 이런 Assembly Metadata는 **Assembly Manifest**에 들어있다.  
Assembly Manifest는 Assembly의 버전 요구 사항과 보안 ID를 지정하는 데 필요한 모든 Metadata, Assembly 범위를 정의하고 리소스나 클래스에 대한 참조를 확인하는 데 필요한 모든 MetaData를 포함한다.  
Assembly Manifest는 MSIL(Microsoft Intermediate Language) 코드가 있는 PE 파일(.exe 또는 .dll)에 저장되거나 Assembly Manifest 정보만 포함하는 독립 실행형 PE 파일에 저장된다.  
[Link | MicroSoft :: Assembly Manifest](https://docs.microsoft.com/ko-kr/dotnet/standard/assembly/manifest)

##### Assembly File Version
AssemblyInfo.cs./vb 파일에서 AssemblyFileVersion 속성을 업데이트한다. 이 값은 운영체제에서 .dll 또는 .exe의 세부 정보 탭에서 "**파일 버전**"으로 사용된다. 예를 들어 기능 기반 API가 이전 버전과의 호환성을 위해 동일한 패치를 제품에 적용하는 경우 파일 버전을 늘리는 것.  
AssemblyVersion은 동일하게 유지하고, 패치가 있는 dll/exe와 패치가 없는 dll/exe를 확인하기 위해 버전을 늘릴 수 있다.
```
// 파일 버전은 다음 코드로 접근 할 수 있다.
var assemblyLocation = System.Reflection.Assembly.GetExecutingAssembly().Location;
var fileVersion = System.Diagnostics.FileVersionInfo.GetVersionInfo(assemblyLocation).FileVersion; 
Console.WriteLine(fileVersion); // -> "1.2.32.2"
```
##### Assembly Info Version
(.Net Framework)  
AssemblyInfo.cs./vb 파일에서 AssemblyImformationalVersion 속성을 업데이트한다. 이 값은 운영 체제에서 .dll 또는 .exe의 세부 정보 탭에서 "**제품 버전**"으로 사용된다.  
이 버전 유형은 Semantic Versioning에 사용할 수 있는 옵션이 있다는 점이 특별하다. 또한, NuGet Publishes 에 사용할 수 있다.  
이 버전 속성에는 제품 이름을 포함 할 수 있는 '접두사 텍스트'를 사용할 수 있다. 접두사 텍스트를 사용하려면 AssemblyInfo 파일에서 정적 접두사 텍스트를 설정하면 자동 버전은 이를 무시하고 버전 번호만 업데이트한다. (Semantic Versioning에서는 허용하지 않기 때문에 사용X)
```
// 다음 코드를 사용해 어세블리 정보 버전에 접근할 수 있다.
var assemblyLocation = System.Reflection.Assembly.GetExecutingAssembly().Location;
var version = System.Diagnostics.FileVersionInfo.GetVersionInfo(assemblyLocation).ProductVersion; 
Console.WriteLine(version); // -> "1.2.32-alpha-01"
```
##### Package Version (BETA/PRO)
(.Net Standard / Core)  
패키지 버전 속성이 업데이트되고 nuget package manifest에 자동으로 사용된다. Semantic Version에서만 사용할 수 있고, 프로젝트에서 이 버전을 얻는 방법은 AssemblyInfoVersion과 동일하다.

##### ClickOnce Version (BETA/PRO)
AssemblyOnce 버전 또는 AssemblyFileVersion과 일치하도록 ClickOnce 버전을 업데이트한다.

## Custom System.Version Options
**Major.Minor.Build.Revision** 형식을 원하는 증가 타입으로 개별적으로 구성 가능  

![img_Custom_System.Version_Options](/images/AV2_Custom_System.Version_Options.png)
- **None** : 증가시키지 않음
- **None w/AutoReset** : 값을 수동으로 설정할 수 있으며, 더 큰 숫자가 증가하지 않는 한 변경되지 않음. 이 경우 0으로 재설정 된다.
- **Increment (Always)** : 항상 값을 1씩 증가시킨다. (예외적으로 증분 시 재설정을 사용하는 경우, 새 버전 빌드를 선택하면 값이 재설정 된다.)
- **Increment w / AutoReset** : 더 큰 숫자가 증가하지 않으면 값을 1씩 증가시킨다. 이 경우 0으로 재설정 됨.
- **On Demand (Build New Version)** : **새 프로젝트 (상황에 맞는)빌드 명령**을 사용하여 빌드를 시작할 때만 값을 1씩 증가. Major 및 Minor 모두 이 값으로 설정하고 상황에 맞는 메뉴를 사용하여 증가시킬 값을 제어할 수 있다. (하단의 사용법 참고)
- **On Demand w / Reset (Obsolete)** : 각 프로젝트 상황에 맞는 메뉴에서 **새 버전 빌드 명령**을 사용하여 빌드 시작할 대만 값을 1씩 증가. 모든 증가 하위 값을 재설정. _레거시 코드, 이 기능은 이후 버전에서는 지원되지 않을 수 있음._
- **Day Of Year (ddd)** : DateTime.UtcNow.DayOfYear (DayOfYear = the current day of the year)
- **Day (dd)** : 현재 달의 날짜 (the current day of the month)
- **Month (MM)** : 올해의 현재 월 (the current month of the year)
- **Year (yyyy)** : 현재 연도 (the current year. ex: 2020)
- **Short Year (yy)** : 2자리 현재 연도 (2 digit current year. ex: 20)
- **Date (yyddd)** : 2자리 현재 연도 + 올해 중 현재 날짜 (where ddd 는 the day of year): 24211.
- **Date (MMdd)** : 2자리 월 + 2자리 일
- **UTC Time (HHmm)** : 현재 시간 (2자리 시간 + 2자리 분)
- **Delta Days (since 1/1/2000)** : 2000년 1월 1일 이후 발생한 일수
- **UTC Seconds Since Midnight/2** : 버전 값에 사용자 정의 고유 시간 기반 스탬프를 사용. 하루 중 시간을 가장 구체적으로 나타낼 수 있다. 이 때 숫자를 2로 나누어야 오버플로우가 발생하지 않는다.

## Custom Semantic Version (BETA/PRO)
**Semantic Versioning**은 일반적으로 매니페스트 파일이나 제품의 'published package'로 사용할 빌드 결과를 'packaging'하는데 사용된다. 그래서 Semantic Versioning은 Major/Minor 번호에 사용할 번호 중 하나를 선택할 수 있도록 함으로써 다른 버전 특성을 활용하고, Patch 및 Pre-Release 번호에 대한 자체 증분 작업을 독립적으로 수행한다.

![img_Custom_Semantic_Version](/images/AV2_Custom_Semantic_Version.png)

- **Major/Minor settings** : AssemblyVersion 이나 AssemblyFileVersion 또는 수동 사용하는 옵션. 수동으로 설정하려면 먼저 **AssemblyInfo file (Full .Net)**이나 **Project Properties (.Net Standard/core)**을 열고 Major/Minor 값을 수동으로 설정해야한다.
- **Patch settings** : 증분 설정에 따라 패치 번호(Patch Number)가 증가한다. **Increment Once**는 'pre-release(시험판)' 버전으로 전환할 때 사용할 수 있으며, 일반적으로 pre-release cycle이 시작할 때 패치 번호가 한 번 증가한다. 패치 번호가 한 번 증가한 후 Patch setting은 None 옵션으로 자동 변경 된다. (pre-release가 증분을 수행)
- **Pre-Release (optional)** : alpha, beta, preview, rc, N/A(release)로 설정. pre-release로 설정하면, 패치가 자동으로 "increment once"로 변경되고, pre-release는 alpha, alpha-01, alpha-02, 등으로 시작해 대신 증가한다. N/A(release)로 설정하면 패치가 자동으로 "Increment w/AutoReset"으로 변경 된다.

## Advanced
- **Event Logging** : 자동 버전 이벤트 기록 활성화/비활성화. (ex : "x에서 y로 업그레이드")
- **AssemblyInfo Location (optional)** : AssemblyInfo 클래스 파일의 사용자 지정 경로 지정. (기본 경로는 속성 폴더 안)
- **Primary Version Type** : None 이외의 옵션으로 설정하면 설정한 속성값이 "AssemblyVersions"라는 MSBuild 속성으로 저장됨. 이를 통해 개발자는 텍스트 파일이나 AssemblyInfo 소스 파일에서 버전 번호를 분석할 필요 없이 사용자 정의 빌드 프로세스를 사용할 수 있다.

## 사용
자동 버전을 구성하려면 **도구 메뉴 > 자동 버전 설정** 으로 이동
프로젝트를 빌드 할 때마다 버전이 자동으로 변경된다.

"On Demand" 버전을 사용하려면 **마우스 오른쪽 버튼 클릭 > 새 버전 구축**에서 모두 "On Demand"설정으로 변경하거나, **Build New Major Version**이나 **Vuild new Minor Versioin**에서 Major나 Minor 각각 "On Demand"설정으로 변경하면 된다.

## 설치
##### 다운로드
[Download | Visual Studio :: Marketplace :: Automatic Version 2](https://marketplace.visualstudio.com/items?itemName=PrecisionInfinity.AutomaticVersions) 에서 다운로드 할 수 있다.
(Visual Studio 공식 홈페이지 - Marketplace - Automatic Versions 2)