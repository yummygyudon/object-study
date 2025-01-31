# Appendix B. 타입 계층 구현
#### ✅ 기본 전제
- 리스코프 치환 원칙은 <u>구현 방법에 의해서 보장되는 것이 아니기</u> 때문에 <br/> " 클라이언트 관점에서 타입 계층에서의 **행동 호환성 보장** "은 개발자의 몫이다.
  - _올바른 타입 계층 : <u>서브 타입이 **슈퍼타입을 대체**</u>할 수 있도록 **리스코프 치환 원칙 준수** 필요_


- **"동일 메시지에 대한 행동 호환성"** 형성 목적
  - `타입 계층` 구현 방식  & `다형성`을 구현하는 방법
  - " _슈퍼타입에 대해 전송한 메시지를 <u>서브 타입별로 다르게 처리</u>할 수 있는 방법_ " 제공


<br/>

---
## 구현 방법
### 클래스
> `타입` → 객체의 "**퍼블릭 인터페이스**"<br/>
> `클래스` → 객체의 "<u>타입</u>(**퍼블릭 인터페이스**)" 및 "<u>**구현**</u>" 정의 매개체 → "**사용자 정의 타입** `User-Defined Data Type`"

클래스 내에선 <u>"**퍼블릭 메서드**"를 구현함으로서 퍼블릭 인터페이스를 구성</u>해 나가는데<br/>
**타입**은 즉 " 퍼블릭 인터페이스 "를 의미하기 때문에<br/>
정리하자면<br/>
**퍼블릭 메서드**로 "<u>메시지에 응답할 수 있는 `타입`을 선언</u>"하는 동시에 "<u>객체 `구현`</u>"을 하는 것과 동일하다.

단, " _특정 개념의 타입이 절대적으로 하나만 존재_ "한다면 `타입 == 클래스`에 그쳐도 무방하지만<br/>
만약 동일 퍼블릭 인터페이스를 기반으로 <u>서로 다른 내용 및 방식의 구현</u>이 필요할 경우, <br/>
해당 개념이 파괴된다.

<u>`퍼블릭 인터페이스`를 "**유지**" 하면서 서로 다른 `구현`을 "**추가**"</u>해야 할 땐,<br/>
" `추상 클래스` **상속** " & " `인터페이스` **구현** "를 사용하는 방식이 있다.

( _단순 **"구체 클래스 상속"** 은 강한 결합을 발생시키기 때문에 비권장한다._ )

<br/>

### 인터페이스
세상 모든 구현체들 중 어느 한 가지의 특성만 가진 경우는 찾기 힘들 것이다.<br/>
다양한 개념 및 범주들의 교집합으로서 특정 구현체가 생겨나는 것이 대부분인데

만약 이를 "클래스"와 "상속"을 통해서 객체를 구현하고자 한다면<br/>
"**단일 상속**" 정책에 의해 <u>교집합적인 개념들을 동시 구현</u>하기 **어려울 것**이다.

이럴 경우의 해결 방법이 바로 "**다중 구현**"이 가능한 "`인터페이스`"를 활용하는 것이다.

- 상속으로 인한 **강한 결합도** 문제 회피
- **단일 상속** 제약 회피

더 나아가 "<u>인터페이스 간 상속</u>" 을 통한 <br/>
더 많은 행동을 가진 다른 타입에 의해 "**확장**"까지 가능하다.<br/>
( _ex. `(interface) extends (interface)`_ )

이처럼 " _인터페이스가 다른 인터페이스를 확장하도록 설계_ "하면<br/>
<u>슈퍼 타입과 서브 타입 간의 **타입 계층**</u>을 구성할 수 있다.

<br/>

위 방법에 따라 타입 계층을 설계한 후,<br/>
다양한 타입들에 대해 "`클래스`"라는 수단을 통해<br/>
다양한 **타입(인터페이스)들을 구현**하면 된다.

> - **여러** 클래스 → <u>동일</u>한 타입 구현 : 여러 `서브 타입`
>   - **동일한 메시지에 응답**할 수 있는 <u>타입은 동일하지만 서로 다른 클래스</u>들
> 
> 
> - **단일** 클래스 → <u>여러</u> 타입 구현 : `다중 구현`

⁜ `인터페이스`를 이용해 "**타입**"을 정의하고, `클래스`를 이용해 객체를 "**구현**"하면 <br/>
<u>**상속을 사용하지 않고도** 타입 계층 구현</u>이 가능하다.

- `클래스(Class)` : 객체의 "**구현**" 정의 → 내부 상태 & 오퍼레이션 <br/>
  == " _타입 소속 객체 구현 매커니즘_ "


- `타입(Type)` : **Only** "**인터페이스**" 정의 → 반응할 수 있는 **오퍼레이션 집합**<br/>
  == " _동일한 퍼블릭 인터페이스를 가진 객체들의 범주_ "


