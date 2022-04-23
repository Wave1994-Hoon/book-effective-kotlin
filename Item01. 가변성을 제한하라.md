# Item01. 가변성을 제한하라
## 1. 코틀린은 모듈로 프로그램을 설계한다.
모듈은 클래스, 객체, 함수, type alias, top level property 등 다양한 요소로 구성

<br>

## 2. 예제 코드
```kotlin
class BankAccount {
    var balance: Double = 0.0
        private set

    fun deposit(depositAmount: Double) {
        balance += depositAmount
    }

    @Throws(InsufficientFunds::class)
    fun withdraw(withdrawAmount: Double) {
        if (balance < withdrawAmount) {
            throw InsufficientFunds()
        }
        balance -= withdrawAmount;
    }
}

// 객체가 상태를 가지게 되는 것은 위험하다.

class InsufficientFunds : Exception()
val account = BankAccount()

println(account.balance) // expect: 00

account.deposit(100.0)
println(account.balance) // expect: 100.0

account.deposit(50.0)
println(account.balance) // expect: 50.0
```

## 3. 상태를 적절하게 관리하기 어려운 이유
#### (1) 프로그램을 이해하고 디버그가 힘들다.
상태를 갖는 부분들의 관계를 이해하고, 상태 변경이 많아지면 추척하는 것이 힘들다.

#### (2) 가변성이 있으면 코드의 실행 추론이 어렵다.
현재 어떤 값을 갖고 있는지 알아야 코드의 실행을 예측할 수 있다.

#### (3) 멀티스레드 프로그램일 때는 적절한 동기화가 필요하다.
변경이 일어나는 모든 부분에서 충돌이 발생할 수 있다.

#### (4) 테스트하기가 어렵다. -> 모든 상태를 테스트해야함
모든 상태를 테스트해야하기 때문에, 변경이 많으면 많을수록 더 많은 조합을 테스트해야한다.

#### (5) 상태 변경이 일어날 때, 이러한 변경을 다른 부분에 알려야 하는 경우가 있다.
예를 들어 정렬되어 있는 리스트에 가변 요소를 추가한다면, 요소에 변경이 일어날 때마다 리스트 전체를 다시 정렬해야 한다.

<br>

## 4. 코틀린에서 가변성 제한하기
#### 정리
1. 읽기 저뇽 프로퍼티 (val)

2. 가변 컬렉션과 읽기 전용 컬렉션 구분하기

3. 데이터 클래스의 COPY


#### 읽기 저뇽 프로퍼티 (val)

#### 가변 컬렉션과 읽기 전용 컬렉션 구분하기

#### 데이터 클래스의 COPY

<br>

## 5. 다른 종류의 변경 가능 지점


