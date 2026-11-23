# Unity C# 학습 노트 재구성안 (B안 목차)

주제별로 장을 다시 묶은 목차입니다. 옛 번호는 현재 합본 문서의 위치를 나타냅니다. 중복된 내용은 한 장에만 남깁니다.

## 1. 유니티 기본

### 1-1. 구성 요소

- **GameObject**: 씬에 있는 모든 객체(캐릭터, 카메라, 조명, UI, 사운드 등)의 기본 단위입니다.
- **Component**: 오브젝트에 붙여 기능(충돌, 이동, 렌더링 등)을 주는 부품입니다. C# 스크립트도 컴포넌트입니다.
- **Transform**: 모든 오브젝트가 기본으로 가지며, 위치(Position), 회전(Rotation), 크기(Scale)를 담습니다.
- **Hierarchy(하이어라키)**: 씬 안 오브젝트의 계층 구조를 보여 주는 창입니다.
- **Inspector(인스펙터)**: 선택한 오브젝트의 속성과 컴포넌트 값을 설정하는 창입니다.

### 1-2. 스크립트 기본 구조

- **MonoBehaviour**: 유니티 엔진과 연동되는 최상위 클래스입니다. 새 스크립트에는 기본으로 `: MonoBehaviour`가 붙습니다.
- **Class(클래스)**: 변수와 함수를 묶은 설계도입니다. 스크립트 파일명과 클래스 이름이 같아야 합니다.
- **Variable(변수)**: 데이터를 담는 메모리 공간입니다. 예: `int score = 100;`
- **Function(함수)**: 특정 동작을 하나로 묶은 코드 블록입니다.

#### 함수와 return

- `return`은 함수를 끝내고 **호출한 곳으로 값을 돌려주는 것**입니다. 처음으로 돌아가거나 앞의 작업을 취소하지 않습니다.
- `void` 함수에서도 `return;`만 써서 중간에 끝낼 수 있습니다.

```csharp
int Add(int a, int b)
{
    return a + b;   // 결과를 호출한 곳에 돌려줌
}

int result = Add(2, 3);   // result에 5가 들어감
```

#### 사용자 함수와 Unity 이벤트 함수

- **Unity 이벤트 함수**(`Start`, `Update` 등)는 Unity가 특정 시점에 자동으로 호출합니다.
- **사용자 함수**(`Init`, `Setup` 등)는 개발자가 이름을 붙이고, 누군가 직접 호출해야 실행됩니다.

```csharp
public void Init()
{
    hp = maxHp;                 // 시작 값 설정
    isDead = false;             // 상태 초기화
}
```

#### this

- `this`는 **지금 실행 중인 객체 자신**을 가리킵니다.
- 필드와 매개변수 이름이 같을 때 필드를 지정하는 데 씁니다.

```csharp
public void Init(RaidGame _game)
{
    this._game = _game;   // 왼쪽은 필드, 오른쪽은 매개변수
}
```

- 상위 개념과 연결하는 키워드가 아니라 **자기 자신을 가리키는 이름표**입니다.

### 1-3. 프리팹과 인스턴스

프리팹은 15장에서 자세히 다룹니다. 여기서는 기본 개념만 봅니다.

- **프리팹**: 원본 틀(설계도)이며, Project 창의 에셋입니다.
- **인스턴스**: 틀에서 찍어낸 실제 오브젝트이며, 씬에 여러 개 둘 수 있고 서로 독립적입니다.
- 원본 프리팹을 고치면 모든 인스턴스에 반영됩니다.

## 2. C# 기초 문법

### 2-1. 변수와 자료형

- **int**: 정수 (예: -1, 0, 2)
- **long**: 정수 중 범위가 가장 넓은 자료형입니다. 큰 숫자 뒤에 `L`을 붙입니다 (예: `9000000000L`).
- **float**: 실수. 숫자 뒤에 `f`를 붙입니다 (예: `3.14f`). Unity의 위치와 속도 계산은 대부분 float을 씁니다.
- **double**: 실수. float보다 범위와 정밀도가 크고, 뒤에 `f`를 붙이지 않습니다 (예: `3.14`). 메모리를 두 배 쓰므로 정밀도가 꼭 필요할 때만 씁니다.
- **char**: 문자 한 개를 담습니다. 작은따옴표로 씁니다 (예: `'A'`). 큰따옴표로 감싼 `"A"`는 문자열(string)입니다.
- **string**: 문자열 (예: `"안녕하세요"`)
- **bool**: `true` 또는 `false`
- **Vector2 / Vector3**: 2차원, 3차원 좌표나 방향 (Unity 타입)
- 선언: `자료형 변수명;` (예: `int a;`)

#### long과 정수 변환

- **long**은 정수형 중 저장 범위가 가장 넓습니다(약 ±922경). 큰 숫자 뒤에 `L`을 붙입니다.
- `int`를 `long`에 넣는 것은 자동 변환이므로 `parse`가 필요 없습니다.
- `long`을 `int`에 넣을 때는 값이 잘릴 수 있으니 `(int)` 캐스팅 전에 범위를 확인합니다.
- 두 `int`를 곱한 뒤 `long`에 담으면 이미 `int` 범위에서 깨집니다. `(long)a * b`처럼 먼저 `long`으로 바꿔 계산합니다.
- `parse`는 **문자열을 숫자로 바꿀 때만** 씁니다. `PlayerPrefs`에는 `GetLong`이 없으므로 문자열로 저장한 뒤 파싱합니다.
- Inspector의 `int` 필드를 `long`으로 바꾸면 기존 값이 초기화될 수 있으니 다시 입력합니다.

### 2-2. 연산자

- **산술**: `+`, `-`, `*`, `/`, `%`(나머지)
- **대입**: `=`는 저장, `a += b`는 a에 b를 더해 다시 저장, `a *= 2`는 2를 곱해 저장
- **증감**: `a++`는 1 증가, `a--`는 1 감소
- **관계**: `==`(같다), `!=`(다르다), `>`, `<`. `=`는 대입이고, 같은지 비교할 때는 `==`를 씁니다.
- **논리**: `&&`(그리고), `||`(또는), `!`(반대)
- **삼항 연산자**: `A = B > C ? 1 : 2;` (B가 C보다 크면 A는 1, 아니면 2)
- **null 병합**: `a ?? b`는 a가 null이면 b를 선택합니다. `a ??= b`는 a가 null일 때 a에 b를 저장합니다.

#### 나머지 연산자 `%`

- `%`는 나눈 뒤 **남는 값**을 구합니다. `10 % 3`은 `1`입니다.
- 짝수와 홀수 판별, 순환 번호, 일정 간격마다 실행하는 곳에 씁니다.

```csharp
currentIdx = (currentIdx + 1) % 3;   // 0 → 1 → 2 → 0 순환
```

- 음수에서는 결과의 부호가 왼쪽 값을 따릅니다. `-7 % 3`은 `-1`입니다. 항상 양수가 필요하면 `((x % n) + n) % n`처럼 계산합니다.
- 0으로 나누면 `DivideByZeroException`이 발생합니다.

### 2-3. 조건문과 반복문

- **if / else if / else**: 조건에 따라 실행할 코드를 고릅니다.
- **while**: 조건이 참인 동안 반복합니다.
- **for**: 횟수를 정해 반복합니다.
- **foreach**: 배열이나 리스트의 모든 요소를 차례로 꺼냅니다.

```csharp
for (int a = 0; a < 10; a++)   // 0부터 9까지 총 10번
{
}
```

#### foreach의 동작 원리

- `foreach (string s in stack)`에서 `s`는 값 하나를 담는 **임시 이름표**입니다. Stack의 성질을 가지지 않습니다.
- 순서는 자료구조가 정합니다. Stack은 맨 위부터, Queue는 앞부터, List는 넣은 순서대로입니다.
- `foreach`는 값을 **보기만** 하고 꺼내지 않습니다. 꺼내려면 `Pop`이나 `Dequeue`를 씁니다.
- 순회 도중 컬렉션의 요소를 추가하거나 지우면 오류가 납니다. 지워야 할 때는 뒤에서부터 도는 `for`문을 씁니다.

### 2-4. 배열

- **정의**: 같은 타입의 변수를 여러 개 모아 저장하는 자료형입니다. 인덱스는 **0부터** 시작합니다.
- **선언**: `int[] idxArr = new int[5];`
- **초기화**: `int[] idxArr = new int[] { 1, 2, 3, 4, 5 };` 또는 `int[] idxArr = { 1, 2, 3, 4, 5 };`
- **`[]`와 `{}`**: `[]`는 배열 공간의 형태를, `{}`는 그 안에 들어갈 값을 정합니다.
  - `new GameObject[] { a, b }`처럼 `[]`를 비우면 값 개수에 맞춰 크기가 정해집니다.
  - `new GameObject[3]`처럼 숫자를 쓰면 값이 없는 빈 슬롯 3개(null)를 먼저 만듭니다.