- _객체 지향에서 가장 중요한 것이 "협력 안에서 객체가 제공하는 **행동**"인만큼 <br/>
  클래스 자체보단 <u>타입이 더 중요</u>하다._

<br/>

### 추상클래스
> "<u>**추상 클래스**를 상속</u>받는 **구체 클래스**" 추가 → 타입 계층 구현

#### 💡구체 클래스 상속 vs 추상 클래스 상속
1. 의존하는 대상의 **"추상화 정도"**
   - **구체 클래스 상속** → 내부 구현에 강한 결속
   - **추상 클래스 상속** → 추상 메서드에 시그니처에만 의존 (내부 구현 의존 ❌)
     - 오직 추상 메서드 `오버라이딩`에 의존 :: " `의존성 역전의 원칙` "<br/><br/>
   - 모든 자식 클래스들은 항상 추상 메서드의 시그니처 준수 
     - 유연 & 변화에 안정적인 설계
   - 추상화 ⬆ == 결합도 ⬇ == 변경에 의한 영향 ⬇ <br/><br/><br/>
2. 상속을 사용하는 "**의도**"
   - 구체 클래스 상속 → `코드 재사용`
     - _확장에 닫혀있음_
   - 추상 클래스 상속 → `상속`
     - 자기 자신의 **인스턴스 생성** 불가능
     - 오직 <u>**자식 클래스** 추가</u>를 위해 존재<br/><br/>
   - 상속 **계층 확장** 용이 & **결합도** 부작용 방지

<br/>

### 추상 클래스 + 인터페이스
> :: " `골격 구현 추상 클래스` ( _Skeletal Implementation Abstract Class_ ) "

- `인터페이스` → **"다중 상속"** 문제 해결
- `추상 클래스` → **"중복 코드"** 방지

```java
import domain.Money;
import domain.Screening;

public interface DiscountPolicy {
    Money calculateDiscountAmount(Screening screening);
}

/**
 * 골격 구현 추상 클래스 `DefaultDiscountPolicy`
 */
public abstract class DefaultDiscountPolicy implements DiscountPolicy {
    private List<DiscountPolicy> conditions = new ArrayList<>();

    public DefaultDiscountPolicy(DiscountPolicy... conditions) {
        this.conditions = Arrays.asList(conditions);
    }

    @Override
    public Money calculateDiscountAmount(Screening screening) {
        for (DiscountPolicy policy : conditions) {
            if(policy.isSatisfiedBy(screening)) {
                return getDiscountAmount(screening) ;
            }
        }
        return screening.getMovieFee();
    }
    
    abstract protected Money getDiscountAmount(Screening screening);
}

/**
 * `calculateDiscountAmount`의 구현 상속 & 인스턴스 변수 상속
 * - 중복 코드 방지
 * - 하나의 상속 계층으로 묶을 수 있음
 * 
 * 각자 `getDiscountAmount` 에 대해 구현
 * - 시그니처 동일 :: 의존성 역전 원칙 적용 가능
 * - 행동 호환 가능 :: 타입 계층 구현
 */
public class AmountDiscountPolicy extends DefaultDiscountPolicy {
}
public class PercentDiscountPolicy extends DefaultDiscountPolicy {
}
```
<br/>

#### 💡골격 구현 추상 클래스 vs 추상 클래스 
- [장점] <u>**상속 계층**에 얽매이지 않는</u> 타입 계층 & 코드 <u>중복 제거</u>
  - 간단한 새로운 구현 방법 추가
    - 간단하게 새로운 **추상 클래스** 추가 
  - 손쉬운 인터페이스 추가/삭제
    - 기존 부모 클래스에서 **새로운 타입**으로 쉽게 **확장**


- [단점] **복잡도**가 올라감

<br/>

자칫 복잡도가 올라가 불필요하게 <u>이해용이성이 떨어질</u> 수 있기 때문에<br/>
**복잡성 절하**가 더 중요하다면<br/>
타입을 정의하기 위해 인터페이스 / 추상 클래스 **둘 중 하나만 사용**하는 것이 바람직하다.
- **단일** 상속 계층 **가능** : `클래스`/`추상 클래스`로 타입 정의
- **단일** 상속 계층 **불가능** : `인터페이스`로 타입 정의

<br/>
<br/>

---
### 덕 타이핑 - `동적 타입 언어`
> _주로 "**동적 타입 언어**"에서 사용하는 방법_<br/>
> 
> <span style="color:grey">_" 어떤 새가 오리처럼 걷고, 오리처럼 헤엄치며, 오리처럼 꽥꽥 소리를 낸다면 이 새를 오리라고 부를 것이다. "<br/>라는 논리를 사용하는 `덕 테스트(Duck Test)`를 프로그래밍에 적용한 것_</span>

<br/>

