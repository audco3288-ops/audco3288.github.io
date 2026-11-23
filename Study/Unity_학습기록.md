# Unity C# 학습 기록

## 1. 학습 목차 (진행 순서)

1. 유니티 기본 (GameObject, Component, Transform, Hierarchy)
2. C# 문법 기초 (자료형, 연산자, 형변환, 조건문·반복문, 삼항 연산자, `??=`)
3. 객체 지향 (OOP, SOLID, 접근 제한자, 상속과 다형성, 프로퍼티, 이벤트)
4. 자료구조 (List, Dictionary, HashSet, Queue, Stack, 연결 리스트, 선형 리스트)
5. 문자열 (StringBuilder, `$""` 보간)
6. 생명주기와 코루틴 (Awake, OnEnable, Start, FixedUpdate, Update, LateUpdate, OnDisable, OnDestroy)
7. 람다와 클로저
8. 좌표와 Transform (GetChild, Find)
9. 물리·충돌·카메라·입력 (Rigidbody, Raycast, OnCollision/OnTrigger, Enter/Stay/Exit)
10. 컴포넌트 제어, 11. UI(ScrollRect, Button), 12. 애니메이션(Animator)
13. 리소스, 14. 네임스페이스와 자료구조 활용
15. 프리팹과 프로젝트 구조 (Prefab, Prefab Variant, ScriptableObject, 씬 전환, 머티리얼, 파티클, 오디오)
16. 저장과 외부 라이브러리 (PlayerPrefs, iTween, DOTween)
17. 입력 시스템 (New Input System, Player Input, `OnMove(InputValue)`)
18. 기타 (디버깅, Git)

## 2. 학습 내용 요약

### 2-1. 생명주기와 구조
- `Awake → OnEnable → Start → FixedUpdate → Update → LateUpdate → OnDisable → OnDestroy` 순서로 호출됩니다.
- 프리팹은 설계도, 인스턴스는 완성품입니다. 원본 프리팹을 수정하면 모든 인스턴스에 반영됩니다.
- ScriptableObject는 여러 오브젝트가 공유하는 데이터를 담을 때 사용합니다.

### 2-2. 상속과 다형성
- `virtual`(부모에서 재정의 허용), `override`(자식에서 재정의), `base`(부모 기능 호출)를 사용합니다.
- `Pet dog1 = new Dog();`에서 `dog1`은 `Pet` 타입이므로 `Dog`에만 있는 `Chase()`는 호출할 수 없습니다.

### 2-3. 자료형과 참조
- 값 타입(int, float, bool 등)과 참조 타입(class, string 등)의 차이를 학습했습니다.
- 자료구조는 용도에 맞게 선택합니다. 순서가 중요하면 List, 키로 찾으면 Dictionary, 중복을 막으려면 HashSet을 씁니다.

### 2-4. 스크롤뷰 제어 (실습)
- `ScrollRect.horizontalNormalizedPosition`(0.0 ~ 1.0)으로 가로 위치를 제어합니다.
- 자동 슬라이드는 `Update()`에서 `slideSpeed * Time.deltaTime`만큼 이동시키고, 끝(1.0)에 닿으면 `(currentIdx + 1) % cnt`로 다음 화면을 고릅니다.
- 버튼 클릭 이벤트는 `for`문에서 `int tmp = i;`로 값을 고정해 클로저 문제를 막습니다.

### 2-5. 충돌
- `OnCollision`은 물리적으로 부딪히는 충돌, `OnTrigger`는 통과하며 감지만 하는 충돌입니다.
- 시점은 `Enter`(처음 닿음), `Stay`(닿아 있는 동안), `Exit`(떨어짐)으로 구분합니다.

### 2-6. 플레이어 체력 구조 (실습)
- 체력은 한 곳(Status 또는 PlayerStatus의 TakeDamage)에서만 변경하도록 권장했습니다.
- 파일명·클래스명·참조명을 일치시켜야 컴파일과 연결 오류가 나지 않습니다.

## 3. 질문과 답변 정리

| 질문 | 답변 요약 |
|---|---|
| GetChild와 Find의 차이 | GetChild는 바로 아래 자식을 순서(index)로 가져오고, Find는 경로 이름으로 찾습니다. Find는 이름이 바뀌면 null이 됩니다. |
| cnt, tmp, idx의 역할 | cnt는 자식 총 개수, tmp와 idx는 버튼·화면 번호를 담는 변수입니다. |
| 클로저 문제 | 람다가 `i`를 참조하면 실행 시점의 최종 값이 들어갑니다. 복사본 `tmp`로 해결합니다. |
| 프리팹과 인스턴스 | 프리팹은 원본, 인스턴스는 씬에 배치한 사본입니다. 독립적으로 동작합니다. |
| 머티리얼을 코드로 바꾸기 | `GetComponent<Renderer>().material = newMaterial;` |
| 3D 사운드 설정 | Spatial Blend를 3D로, Min/Max Distance와 Volume Rolloff를 조정합니다. Audio Listener가 필요합니다. |
| PlayerPrefs | 로컬 저장소입니다. SetInt/SetFloat/SetString으로 저장하고 GetInt 등으로 불러옵니다. |
| iTween | `iTween.Hash()`로 파라미터를 전달합니다. 최신 프로젝트에서는 DOTween을 많이 씁니다. |
| "unassigned local variable" 오류 | 조건문에서 값이 할당되지 않은 경로가 있을 때 발생합니다. 초기값이나 else를 추가해 해결합니다. |
| "Can't add script component" 오류 | 파일명과 클래스명이 다르거나 컴파일 오류가 있을 때 발생합니다. |
| 같은 이름 클래스 중복 오류 | 같은 이름의 스크립트 파일이 두 개 이상 있을 때 발생하며, 하나로 정리합니다. |

## 4. 학습노트 안내

- 최신 학습노트는 `Unity_CSharp_학습노트_최신.md`(재구성된 B안 목차, rev 32 기준)입니다.
- 이 파일은 같은 폴더에 함께 저장하는 것을 권장합니다.