- **실무 예시**: 인벤토리나 장착 슬롯처럼 크기는 고정하고 내용만 바뀌는 데이터에 씁니다. 한 개만 장착하는 부위는 단일 변수가 효율적입니다.

#### 배열과 Stack의 차이

- 배열은 `arr[2]`처럼 번호로 아무 위치나 접근합니다.
- Stack은 맨 위에서만 넣고 뺍니다. 번호로 접근하는 기능이 없습니다.
- C#의 `Stack<T>`는 내부적으로 배열을 쓰지만, 사용자는 인덱스로 접근할 수 없도록 규칙만 노출합니다.

### 2-5. 열거형 (enum)

열거형은 관련된 이름들을 모아 두고 그중 하나를 고르는 타입입니다.

```csharp
public enum MonsterType { Goblin, Slime, Orc }

MonsterType type = MonsterType.Goblin;
```

- 정해진 이름 중에서만 고를 수 있어 잘못된 값이 들어가기 어렵습니다.
- 내부적으로는 0, 1, 2 같은 정수로 저장되지만, 코드에서는 이름으로 씁니다.
- 번호를 직접 정할 수 있습니다: `enum Size { Small = 1, Medium = 5, Large = 10 }`
- 값을 중간에 끼워 넣으면 숫자로 저장된 데이터가 어긋날 수 있으므로, 추가는 뒤에 합니다.
- 여러 상태를 조합할 때는 `[System.Flags]`와 2의 거듭제곱 값을 씁니다.

```csharp
[System.Flags]
public enum Status { None = 0, Poison = 1, Burn = 2, Freeze = 4 }

Status s = Status.Poison | Status.Burn;
bool isPoisoned = (s & Status.Poison) != 0;
```

## 3. 객체 지향

### 3-1. OOP 개요

**객체 지향 프로그래밍(OOP)** 은 데이터와 그 데이터를 다루는 동작을 **객체**라는 하나의 단위로 묶어 프로그램을 만드는 방식입니다.

| 개념 | 뜻 | 예 |
| --- | --- | --- |
| 클래스 | 객체의 설계도 | `Monster`, `MonsterData` |
| 객체 | 설계도로 만든 실체 | 씬의 "Goblin(Clone)" |
| 상속 | 공통 기능을 물려받음 | `Enemy` → `Boss` |
| 다형성 | 같은 이름의 함수를 객체마다 다르게 실행 | `Attack()`이 객체마다 다르게 동작 |
| 캡슐화 | 내부 데이터를 숨기고 필요한 기능만 공개 | `private` 필드와 `public` 메서드 |

Unity는 `GameObject`에 컴포넌트를 붙이는 구조이므로 OOP 생각과 잘 맞습니다.

### 3-2. SOLID 원칙

객체 지향 설계에서 코드를 고치기 쉽고 확장하기 쉽게 만드는 다섯 가지 원칙입니다. 정답 규칙이라기보다 **코드를 점검하는 질문 목록**으로 생각하면 충분합니다.

| 약어 | 원칙 | 핵심 질문 |
| --- | --- | --- |
| S | 단일 책임 원칙 (SRP) | 이 클래스를 고치는 이유가 하나인가? |
| O | 개방-폐쇄 원칙 (OCP) | 기능을 추가할 때 기존 코드를 고쳐야 하는가? |
| L | 리스코프 치환 원칙 (LSP) | 자식을 부모 자리에 넣어도 규칙을 어기지 않는가? |
| I | 인터페이스 분리 원칙 (ISP) | 쓰지 않는 기능을 억지로 구현하고 있지 않은가? |
| D | 의존 역전 원칙 (DIP) | 구체적인 클래스에 직접 묶여 있지 않은가? |

#### 단일 책임 원칙 (SRP)

한 클래스는 **고칠 이유가 하나**여야 합니다. 이동, 피격, 저장, UI를 모두 맡은 `Player`는 세이브 형식만 바뀌어도 고쳐야 합니다.

```csharp
public class PlayerMover { /* 이동만 */ }
public class PlayerHealth { /* 체력과 피격만 */ }
public class PlayerSaver { /* 저장만 */ }
```

- 잘게 나누기만 하면 오히려 복잡해지므로, 책임의 단위는 프로젝트 규모에 맞춥니다.

#### 개방-폐쇄 원칙 (OCP)

새 기능은 **기존 코드를 고치지 않고 새 클래스를 추가해서** 붙입니다.

```csharp
public abstract class Monster { public abstract void Attack(); }
public class Goblin : Monster { public override void Attack() { /* ... */ } }
public class Orc : Monster { public override void Attack() { /* ... */ } }
```

#### 리스코프 치환 원칙 (LSP)

자식 객체는 부모 타입 자리에 넣어도 **부모의 규칙을 어기지 않아야** 합니다. 부모가 음수를 허용하는데 자식이 예외를 던지면 이 원칙을 어깁니다.

#### 인터페이스 분리 원칙 (ISP)

한 인터페이스에 여러 기능을 몰아넣지 말고 작게 나눕니다. 쓰지 않는 메서드를 빈 구현으로 채워야 한다면 인터페이스가 너무 큰 것입니다.

```csharp
public interface IMovable { void Move(); }
public interface IJumpable { void Jump(); }
```

#### 의존 역전 원칙 (DIP)

구체 클래스 대신 인터페이스에 의존하고, 실제 구현은 바깥에서 넣어 줍니다.

```csharp
public interface ISaver { void Save(GameData data); }

public class Game
{
    private ISaver saver;
    public Game(ISaver saver) { this.saver = saver; }
}
```

- Inspector에서 연결하는 방식도 "구체 대상을 바깥에서 넣어 주는" 생각과 닮아 있습니다.

### 3-3. 접근 제한자

| 구분 | public | private |
| --- | --- | --- |
| 외부 스크립트 접근 | 가능 | 불가능 |
| 인스펙터 노출 | 노출됨 | 숨겨짐 |
| 주요 목적 | 외부 접근이나 에디터 연결 | 내부 처리와 값 보호 |

- **권장**: 변수는 기본을 `private`으로 두고, 외부 공개가 필요할 때만 `public`으로 바꿉니다.
- **\[SerializeField\]**: 외부에는 숨기고 인스펙터에서만 연결하고 싶을 때 씁니다.

```csharp
[SerializeField] private GameObject btn0;   // 외부 비공개 + 인스펙터 연결 가능
```

### 3-4. 상속과 다형성

부모 클래스의 변수와 함수를 물려받아 자식 클래스를 만듭니다. 공통 기능은 부모에 한 번만 쓰고, 자식은 차이점만 적습니다.

```
public class Enemy : MonoBehaviour
{
    public virtual void Attack() { Debug.Log("기본 공격"); }
}

public class Boss : Enemy
{
    public override void Attack() 
{ base.Attack(); Debug.Log("보스 공격"); }
}

Enemy e = new Boss();
e.Attack();   // 출력: 기본 공격 → 보스 공격
```

#### 매뉴얼 비유

- **부모(Enemy)**: 기본 몬스터 매뉴얼
- **virtual**: "이 항목은 고쳐 써도 됩니다"라는 표시
- **override**: 자식이 그 항목을 자기 방식으로 덮어씀. 부모 매뉴얼 자체는 바뀌지 않음
- **base.함수()**: 원래 매뉴얼의 내용을 먼저 실행하고 자식 동작을 이어 붙임
- **다형성**: 부모 타입으로 선언해도 실제 객체가 자식이면 자식의 함수가 실행됨

#### 사용하는 경우와 주의

- 같은 틀에 행동만 다른 대상이 여럿 있을 때 (몬스터, 아이템)
- 여러 종류를 `List<Enemy>`에 담아 한 번에 처리할 때
- "is-a" 관계일 때만 씁니다. 무기를 "가지고 있는" 경우는 컴포넌트 참조가 알맞습니다.
- 상속 단계는 두세 단계 이내로 둡니다.
- 자식이 바꿀 이유가 없는 함수에는 `virtual`을 붙이지 않습니다.
- 유니티 이벤트 함수(`Start`, `Update` 등)는 `override` 없이 같은 이름으로 작성합니다.

#### 추상 클래스

`abstract`는 부모가 틀만 제공하고 자식이 반드시 구현해야 하는 함수를 정합니다.

```csharp
public abstract class Potion
{
    public int amount;
    public abstract void Use(PlayerStats player);   // 자식이 반드시 구현
}
```