✅ `어떤 대상의 행동이 특정 타입과 동일하다면 그것을 해당 타입으로 취급해도 무방하다`<br/>
<br/>
→ " `객체`가 <u>어떤 `인터페이스`에 정의된 **행동**</u>을 **수행**할 수만 있다면 그 객체를 **해당 타입으로 분류** "<br/>
→ " 어떤 클래스가 <u>어떤 인터페이스에 정의된 **오퍼레이션**</u>과 **동일한 시그니처**를 가진 `퍼블릭 메서드`를 포함하고 있다면 **동일한 타입으로 분류** "

<br/>

`런 타임`에 타입을 결정하는 `동적 타입 언어`는<br/>
<u>특정 클래스 상속이나 인터페이스 구현 없이도</u> " **수신할 수 있는 메시지 집합** "을 통해<br/>
객체의 **타입을 결정**할 수 있다.

- `덕 타이핑` : **특정 행동을 수행할 수만 있다면** 해당 객체를 동일한 타입으로 취급할 수 있다.
  - `타입은 '행동'에 대한 것이다.` 라는 본질적 개념에 초점
  - "동일 **행동**" → "동일 **타입**" (_내부 구현이 어떤 방식이든 상관 ❌_ )<br/></br>
  - `덕 타입(Duck Type)` : 특정 클래스에 종속되지 않은 퍼블릭 인터페이스


- `컨텍스트 독립성`(Context Independence) → 유연한 설계
  - "**구현**"이라는 컨텍스트에 대해 `인터페이스`는 **독립적인 코드**를 작성할 수 있게 해줌
    - `인터페이스 구현` > `클래스 상속`


- 단지 메서드의 <u>**시그니처**만 동일</u>하면 "`명시적 타입 선언` 컨텍스트" **제거 가능**
  - "`메시지`**에 대한 의존성**" ⬅ `클래스`/`인터페이스`에 대한 의존성 

<br/>

>✵ _대부분의 `정적 타입 언어`( ex. Java )의 경우,<br/>
<u>두 클래스를 동일한 타입으로 취급</u>하기 위해서는 "**코드** 상의 **타입이 동일하게 선언**"되어야 하고<br/>
**모든 요소**의 "**타입**이 **명시적**으로 **기술**"되어 있어함과 <br/>
더불어 객체의 <u>**"퍼블릭 인터페이스"만**으로 **타입**을 **추측**하는 것</u>이 불가능하기 때문에 <br/>
`덕 타이핑(Duck Typing)`을 활용하기엔 적합하지 않다_

✵ 정적 타입 언어 중에서도 "`C#`의 `dynamic` 키워드" 나 "`C++`의 `템플릿`"을 활용하여<br/>
덕 타이핑을 구현하는 방법이 존재하지만<br/>
이에 따른 장점과 단점이 명확하기에 <u>설계 과정에서의 트레이드 오프</u>가 중요하다.<br/>
( _자세한 내용 : 본 서적 ⌜592pg - 595pg⌟_ )


<br/>
<br/>

---

---
### 믹스인 & 타입 계층
> ⁜`믹스인(mixin)` : 객체 생성 시, <u>코드 일부를 섞어 넣을 수 있도록</u> 만들어진 " **추상 서브** 클래스 "
>
> → 다양한 객체 구현 안에서 <u>동일한 "`행동`"</u>을 **중복 코드 없이 재사용**할 수 있도록

`믹스인(mixin)`을 통해 코드를 재사용하는 객체들<br/>
→ 동일한 "**행동**" 공유<br/>
→ 동일한 "메시지를 수신할 수 있는 **퍼블릭 인터페이스**" 공유<br/><br/>
→ `믹스인(mixin)`을 구현하는 기법 ⇋ `타입`을 정의하는 것

✵ `Scala` : `trait` / `Java` - `Comparable<T>` 인터페이스(**믹스인 인터페이스**)

<br/>

Java에 Scala의 `trait` 와 유사한 방식으로 사용 가능한 기능이 추가되긴 했다.<br/>
바로 "<u>인터페이스</u>에 사전 구현해 놓는 `디폴트 메서드(Default Method)`"이다.

- **인터페이스**에 <u>"메서드의 **기본 구현**"을 추가</u>하는 것 허용
  - " **코드 재사용** "(추상클래스) & " **상속 계층에 얽매이지 않는 것** "(인터페이스)
  - 해당 인터페이스를 **구현하는 모든 클래스들**은 <u>해당 메서드의 **구현 제공** 필수</u>

<br/>

하지만 한계가 분명하다.
- `인터페이스` 내 정의 → **클래스** 내부 `퍼블릭 메서드`로 구현 필수
  - 캡슐화 약화
    - 클래스 내부 정보 공개
    - 접근 허용
  - 불필요한 퍼블릭 인터페이스 요소 증가


- **<u>완벽한</u> 코드 중복 제거** 불가능

> 📍 `디폴트 메서드`의 목적
> 
> : " _**기존**에 널리 사용되는 **인터페이스**_ "에 **새로운 오퍼레이션 추가**할 때 발생하는<br/>
> `하위 호환성` 문제 해결<br/>
> (_추상 클래스 역할 대체 ❌_)