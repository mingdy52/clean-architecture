# 6장 함수형 프로그래밍
이 패러다임에서 핵심이 되는 기반은 람다 계산법이다.

## 정수를 제곱하기

- 함수형 언어에서 변수는 변경되지 않음
    - 자바: 가변변수를 사용(e.g. for - i)
        - 가변변수: 프로그램 사용 중에 상태가 변할 수 있음
    - 클로저: 함수형 언어
        - 가변변수가 전혀 없음
        - => 함수형 언어의 변수는 변경되지 않음

## 불변성과 아키텍처
- 아키텍처 고려시 변수의 가변성을 고려하는 이유
    - 가변변수로 인한 문제: 경합, 조건, 교착상태 조건, 동시 업데이트
        - 락(lock)이 가변적이지 않다면, 교착상태 미발생
        - 변수가 갱신되지 않으면, 경합조건이나 동시 업데이트 문제 미발생

## 가변성의 분리
위의 내용과 관련된 주요한 타협은 **가변 컴포넌트**와 **불변 컴포넌트**로 **분리**하는 일
```mermaid
graph  TD
	S1[불변 컴포넌트] --> V[가변 컴포넌트]
	S2[불변 컴포넌트] --> V
	S3[불변 컴포넌트] --> V

	V <--> T[트랜잭션 메모리]

	style  S1  fill:#B4C6E7,stroke:#1C4587,stroke-width:2px
	style  S2  fill:#B4C6E7,stroke:#1C4587,stroke-width:2px
	style  S3  fill:#B4C6E7,stroke:#1C4587,stroke-width:2px
	style  V  fill:#FFD966,stroke:#B45F06,stroke-width:2px
	style  T  fill:#D9EAD3,stroke:#38761D,stroke-width:2px
```

상태변경은 컴포넌트를 갖가지 동시성 문제를 노출하는 일
- 동시 업데이트와 경합 조건 문제로부터 가변 변수 보호: 트랜잭션 메모리를 이용
- 예시: 클로저의 atom

애플리케이션을 구조화하려면, 변수를 변경하는 컴포넌트와 변경하지 않는 컴포넌트를 분리해야 함

## 이벤트 소싱
더 많은 메모리와 기계가 빨라질 수록, 필요한 가변 상태는 점점 줄어듬
-> 이벤트소싱은 상태(값)이 아닌 트랜잭션을 저장하자는 전략

이벤트 소싱
- 많은 데이터 저장소가 필요함
- 데이터 저장소에서 삭제되거나 변경되는 것이 하나도 없음
- 모든 애플리케이션은 CRUD가 아닌 CR만 수행
- 동시 업데이트 문제 미발생: 데이터 저장소에서 변경, 삭제가 발생하지 않아서
- 이벤트 소싱의 예시: 소스 코드 버전 관리 시스템

## 결론
- 요약
    - 구조적 프로그래밍은 제어 흐름의 직접적인 전환에 부과되는 규율이다.
    - 객체 지향 프로그래밍은 제어 흐름의 간접적인 전환에 부과되는 규율이다.
    - 함수형 프로그래밍은 변수 할당에 부과되는 규율이다.

- 소프트웨어 핵심: 순차, 분기, 반복, 참조로 구성된다.