- 수치만 다르고 동작이 같다면 enum과 데이터로 구분하고, 효과 자체가 다르다면 상속으로 나눕니다.

#### 상속의 기본 규칙

- 부모의 변수와 함수를 자식이 물려받습니다. 부모를 수정하면 상속받은 자식도 영향을 받습니다.
- 자식에서 값을 바꿔도 부모는 바뀌지 않습니다. 부모 클래스는 객체를 만드는 **설계도(프리팹)** 역할을 합니다.

#### 변수 수정: protected와 생성자

- **private**: 같은 클래스 안에서만 접근합니다. 자식도 접근할 수 없습니다.
- **protected**: 같은 클래스와 **자식 클래스**에서 접근합니다. 외부 스크립트에서는 접근할 수 없습니다.
- 자식이 부모 변수의 값을 정하고 싶다면 `protected`로 열어 두고, 자식의 **생성자에서** 값을 넣습니다.

```csharp
class Pet
{
    protected string name = " ";   // 자식은 접근 가능, 외부는 접근 불가
}

class Dog : Pet
{
    public Dog() { name = "강아지"; }   // 생성자에서 부모의 protected 값을 설정
}
```

#### 함수 수정: virtual, override, base

- 부모 함수에 `virtual`을 붙이면 자식이 로직을 바꿀 수 있습니다.
- 자식은 같은 이름과 매개변수의 함수를 `override`로 선언합니다.
- 부모 로직을 먼저 실행하고 추가 내용을 넣으려면 `base.함수명();`을 씁니다. **`base` 호출은 `override`된 함수 안에서만** 의미가 있습니다.

```csharp
public override void Crying()
{
    base.Crying();          // 부모: "강아지가 짖다"
    Debug.Log("멍멍");       // 자식: 추가 동작
}
```

- `override` 없이 같은 이름의 함수를 자식에 만들면 부모 함수가 가려질 뿐, 다형성이 동작하지 않습니다.

#### 다형성과 한계

- 부모 타입으로 자식 객체를 만들 수 있습니다. 예: `Pet dog1 = new Dog();`
- 부모 타입으로 선언하면 **부모에 있는 함수만 호출할 수 있습니다.** 예: `dog1.Chase()`는 호출할 수 없습니다.
- `override`된 함수는 자식의 내용으로 실행됩니다.
- 객체는 부모 설계도를 기준으로 만들어지므로, 부모 타입으로 보면 자식만의 차이는 인지되지 않습니다.

#### 예제: Pet, Dog, Cat

공통 기능은 `Pet`에 두고, 종류마다 다른 부분만 자식에서 바꿉니다.

| 구분 | 설계 | 이유 |
| --- | --- | --- |
| 공통 변수 `name` | `protected` | 자식이 생성자에서 자기 이름을 설정 |
| `Move`, `Describe` | `virtual` 없음 | 자식이 바꾸지 않는 기능 |
| `Crying` | `virtual` | 종류마다 소리가 다름 |
| `Dog.Crying` | `override` + `base.Crying()` | 부모 동작 후 "멍멍" 추가 |

실행 결과는 다음과 같습니다.

```
강아지가 짖다 → 멍멍        ← dog.Crying()
강아지가 움직인다            ← dog.Move()
나는 강아지입니다.           ← dog.Describe()
강아지가 누군가를 쫓는다.    ← dog.Chase()
-------------------------
(고양이)가 짖다 → 냐옹       ← cat.Crying()
(고양이)가 움직인다          ← cat.Move()
나는 (고양이)입니다.         ← cat.Describe()
(고양이)가 가출했다          ← cat.RunAway()
```

### 3-5. 프로퍼티

변수처럼 `.`으로 접근하면서도, 값을 넣거나 꺼낼 때 검사나 계산을 끼워 넣을 수 있습니다.

```csharp
private int hp = 100;

public int HP
{
    get { return hp; }                       // 꺼낼 때
    set { hp = Mathf.Clamp(value, 0, 100); } // 넣을 때 0~100으로 제한
}
```

- **get**은 값을 읽을 때, **set**은 값을 쓸 때 실행됩니다. `value`는 대입된 값입니다.
- `public int HP { get; private set; }`처럼 쓰면 바깥에서는 읽기만 가능합니다.
- `private` 변수를 안전하게 공개하기 위해 씁니다. 필드를 그대로 `public`으로 열면 아무 값이나 들어올 수 있습니다.

### 3-6. 이벤트

"어떤 일이 일어났다"는 사실을 알리고, 받고 싶은 쪽이 구독하는 방식입니다. 알리는 쪽은 받는 쪽을 몰라도 됩니다.

```csharp
using System;

public class Player : MonoBehaviour
{
    public event Action OnDead;

    public void Die() { OnDead?.Invoke(); }
}

public class UIManager : MonoBehaviour
{
    public Player player;

    void OnEnable()  { player.OnDead += ShowGameOver; }   // 구독
    void OnDisable() { player.OnDead -= ShowGameOver; }   // 해제

    void ShowGameOver() { Debug.Log("게임 오버 화면 표시"); }
}
```

- **Action**: 값을 주고받지 않는 함수 형태입니다. 값이 있으면 `Action<int>`처럼 씁니다.
- **?.Invoke()**: 구독자가 없을 때 `null` 오류를 막습니다.
- 버튼의 `onClick.AddListener`도 같은 원리입니다.
- 구독했으면 반드시 해제합니다. 해제하지 않으면 파괴된 오브젝트에서 오류가 납니다. `OnEnable`에서 구독하고 `OnDisable`에서 해제하는 것이 기본입니다.

## 4. 자료구조

### 4-1. 선형 리스트와 연결 리스트

- **선형 리스트**는 데이터가 한 줄로 이어져 순서를 가지는 구조를 통틀어 부르는 말입니다. 배열, List, Stack, Queue, 연결 리스트가 여기에 속합니다.
- **연결 리스트**는 각 노드가 다음 노드의 연결 고리를 가집니다. 중간 삽입과 삭제는 빠르지만, 번호로 찾으려면 처음부터 따라가야 합니다.

```csharp
public class Node
{
    public int value;
    public Node next;   // 다음 노드를 가리키는 연결 고리
}
```

- 실무에서는 `List`가 대부분 더 빠르므로, 삽입과 삭제가 아주 잦은 특수한 경우에만 고려합니다.

### 4-2. List, Dictionary, HashSet

```csharp
// List: 번호로 접근, 크기 자동 조절
List<GameObject> enemies = new List<GameObject>();
enemies.Add(enemyA);
enemies.Remove(enemyA);
enemies.RemoveAt(0);
int count = enemies.Count;                              // 괄호 없는 프로퍼티
GameObject found = enemies.Find(e => e.name == "Boss");

// Dictionary: 키로 값 찾기
Dictionary<string, GameObject> monsters = new Dictionary<string, GameObject>();
monsters.Add("boss", bossObj);
if (monsters.ContainsKey("dragon")) { }   // 없는 키를 꺼내면 오류, 먼저 확인

// HashSet: 중복 없는 목록
HashSet<string> collected = new HashSet<string>();
collected.Add("열쇠");
bool hasKey = collected.Contains("열쇠");
```

- **Dictionary**: 같은 키를 두 번 `Add`하면 오류가 납니다. 값을 바꿀 때는 `dict[key] = value`를 씁니다.
- **HashSet**: 같은 값을 넣어도 무시됩니다. `Contains`로 포함 여부를 확인합니다.
- **List 순회 중 삭제 금지**: `foreach` 도중 지우면 오류가 납니다. 뒤에서부터 도는 `for`문을 씁니다.

```csharp
for (int i = enemies.Count - 1; i >= 0; i--)
{
    if (enemies[i] == null) enemies.RemoveAt(i);
}
```

### 4-3. Stack과 Queue

**Stack**은 입구와 출구가 같은 막힌 통이고, **Queue**는 입구와 출구가 반대쪽인 관입니다.

| 구분 | Stack | Queue |
| --- | --- | --- |
| 순서 | 후입선출 (LIFO) | 선입선출 (FIFO) |
| 넣기 | `Push` (맨 위) | `Enqueue` (맨 뒤) |
| 꺼내기 | `Pop` (맨 위) | `Dequeue` (맨 앞) |
| 확인만 | `Peek` (맨 위) | `Peek` (맨 앞) |
| 대표 용도 | 뒤로 가기, 되돌리기, UI 팝업 | 대기열, 턴 순서, 순차 처리 |

- Stack과 Queue의 메서드 이름은 서로 쓸 수 없습니다.

```csharp
Stack<string> stack = new Stack<string>();
stack.Push("A");
stack.Push("B");
stack.Pop();        // "B" (나중에 넣은 것부터)

Queue<string> queue = new Queue<string>();
queue.Enqueue("A");
queue.Enqueue("B");
queue.Dequeue();    // "A" (먼저 넣은 것부터)
```

#### Queue로 턴 순서 돌리기

`Dequeue`로 맨 앞을 꺼내 처리하고 `Enqueue`로 맨 뒤에 다시 넣으면 순서가 한 바퀴씩 돕니다. 종료 조건이 없으면 무한 루프가 되므로 횟수를 정합니다.

```csharp
for (int i = 0; i < 6; i++)
{
    string current = turnOrder.Dequeue();
    Debug.Log($"{current}의 턴");
    turnOrder.Enqueue(current);
}
```

#### Stack으로 UI 팝업 관리

- 창을 열 때 `Push`, 닫을 때 `Pop`을 호출하면 나중에 연 창이 먼저 닫힙니다.
- 전부 닫을 때는 `while (stack.Count > 0)`으로 `Pop`을 반복합니다.
- 원래 값(예: `timeScale`)은 처음 열 때 한 번 저장하고, 모든 창이 닫힐 때 한 번 복원합니다. 열 때마다 저장하면 이미 바뀐 값이 저장됩니다.

## 5. 문자열 처리

### 5-1. 보간 `$""`

`+`를 여러 번 쓰는 대신 `$`를 앞에 붙이고 `{}`에 변수를 넣습니다.

```csharp
int score = 120;
string msg = $"현재 점수: {score}점";   // "현재 점수: 120점"
```

### 5-2. StringBuilder

문자열은 수정할 수 없어서 `+`로 계속 이어 붙이면 새 문자열이 계속 만들어지고 메모리 할당이 쌓입니다. 많이 이어 붙일 때는 `StringBuilder`를 씁니다.

```csharp
using System.Text;

StringBuilder sb = new StringBuilder();
for (int i = 0; i < 5; i++) { sb.Append(i); sb.Append(", "); }
string result = sb.ToString();   // "0, 1, 2, 3, 4, "
```

- `Update`에서 매 프레임 문자열을 새로 만들지 않습니다. UI 텍스트처럼 값이 바뀔 때만 갱신합니다.

## 6. 생명주기와 코루틴

### 6-1. 실행 순서

오브젝트가 씬에 활성화되면 대략 다음 순서로 함수가 불립니다.

```
Awake → OnEnable → Start → (매 프레임) FixedUpdate → Update → LateUpdate
→ (비활성화) OnDisable → (파괴) OnDestroy
```

- **Awake**: 다른 스크립트가 준비되기 전에 호출됩니다. 자기 자신의 `GetComponent`를 여기서 해 두면 안전합니다.
- **Start**: 씬의 모든 Awake가 끝난 뒤 실행되므로, 다른 스크립트의 값을 참조하기 좋습니다.
- **OnEnable / OnDisable**: 켜지거나 꺼질 때마다 호출됩니다. 이벤트 구독과 해제에 씁니다.
- **LateUpdate**: 모든 Update가 끝난 뒤 호출됩니다. 카메라 추적처럼 움직임이 끝난 뒤 계산할 때 씁니다.
- **FixedUpdate**: 정해진 물리 주기(기본 0.02초)마다 호출됩니다. Rigidbody 조작에 씁니다.

> Start는 처음 활성화될 때 한 번만 호출됩니다. 다시 켤 때 초기화가 필요하면 OnEnable에 둡니다.

#### Script Execution Order

스크립트끼리 "A가 먼저 준비되어야 B가 동작한다"는 관계가 있을 때 `Edit > Project Settings > Script Execution Order`에서 실행 순서를 숫자로 정합니다. 실행 순서에 기대는 구조는 나중에 찾기 어려우니 최소한으로 씁니다.

### 6-2. 코루틴

함수 실행을 중간에 멈췄다가 조건이 맞으면 이어서 실행합니다. Update처럼 매 프레임 검사하지 않고, 시간이 지나거나 조건이 맞을 때까지 기다릴 수 있습니다.

```csharp
IEnumerator BlinkRoutine()
{
    for (int i = 0; i < 5; i++)
    {
        sprite.enabled = false;
        yield return new WaitForSeconds(0.1f);   // 0.1초 대기
        sprite.enabled = true;
        yield return new WaitForSeconds(0.1f);
    }
}

StartCoroutine(BlinkRoutine());   // 시작
```

| 대기 방식 | 의미 |
| --- | --- |
| `yield return null` | 다음 프레임까지 대기 |
| `new WaitForSeconds(초)` | 게임 시간 기준 대기 |
| `new WaitForSecondsRealtime(초)` | 일시정지와 무관하게 실제 시간 대기 |
| `new WaitUntil(() => 조건)` | 조건이 참이 될 때까지 대기 |

- **중단**: `StopCoroutine(코루틴)`, `StopAllCoroutines()`
- **비활성화 주의**: 코루틴은 시작한 오브젝트가 활성 상태일 때만 진행됩니다. `SetActive(false)`로 끄면 코루틴도 멈춥니다. 꺼진 뒤에도 기다려야 하는 리스폰 같은 처리는 살아 있는 매니저에서 시작합니다.
- **틱 피해 예시**: 함정에 들어가면 코루틴을 시작하고, 피해를 준 뒤 1초 쉬고 다시 주는 흐름을 `while (true)`와 `yield return`으로 만듭니다. 나갈 때 `StopCoroutine`으로 멈춥니다. 매 프레임 시간을 세는 방식보다 흐름이 코드에 그대로 드러납니다.
- 코루틴은 별도 스레드가 아니라 메인 스레드에서 나눠 실행됩니다. 같은 코루틴의 중복 실행을 막으려면 핸들을 저장해 둡니다.

## 7. 람다식과 클로저

- **람다식(`() => 실행문`)**: 이름 없이 그 자리에서 만드는 함수입니다.
- **매개변수가 있는 함수를 버튼에 연결할 때**
  - 잘못된 예: `AddListener(imgView(cnt));`는 함수를 실행한 결과를 넘기므로 에러가 납니다.
  - 올바른 예: `AddListener(() => imgView(cnt));`는 클릭할 때 실행되도록 등록합니다.

### 7-1. 클로저 문제

람다식은 `i`의 숫자를 복사하지 않고 변수 `i`를 참조합니다. 반복문이 끝나면 `i`는 마지막 값이 되어, 모든 버튼이 같은 값을 씁니다. 반복문 안에서 임시 변수에 복사해 해결합니다.

```csharp
for (int i = 0; i < 4; i++)
{
    int idx = i;   // 매 바퀴의 i 값을 고정
    btnList[idx].GetComponent<Button>().onClick.AddListener(() => { imgView(idx); });
}
```

- `idx`는 이름일 뿐이며 `k`, `buttonIndex`를 써도 동작은 같습니다. 역할이 드러나는 이름을 쓰는 것이 좋습니다.
- 주석(`//`)은 빌드할 때 제거되므로 실행 성능에 영향이 없습니다.

## 8. 좌표와 Transform

### 8-1. Vector3

- x, y, z 세 축의 위치나 방향을 담는 구조체입니다.
- 사용: `Vector3 loc = new Vector3(x, y, z);`

### 8-2. Transform 제어

- 위치: `transform.position = new Vector3(x, y, z);`
- 회전: `transform.rotation = Quaternion.Euler(x, y, z);`
- 크기: `transform.localScale = new Vector3(x, y, z);`
- **Euler(오일러)**: x, y, z 세 축으로 회전을 나타냅니다. 이해하기 쉽습니다.
- **Quaternion(쿼터니언)**: 네 개의 값으로 회전을 나타냅니다. 짐벌락 문제를 피할 수 있습니다.

### 8-3. Time.deltaTime

- **개념**: 1초처럼 고정된 값이 아니라, 직전 프레임부터 현재 프레임까지 흐른 짧은 시간입니다.
- **타이머 원리**: Update는 1초에 수십\~수백 번 실행됩니다. 매 프레임 deltaTime을 더해 나가면 누적값이 1초가 되는 순간 현실의 1초가 지난 것입니다.
- **장점**: 기기 성능과 관계없이 현실 시간 기준의 타이머를 만들 수 있습니다.
- **활용**: `speed * Time.deltaTime`을 곱하면 프레임 수와 상관없이 이동 거리가 일정합니다.

### 8-4. GetChild와 Find

- **GetChild(index)**: 바로 아래 자식을 순서(0, 1, 2…)로 가져옵니다. `for`문으로 자식을 순회할 때 적합합니다. `GetChild(0).GetChild(idx)`처럼 이어 쓰면 손자까지 접근할 수 있습니다.
- **Find("상위/하위")**: 이름이나 경로로 특정 자식을 가져옵니다.
- **주의**: GetChild는 자식 순서가 바뀌면 다른 대상을 잡습니다. Find는 이름이 바뀌면 `null`이 됩니다. 중요한 대상은 Inspector 연결을 우선 고려합니다.

## 9. 물리, 충돌, 카메라, 입력

### 9-1. 충돌과 물리 설정

- **Box Collider**: 충돌 범위를 정합니다.
- **Rigidbody**: 중력과 물리 연산을 적용합니다.
- **Is Trigger**: 충돌을 감지하지만 물리적으로 막지는 않습니다. `OnTriggerEnter` 같은 함수가 호출됩니다.
- **Is Kinematic**: 체크하면 물리 엔진의 힘을 받지 않고 코드(Transform)로 직접 움직입니다.

### 9-2. 카메라

- **Culling Mask**: 카메라가 보여 줄 레이어를 고릅니다.
- **Orthographic(직교)**: 원근감이 없는 2D에 씁니다.
- **Perspective(원근)**: 가까울수록 크게 보이는 3D에 씁니다.
- **Near / Far**: 렌더링을 시작하고 끝내는 거리입니다.
- **Field of View**: 시야각입니다.

### 9-3. 충돌 메소드와 Raycast

| 구분 | OnCollision (물리 충돌) | OnTrigger (통과형 감지) |
| --- | --- | --- |
| 작동 | 부딪혀 튕기거나 막힘 | 뚫고 지나가며 감지만 함 |
| 예시 | 벽, 바닥, 장애물 | 코인, 체크포인트, 포털 |
| 필수 조건 | Collider와 Rigidbody 중 하나 이상 | Collider의 Is Trigger 체크 |

- **타이밍**: `Enter`는 처음 닿을 때, `Stay`는 닿아 있는 동안 매 프레임, `Exit`는 떨어질 때입니다. 예: `OnCollisionEnter`, `OnTriggerExit`
- **트리거 판단**: `other.CompareTag("Coin")`으로 상대의 태그를 확인해 원하는 대상일 때만 처리합니다.
- `other`는 Unity가 충돌할 때 자동으로 넘겨 주는 매개변수입니다. 동작하려면 한쪽에 Collider, 한쪽에 `Is Trigger`, 한쪽에 Rigidbody가 필요합니다.

```csharp
void OnTriggerEnter(Collider other)
{
    if (other.CompareTag("Coin"))   // 문자열 비교보다 가볍고 오타도 잡아 줌
    {
        // 코인 획득 처리
    }
}
```

#### Raycast와 SphereCast

- **Raycast**: 가는 선(광선)을 쏘아 거리 안의 대상을 탐지합니다. 총알, 시선, 클릭 위치에 씁니다.
- **SphereCast**: 반지름이 있는 구를 쏩니다. 몸 크기만큼 넓게 검사하므로 얇은 모서리를 빗나가는 문제가 줄어듭니다. 바닥 판정이나 넓은 범위의 근접 공격 판정에 씁니다.

```csharp
RaycastHit hit;
bool hitSomething = Physics.SphereCast(origin, radius, direction, out hit, maxDistance);
```

- 시작점이 이미 물체 안에 있으면 그 물체를 맞지 않을 수 있으므로 시작 위치를 살짝 띄웁니다.
- 반지름이 너무 크면 닿지 않은 것도 맞는 것으로 판정되므로 몸 크기에 맞춥니다.
- `Physics.SphereCast`는 Raycast보다 무거우므로 매 프레임 불필요하게 호출하지 않습니다.
- 디버깅할 때는 `Gizmos.DrawWireSphere`로 검사 범위를 씬 뷰에 그려 확인합니다.

### 9-4. 물리 이동

| 방식 | 특징 | 용도 |
| --- | --- | --- |
| `transform.position` 직접 변경 | 물리를 무시하고 위치만 바뀜. 충돌이 누락될 수 있음 | 물리가 필요 없는 오브젝트 |
| `rb.velocity` (최신 Unity는 `linearVelocity`) | 속도를 지정. 지정하지 않은 축은 유지 | 방향키 이동 |
| `rb.AddForce()` | 힘으로 가속. 질량과 ForceMode에 따라 달라짐 | 점프, 밀치기 |
| `rb.MovePosition()` | 물리 계산을 거쳐 목표 위치로 이동 | 충돌을 유지하며 이동 |

```csharp
void FixedUpdate()
{
    Vector3 input = new Vector3(Input.GetAxis("Horizontal"), 0, Input.GetAxis("Vertical"));
    rb.velocity = new Vector3(input.x * moveSpeed, rb.velocity.y, input.z * moveSpeed);   // y(중력) 유지
}
```

- 물리 관련 코드는 `FixedUpdate`에 둡니다. 입력 감지는 `Update`에서 해도 됩니다.
- 사용하는 Unity 버전에 따라 `velocity`가 `linearVelocity`로 바뀌었을 수 있습니다.

#### ForceMode

| 모드 | 설명 | 예 |
| --- | --- | --- |
| Force | 질량의 영향을 받는 지속적인 힘 (FixedUpdate에서 계속 호출) | 추진력 |
| Impulse | 질량의 영향을 받는 순간적인 힘 (한 번 호출) | 점프, 폭발 |
| VelocityChange | 질량을 무시하고 속도를 바로 바꿈 | 같은 속도로 튀기기 |
| Acceleration | 질량을 무시하고 가속도를 줌 | 질량과 무관한 가속 |

```csharp
rb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);   // 점프
```

#### Layer와 LayerMask

- 태그는 라벨이고, 레이어는 물리 충돌과 렌더링의 대상을 나누는 기능입니다.
- **추가**: Inspector의 Layer 드롭다운에서 Add Layer, 또는 `Edit > Project Settings > Tags and Layers`
- **충돌 설정**: `Edit > Project Settings > Physics`의 Layer Collision Matrix에서 레이어 쌍마다 충돌 여부를 정합니다.
- 바닥 판정은 아래 방향 레이캐스트나 SphereCast로 하는 것이 안전합니다. `OnCollisionEnter`로 착지를 판정하면 벽에서도 점프되는 문제가 생깁니다.

```csharp
public LayerMask groundMask;   // Inspector에서 Ground 레이어 선택

bool isGrounded = Physics.Raycast(transform.position, Vector3.down, 1.1f, groundMask);
```

### 9-5. 입력과 태그

- **Input System 오류**: New Input System으로 설정된 상태에서 `Input.GetKey`를 쓰면 에러가 납니다. `Project Settings > Player > Other Settings > Active Input Handling`을 `Input Manager (Old)` 또는 `Both`로 바꾸고 재시작합니다.
- **입력 함수**: `Input.GetKey`는 누르는 동안 매 프레임(이동), `Input.GetKeyDown`은 누른 순간 한 번(점프)입니다.
- **태그(Tag)**: 오브젝트를 그룹으로 묶는 라벨입니다. 충돌 상대를 구분하거나 `GameObject.FindGameObjectsWithTag("태그")`로 일괄 검색할 때 씁니다.
- 태그는 대소문자를 구분합니다(`player1`과 `Player1`은 다름). 비교는 `CompareTag`를 씁니다.

## 10. 컴포넌트 제어

- **GetComponent\<T>()**: 같은 오브젝트에 붙은 부품을 가져옵니다. 예: `GetComponent<AudioSource>().enabled = true;`
- **GetComponentInChildren\<T>()**: 자기 자신과 모든 자식(손자 포함)에서 첫 번째 `T`를 찾습니다. 꺼진 자식까지 찾으려면 `GetComponentInChildren<T>(true)`를 씁니다.
- **GetComponentInParent\<T>()**: 자기 자신부터 위쪽 부모 방향으로 찾습니다.
- **SetActive(bool)**: 오브젝트 전체를 켜거나 끕니다. 인스펙터 맨 위의 체크박스입니다.
- **enabled**: 컴포넌트 하나만 켜고 끕니다. 오브젝트는 그대로 보입니다.
- **SetTrigger("이름")**: Animator에 일회성 신호를 보냅니다. 예: `animator.SetTrigger("doDie");`
- **AddComponent\<T>()**: 부품을 코드로 새로 붙입니다. 예: `gameObject.AddComponent<AudioSource>();`
- **Destroy**: `Destroy(gameObject)`는 오브젝트 전체를, `Destroy(GetComponent<AudioSource>())`는 부품 하나만 삭제합니다.
- **Instantiate / Debug.Log**: `Instantiate`는 생성, `Debug.Log`는 Console 창에 메시지를 출력합니다.

찾는 함수는 비용이 있으므로 `Update`에서 매번 부르지 말고 `Awake`에서 한 번 가져와 저장합니다.

## 11. UI

### 11-1. 이미지와 폰트

1. 이미지 파일을 Assets에 넣고 Texture Type을 \*\*Sprite (2D and UI)\*\*로 바꿉니다.
2. Canvas 아래에 Image 오브젝트를 만들고 Sprite에 연결합니다.
3. 폰트는 `Window > TextMeshPro > Font Asset Creator`에서 폰트 에셋을 만들고, **Atlas Population Mode**를 Static에서 **Dynamic**으로 바꿉니다.

### 11-2. Button

- **Transition(클릭 효과)**: Color Tint(색 변화), Sprite Swap(이미지 교체), Animation(애니메이션)
- **코드 연결**: `public Button leftButton;`으로 선언하고 인스펙터에서 연결한 뒤 `leftButton.onClick.AddListener(함수이름);`으로 등록합니다.
- **인스펙터 연결**: 버튼의 On Click 항목에 함수를 직접 지정하면 코드 없이 연결됩니다.
- 매개변수가 있는 함수는 람다식으로 감쌉니다(7장 참고).

### 11-3. Scroll View와 자동 슬라이드

- **Viewport**: 실제로 보이는 화면 영역입니다. **Horizontal / Vertical**은 스크롤 방향입니다.
- **horizontalNormalizedPosition**: 가로 스크롤 위치를 0.0(맨 왼쪽)\~1.0(맨 오른쪽) 비율로 나타냅니다.
- **자동 이동**: `Update`에서 `slideSpeed * Time.deltaTime`을 매 프레임 더합니다.
- **순환 번호**: 끝에 닿으면 `(currentIdx + 1) % cnt`로 다음 화면 번호를 구합니다. 0 → 1 → 2 → 0으로 돕니다.
- **화면 전환 순서**: 모든 자식 화면을 `SetActive(false)`로 끄고, 선택한 화면만 켠 뒤, `scrollRect.content`를 그 화면의 RectTransform으로 바꾸고, 스크롤 위치를 0.0으로 되돌립니다.

### 11-4. UI 심화

| 항목 | 역할 | 유의사항 |
| --- | --- | --- |
| RectTransform 앵커와 피벗 | 화면 크기가 바뀌어도 UI 위치 유지 | 앵커 프리셋은 Alt를 누르면 위치까지 맞춤 |
| Canvas Render Mode | Screen Space Overlay, Camera, World Space 중 선택 | Camera 모드는 Render Camera 지정 필요 |
| Canvas Scaler | 해상도가 달라도 UI 크기를 맞춤 | Scale With Screen Size와 Reference Resolution 설정 |
| Layout Group | 자식을 가로, 세로, 격자로 자동 정렬 | 자식 크기는 Layout Element로 조절 |
| Content Size Fitter | 내용물 크기에 맞춰 부모 크기 조절 | Layout Group과 함께 쓰는 경우가 많음 |
| EventSystem | UI 클릭과 입력 처리 | 씬에 없으면 버튼이 반응하지 않음 |

```csharp
public class HpUI : MonoBehaviour
{
    public Image hpFill;
    public TMP_Text hpText;
    public float maxHp = 100f;

    public void SetHp(float hp)
    {
        hpFill.fillAmount = hp / maxHp;            // 체력바 (0~1)
        hpText.text = $"HP {hp} / {maxHp}";        // 값이 바뀔 때만 호출
    }
}
```

- **Raycast Target**: 클릭을 받을 필요가 없는 Image나 Text는 끕니다. 켜져 있으면 뒤의 버튼 클릭을 막을 수 있습니다.
- Game 뷰의 Aspect를 바꿔 여러 해상도에서 UI가 잘리지 않는지 확인합니다.

## 12. 애니메이션

| 항목 | 역할 | 유의사항 |
| --- | --- | --- |
| Animator Parameter | Float, Bool, Trigger, Int로 전환 조건을 정함 | 이름 오타가 나면 동작하지 않음 |
| Blend Tree | 파라미터 값에 따라 클립을 섞음 | 클립들이 같은 리그를 써야 함 |
| Animation Event | 특정 프레임에 함수 호출 | 함수 이름이 정확해야 함 |
| Avatar와 Humanoid | 사람형 본을 표준 이름으로 매핑 | 실패하면 Configure에서 수동 연결 |
| Layer와 Avatar Mask | 상체와 하체를 따로 재생 | Layer의 Weight 확인 |
| Root Motion | 애니메이션의 이동값을 캐릭터 이동에 반영 | 직접 이동 코드와 충돌할 수 있음 |

```csharp
Animator animator;

void Awake() { animator = GetComponent<Animator>(); }   // 한 번만 가져와 저장

public void SetSpeed(float speed)
{
    animator.SetFloat("Speed", speed, 0.1f, Time.deltaTime);   // dampTime으로 부드럽게 변화
}

public void OnFootstep()   // Animation Event가 호출하는 함수
{
    Debug.Log("발소리");
}

// 부드러운 상태 전환: animator.CrossFade("Walk", 0.2f);
```

- **Animation Event 함수**: `public`이고 Animator가 붙은 같은 오브젝트에 있어야 합니다. 매개변수는 없거나 지정된 타입 하나만 가능합니다.
- **Root Motion**: 직접 이동 코드와 함께 쓰면 이동이 두 배로 보이거나 떨릴 수 있습니다. 직접 이동을 쓴다면 끕니다.
- **Humanoid**: Rigify처럼 본 이름이 다른 모델은 매핑이 실패하기 쉽습니다. 실패하면 Generic으로 바꾸거나 수동 연결합니다.
- **파라미터 관리**: 여러 스크립트가 같은 파라미터를 바꾸면 값이 뒤섞입니다. 담당 스크립트를 하나로 정합니다.

## 13. 리소스: 머티리얼, 파티클, 오디오

### 13-1. 머티리얼

- **개념**: 색, 금속성, 매끄러움, 투명도, 텍스처를 정하는 에셋입니다.
- **만들기**: Project 창에서 Create > Material을 선택하고 Albedo, Metallic, Smoothness를 설정한 뒤 오브젝트에 드래그합니다. URP에서는 Base Map에 Texture를 연결합니다.
- **코드**: `GetComponent<Renderer>().material = newMaterial;`
- **활용**: 피격 시 붉게, 강화 무기의 금색 재질, 투명 유리나 방어막, 같은 모델에 재질만 바꿔 변종 만들기

### 13-2. 파티클에 이미지 넣기

1. 파티클용 머티리얼을 만듭니다.
2. Shader를 `Universal Render Pipeline/Particles/Unlit`(또는 Lit)으로 바꾸고 Base Map에 이미지를 연결합니다.
3. Particle System의 Renderer 모듈 Material 슬롯에 머티리얼을 드래그합니다.

### 13-3. 오디오 소스와 3D 사운드

- **Audio Source**: Add Component로 추가하고 AudioClip에 음원을 연결합니다.
- **3D**: Spatial Blend를 2D(0)에서 3D(1)로 올립니다.
- **거리**: Min Distance는 최대 음량 거리, Max Distance는 무음이 되는 거리입니다. Volume Rolloff를 Linear Rolloff로 두면 거리에 비례해 줄어듭니다.
- **Audio Listener**: 소리를 듣는 기준입니다. 기본 Main Camera에 있으며, 3D 음향에 필요합니다.

## 14. 네임스페이스와 자료구조 활용

### 14-1. List와 인스펙터 할당

- `public List<GameObject> gameObjectList;`를 선언하면 인스펙터에서 넣은 순서대로 0, 1, 2… 인덱스가 붙습니다.
- 사용: `gameObjectList[0]`

### 14-2. 이차원 리스트

- 선언: `public List<List<GameObject>> 몬스터;`
- 카테고리별(대형, 소형 등)로 나눠 관리합니다. 접근은 `몬스터[0][1]`처럼 인덱스를 두 번 씁니다.

### 14-3. enum과 (int) 캐스팅

```csharp
public enum 사이즈 { 소형 = 0, 중형 = 1, 대형 = 2, 보스 = 3 }

몬스터[(int)사이즈.대형][0].transform.position = new Vector3(0, 0, 0);
```

- enum 이름은 문자 타입이라 인덱스 자리에 바로 쓰면 타입 오류가 납니다. 그래서 `(int)`로 숫자 값을 꺼내 씁니다.

### 14-4. 자주 쓰는 네임스페이스

| 네임스페이스 | 용도 |
| --- | --- |
| UnityEngine | 오브젝트, 컴포넌트, 물리, 입력 등 기본 기능 |
| System.Collections.Generic | List, Dictionary 등 자료구조 |
| UnityEngine.UI / TMPro | 버튼, 이미지, 텍스트 UI |
| UnityEngine.SceneManagement | 씬 로드와 전환 |
| System.Collections | 코루틴 |
| UnityEngine.AI | 내비게이션(길찾기) |
| UnityEngine.EventSystems | UI 입력 판정 |
| System.IO | 파일 저장과 불러오기 |
| UnityEngine.Audio | 오디오 믹서 |
| UnityEngine.Profiling | 메모리와 성능 분석 |

## 15. 프리팹과 프로젝트 구조

### 15-1. 프리팹

- **프리팹(Prefab)**: 원본 틀입니다. 원본을 고치면 모든 인스턴스에 반영됩니다.
- **인스턴스**: 틀에서 찍어낸 실제 오브젝트입니다. 씬에 여러 개 둘 수 있고 서로 독립적입니다.
- **만들기**: Hierarchy에서 오브젝트를 구성한 뒤 Project 폴더로 드래그합니다.
- **코드 생성**: `Instantiate(프리팹)`

### 15-2. Prefab Variant

원본 프리팹과 연결을 유지한 채, 다른 값만 기록하는 프리팹입니다.

- **만들기**: 프리팹 우클릭 > `Create > Prefab Variant`
- **값이 따라가는 규칙**
  - 바꾸지 않은 값은 원본을 따릅니다. 원본을 바꾸면 Variant에도 반영됩니다.
  - 인스펙터에서 바꾼 값은 Variant에 유지됩니다. 굵게 표시됩니다.
- **되돌리기**: 굵은 값을 우클릭하고 `Revert`를 선택합니다.
- **일반 복제와의 차이**: 복제본은 원본과 연결이 없고, Variant는 연결을 유지합니다.
- **코드와의 관계**: Variant에 기록되는 것은 에디터(Inspector)에서 바꾼 값뿐입니다. 코드로 바꾼 값은 실행 중에만 적용됩니다.
- **설계 기준**: 바리에이션 값(기준 HP, 속도)과 연결 요소는 인스펙터에서 정하고, 변동 상태(현재 HP)와 공통 동작은 코드에서 관리합니다.

```csharp
public class Enemy : MonoBehaviour
{
    [SerializeField] private int maxHp = 100;     // 기준값: 인스펙터에서 Variant마다 지정
    [SerializeField] private float moveSpeed = 3f;

    private int currentHp;                         // 변동값: 코드에서 관리

    public void Init() { currentHp = maxHp; }

    public void TakeDamage(int dmg)
    {
        currentHp -= dmg;
        if (currentHp <= 0) gameObject.SetActive(false);
    }
}
```

- Variant 위에 또 Variant를 만들면 값이 어디서 바뀌는지 추적하기 어려우므로 단계는 얕게 둡니다.

### 15-3. ScriptableObject

데이터를 담는 에셋(파일)을 만드는 클래스입니다. MonoBehaviour와 달리 오브젝트에 붙지 않고 Project 창에 `.asset` 파일로 존재합니다.

| 파일 | 종류 | 역할 |
| --- | --- | --- |
| `MonsterData.cs` | 코드 | 담을 값의 칸(종류)을 정의 (설계도) |
| `Goblin.asset` | 에셋 | 그 칸에 실제 값을 입력한 데이터 (작성된 시트) |

```csharp
[CreateAssetMenu(menuName = "Game/Monster Data")]
public class MonsterData : ScriptableObject
{
    public string monsterName;
    public int maxHp;
    public float moveSpeed;
}
```

- **만들기**: Project 창에서 우클릭 > `Create > Game > Monster Data`, 그 뒤 Inspector에서 값을 입력합니다.
- **사용**: 몬스터 스크립트가 `MonsterData` 타입의 칸을 두고, 그 칸에 `.asset`을 드래그로 연결합니다. 코드는 연결된 값을 읽어 씁니다.
- **불러오기**: Inspector 연결을 기본으로 합니다. `Resources.Load`는 `Resources` 폴더 안에 있어야 하며, 확장자 없이 경로로 부릅니다. 오타가 나면 `null`이 됩니다.
- **실행 중 값**: 에셋의 값을 실행 중에 바꾸면 에디터에서도 그대로 남습니다. 실행 중 바뀌는 값은 복사본을 만들어 씁니다.

```csharp
MonsterData runtime = Instantiate(data);   // 복사본, 원본 에셋은 그대로
```

#### 스포너와 함께 쓰는 흐름

```
Spawn(MonsterType.Goblin)
  → 목록에서 type이 같은 줄 찾기 (판단)
  → 그 줄의 프리팹을 Instantiate (외형과 동작)
  → 그 줄의 Goblin.asset을 SetData로 전달 (능력치)
  → 몬스터가 기준값(maxHp)으로 현재 상태(currentHp)를 시작
```

- 생성 시점의 기준값은 데이터가 정하고, 게임 중 변화는 몬스터가 관리합니다.

### 15-4. 씬 전환

씬은 게임의 화면 단위입니다. 씬 전환은 현재 씬을 끝내고 다른 씬을 불러오는 것입니다.

```csharp
using UnityEngine.SceneManagement;

SceneManager.LoadScene("GameScene");   // 씬 이름으로 불러오기
```

- 번호보다 이름으로 부르는 것이 안전합니다. 번호는 순서가 바뀌면 틀어집니다.
- **Build Settings 등록**: `Scenes In Build`에 씬이 있어야 합니다. 목록 순서가 번호를 결정합니다.
- **비동기 로드**: 큰 씬은 로딩 화면과 함께 불러옵니다.

```csharp
AsyncOperation op = SceneManager.LoadSceneAsync(sceneName);
op.allowSceneActivation = false;   // 준비만 하고 전환은 보류
// op.progress는 0.9까지 올라감 (로딩 게이지에 사용)
op.allowSceneActivation = true;    // 연출이 끝나면 전환
```

- **씬이 바뀌었을 때**: 씬 안의 오브젝트는 씬 파일의 초기 상태로 다시 생성됩니다. 값을 이어가려면 씬 밖(유지 오브젝트, 정적 변수, 저장 파일)에 보관해야 합니다.
- **씬 로드 알림**: `SceneManager.sceneLoaded` 이벤트에 구독하고, 구독 해제는 `OnDisable`에서 합니다.

### 15-5. 싱글톤과 DontDestroyOnLoad

**싱글톤**은 인스턴스를 하나만 두고 어디서든 `Instance`로 접근하는 방식입니다. 접근이 제한되는 것이 아니라, 여러 곳에서 **같은 하나**를 함께 씁니다. 교무실 열쇠가 하나뿐이고 여러 선생님이 쓰는 것과 같습니다.

```csharp
public class PersistentRoot : MonoBehaviour
{
    public static PersistentRoot Instance { get; private set; }

    void Awake()
    {
        if (Instance != null && Instance != this)
        {
            Destroy(gameObject);   // 중복이면 제거
            return;
        }
        Instance = this;
        DontDestroyOnLoad(gameObject);   // 씬이 바뀌어도 유지
    }
}
```

- **DontDestroyOnLoad**: 루트(최상위) 오브젝트에만 적용됩니다. 자식이면 무시됩니다.
- **계층으로 묶기**: `PERSISTENT` 루트 아래에 유지할 것을 모으면 자식 전체가 함께 유지됩니다. 씬 카메라와 씬 전용 UI는 씬 루트에 둡니다.
- **Camera와 Canvas**: 유지된 카메라와 씬 카메라가 둘 다 있으면 화면이 겹칩니다. 여러 씬에 공통인 UI만 유지합니다.
- 유지 오브젝트가 씬 안의 오브젝트를 Inspector로 연결한 참조는 씬이 바뀌면 끊깁니다.
- 남용하면 어디서 값이 바뀌는지 추적하기 어려우므로, 정말 하나만 필요한 매니저에만 씁니다.

## 16. 저장과 외부 라이브러리

### 16-1. PlayerPrefs

- 유니티에 내장된 간단한 로컬 저장소입니다. Windows에서는 레지스트리에 저장됩니다.
- 용량과 타입이 제한적입니다. 설정값이나 최고 점수처럼 작은 데이터에 씁니다.
- 저장: `PlayerPrefs.SetInt(id, value);`(SetFloat, SetString도 있음) 후 `PlayerPrefs.Save();`
- 불러오기: `PlayerPrefs.GetInt(id);`(GetFloat, GetString도 있음)

### 16-2. iTween

- **Hash 방식**: `iTween.Hash()`에 키와 값을 넣어 시간, 지연, 이징, 완료 콜백을 지정합니다.
- **주요 메서드**: MoveTo, MoveBy, RotateTo, ScaleTo, Punch, Shake

```csharp
iTween.MoveTo(cube, iTween.Hash(
    "x", 10, "y", 5, "z", 10,
    "time", 5,
    "delay", 3,
    "easetype", iTween.EaseType.easeInElastic,
    "oncomplete", "moveComp",
    "oncompletetarget", this.gameObject));
```

- 내부에서 Hash와 리플렉션을 많이 써서 모바일이나 대규모 프로젝트에서는 가비지 컬렉션 부담이 생길 수 있습니다. 그런 환경에서는 DOTween을 검토합니다.

## 17. 추가 학습 자료: 입력 시스템 (New Input System)

New Input System은 키 이름이 아니라 **동작 이름(Move, Jump)** 으로 입력을 다루고, 설정은 **Input Actions 에셋**에 모으는 방식입니다.

### 17-1. 기존 방식과 비교

```csharp
// 기존 방식 (Input Manager): 코드에 키가 고정됨
if (Input.GetKeyDown(KeyCode.Space)) Jump();

// New Input System: 동작 이름으로 입력을 받음
if (jumpAction.WasPressedThisFrame()) Jump();
```

| 구분 | Input Manager (Old) | New Input System |
| --- | --- | --- |
| 키 지정 위치 | 코드 안 | Input Actions 에셋 |
| 여러 장치 지원 | 장치마다 직접 분기 | 키보드, 패드, 터치를 하나의 동작으로 묶음 |
| 키 재설정 | 어려움 | 에셋에서 바꾸고 런타임 재바인딩 가능 |

### 17-2. 패키지 설치와 활성화

1. `Window > Package Manager`를 열고 왼쪽 상단에서 **Unity Registry**를 선택합니다.
2. **Input System**을 찾아 Install을 누르고, 재시작을 요청하면 재시작합니다.
3. `Edit > Project Settings > Player > Other Settings > Active Input Handling`에서 값을 고릅니다.

| 값 | 의미 |
| --- | --- |
| Input System Package (New) | New Input System만 사용 |
| Both | 두 방식을 함께 사용 (이전 프로젝트를 옮길 때 임시로 씀) |
| Input Manager (Old) | 기존 방식만 사용 |

- 값을 바꾸면 에디터가 재시작됩니다. 이 설정이 맞지 않으면 New Input System 상태에서 `Input.GetKey`를 쓸 때 에러가 납니다.

### 17-3. Input Actions 에셋 만들기

1. Project 창에서 우클릭 > `Create > Input Actions`를 선택합니다.
2. 에셋을 더블클릭해 편집창을 엽니다.
3. **Action Map**(예: Player)을 추가하고, 그 안에 **Action**(Move, Jump)을 만듭니다.
4. Action Type과 Binding을 정합니다.

| Action | Type | Binding 예 |
| --- | --- | --- |
| Move | Value (Vector2) | WASD, 방향키, 왼쪽 스틱 |
| Jump | Button | Space, 게임패드 South |

- **Value**: 방향이나 이동량처럼 연속된 값을 받습니다.
- **Button**: 누름과 떼기처럼 한 번씩 일어나는 입력을 받습니다.
- 편집창에서 **Save**를 해야 변경 내용이 반영됩니다.

### 17-4. Player Input 컴포넌트로 연결하기 (권장)

코드를 많이 쓰지 않고 `Player Input` 컴포넌트로 연결합니다.

1. 플레이어 오브젝트에 `Player Input` 컴포넌트를 추가합니다.
2. **Actions**에 Input Actions 에셋을 드래그합니다.
3. **Default Map**에 Player를 고릅니다.
4. **Behavior**를 고릅니다.
   - **Send Messages**: `OnMove`, `OnJump` 같은 이름의 함수를 자동으로 호출합니다.
   - **Invoke Unity Events**: Inspector에서 함수를 직접 연결합니다.
   - **Invoke C# Events**: C# 이벤트로 받습니다.

```
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerController : MonoBehaviour
{
    private Vector2 moveInput;

    // Action 이름이 Move이면 OnMove가 자동 호출됨
    void OnMove(InputValue value)
 
        moveInput = value.Get<Vector2>();
    }

    // Action 이름이 Jump이면 OnJump가 자동 호출됨
    void OnJump(InputValue value)
    {
        if (value.isPressed) Debug.Log("점프");
    }

    void FixedUpdate()
    {
        Vector3 move = new Vector3(moveInput.x, 0, moveInput.y);
        // 이동 처리
    }
}
```

- 함수 이름은 `On` + Action 이름이어야 합니다. 철자가 틀리면 아무 일도 일어나지 않습니다.
- 이동 값은 `OnMove`에서 저장하고 `FixedUpdate`에서 적용하는 구조가 물리 이동과 잘 맞습니다.

### 17-5. 코드에서 Action 직접 사용하기

`Player Input` 대신 스크립트에서 Action을 직접 찾아 쓸 수도 있습니다. 키 재설정이나 세밀한 제어가 필요할 때 씁니다.

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerInputHandler : MonoBehaviour
{
    public InputActionAsset actions;   // Inspector에서 에셋 연결

    private InputAction moveAction;
    private InputAction jumpAction;

    void OnEnable()
    {
        var map = actions.FindActionMap("Player");
        moveAction = map.FindAction("Move");
        jumpAction = map.FindAction("Jump");

        map.Enable();                     // 액션 맵을 켜야 입력을 받음
        jumpAction.performed += OnJump;   // 이벤트 방식으로 구독
    }

    void OnDisable()
    {
        jumpAction.performed -= OnJump;   // 구독 해제
        moveAction.Disable();
    }

    void Update()
    {
        Vector2 move = moveAction.ReadValue<Vector2>();   // 매 프레임 값 읽기
    }

    void OnJump(InputAction.CallbackContext ctx)
    {
        Debug.Log("점프");
    }
}
```

| 메서드 | 의미 |
| --- | --- |
| `WasPressedThisFrame()` | 이번 프레임에 눌렸는가 |
| `WasReleasedThisFrame()` | 이번 프레임에 떼졌는가 |
| `IsPressed()` | 누르고 있는 중인가 |
| `ReadValue<T>()` | 현재 값을 읽음 (Vector2 등) |

### 17-6. 장치 직접 읽기

Action 에셋 없이 장치를 바로 읽을 수도 있습니다. 간단한 테스트나 디버깅에 편리합니다.

```csharp
if (Keyboard.current.spaceKey.wasPressedThisFrame) Jump();

if (Gamepad.current != null)
{
    Vector2 stick = Gamepad.current.leftStick.ReadValue();
}
```

- `Keyboard.current`와 `Gamepad.current`는 장치가 없으면 `null`일 수 있으므로 확인합니다.

### 17-7. 자주 겪는 문제

- **입력이 전혀 안 들어옴**: Active Input Handling 설정, Action Map이 Enable되어 있는지, Player Input의 Default Map이 맞는지 확인합니다.
- **Send Messages 함수가 안 불림**: 함수 이름이 `On` + Action 이름과 같은지, `PlayerInput`과 같은 오브젝트에 스크립트가 있는지 확인합니다.
- **에디터에서 입력이 없음**: Game 뷰를 한 번 클릭해 포커스를 줍니다.
- **기존 코드가 섞여 있음**: `Input.GetKey` 같은 코드가 남아 있으면 Both 설정이 아닌 한 에러가 납니다.
- **씬 전환 후 입력이 멈춤**: 씬이 바뀌면 Action Map이 꺼질 수 있으니 다시 Enable합니다.

### 17-8. 정리

- Unity에서는 **패키지 설치 → Active Input Handling 설정 → Input Actions 에셋 → Player Input 컴포넌트 연결** 순서로 씁니다.
- 코드는 `OnMove`, `OnJump` 같은 함수로 받고, 이동 값은 `FixedUpdate`에서 적용합니다.
- 입력 방식은 한 가지로 통일합니다.

## 18. 기타 미정리 주제

### 18-1. 디버깅 도구

- **Debug.DrawRay / Gizmos**: 씬 뷰에 레이와 검사 범위를 그려 확인합니다.
- **Breakpoint**: Visual Studio에서 중단점을 찍고 변수 값을 확인합니다.
- **콘솔 에러**: 줄 번호와 스택 트레이스를 읽어 원인이 되는 호출을 따라갑니다.

### 18-2. 버전 관리(Git)

- **.gitignore**: `Library`, `Temp`, `Logs` 같은 자동 생성 폴더는 제외합니다.
- **씬과 프리팹 병합**: 텍스트 기반(YAML)이라 충돌이 나면 Unity의 Smart Merge를 쓰거나, 작업자가 씬을 나눠 맡습니다.
- **커밋 단위**: 기능 하나를 끝낼 때마다 커밋해 되돌리기 쉽게 만듭니다.
