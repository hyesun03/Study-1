4장 주석
========

#### &nbsp;잘 달린 주석은 그 어떤 정보보다 유용하다. 경솔하고 근거 없는 주석은 코드를 이해하기 어렵게 만든다. 우리에게 프로그래밍 언어를 치밀하게 사용해 의도를 표현할 능력이 있다면, 주석은 거의 필요하지 않다.

* ## 주석은 나쁜 코드를 보완하지 못한다
#### &nbsp;코드에 주석을 추가하는 일반적인 이유는 코드 품질이 나쁘기 때문이다. 주석을 달지 말고, 코드를 정리해야 한다. 표현력이 풍부하고 깔끔하며 주석이 거의 없는 코드가, 복잡하고 어수선하며 주석이 많이 달린 코드보다 훨씬 좋다.

* ## 코드로 의도를 표현하라
#### &nbsp;확실히 코드만으로 의도를 설명하기 어려운 경우가 존재한다.
```java
// 직원에게 복지 혜택을 받을 자격이 있는지 검사한다.
if((employee.flags & HOURLY_FLAG) && (employee.age > 65))

if(employee.idEligibleForFullBenefits())
```
#### 하지만 이와같이 많은 경우 주석으로 달려는 설명을 함수로 만들어 표현해도 충분하다.

* ## 좋은 주석
#### &nbsp;어떤 주석은 필요하거나 유익하다. 다음은 글자 값을 한다고 생각하는 주석 몇 가지다.

### 01. 법적인 주석
#### &nbsp;각 소스 파일 첫머리에 주석으로 들어가는 저작권 정보와 소유권 정보는 필요하고도 타당하다.

### 02. 정보를 제공하는 주석
#### &nbsp;기본적인 정보를 주석으로 제공하면 편리하다. 예를 들어, 추상 메서드가 반환할 값을 설명하는 경우가 있다.

### 03. 의도를 설명하는 주석
#### &nbsp;때때로 주석은 의도까지 설명 한다.

### 04. 의미를 명료하게 밝히는 주석
#### &nbsp;인수나 반환값이 표준 라이브러리나 변경하지 못하는 코드에 속한다면 의미를 명료하게 밝히는 주석이 유용하다.

### 05. 결과를 경고하는 주석
#### &nbsp;때로 다른 프로그래머에게 결과를 경고할 목적으로 주석을 사용한다. 예를 들어, 특정 테스트 케이스를 꺼야하는 이유를 설명하는 주석이다.
```java
// 여유 시간이 중분하지 않다면 실행하지 마십시오.
public void _testWithReallyBigFile() {
  writeLinesToFile(1000000);
  ...
}
```

### 06. TODO 주석
#### &nbsp;때로는 앞으로 할 일을 TODO 주석으로 남겨주면 편하다. 하지만 TODO로 떡칠한 코드는 바람직하지 않다.

### 07. 중요성을 강조하는 주석
#### &nbsp;대수롭지 않다고 여겨질 뭔가의 중요성을 강조하기 위해서도 주석을 사용한다.

### 08. 공개 API에서 Javadocs
#### &nbsp;설명이 잘 된 공개 API는 참으로 유용하고 만족스럽다.

* ## 나쁜 주석
#### &nbsp;대다수 주석이 이 범주에 속한다.

### 01. 주절거리는 주석
#### &nbsp;특별한 이유 없이 의무감으로 혹은 프로세스에서 하라고 하니까 마지못해 주석을 단다면 전적으로 시간낭비다.

### 02. 같은 이야기를 중복하는 주석
#### &nbsp;다음 함수는 헤더에 달린 주석이 같은 코드 내용을 그대로 중복한다. 자칫하면 코드보다 주석을 읽는 시간이 더 오래 걸린다.
```java
// this.closed 가 true일 때 반환되는 유틸리티 메서드다.
// 타임아웃에 도달하면 예외를 던진다.
public synchronized void waitForClose(final long timeoutMillis) throws Exception {
  if(!closed) {
    wait(timeoutMillis);
    if(!closed)
      throw new Exception("MockResponseSender could not be closed");
  }
}
```

### 03. 오해할 여지가 있는 주석
#### &nbsp;중복이 있으면서도 오해할 여지가 있는 주석은 쓰면 안된다.

### 04. 의무적으로 다는 주석
#### &nbsp;모든 함수에 Javadocs를 달거나 모든 변수에 주석을 달아야 한다는 규칙은 어리석기 그지없다.

### 05. 이력을 기록하는 주석
#### &nbsp;때때로는 사람들이 모듈을 편집할 때마다 모듈 첫머리에 주석을 추가한다. 이제는 VCS가 있으니까 사용하지 말아야 한다.

### 06. 있으나 마나 한 주석
#### &nbsp;너무 당연한 사실을 언급하며 새로운 정보를 제공하지 못하는 주석이다.

### 07. 무서운 잡음
#### &nbsp;때로는 Javadocs도 잡음이다. 단지 문서를 제공해야 한다는 잘못된 욕심으로 탄생한 잡음일 뿐이다.

### 08. 함수나 변수로 표현할 수 있다면 주석을 달지 마라
#### &nbsp;주석이 필요하지 않도록 코드를 개선하는 편이 더 좋다.

### 09. 위치를 표시하는 주석
#### &nbsp;소스 파일에서 특정 위치를 표시하려 주석을 사용한다.
```java
// Actions////////////////////////////////////////
```
#### 극히 드물지만 위와 같은 배너 아래 특정 기능을 모아 놓으면 유용한 경우도 있긴 있다. 하지만 반드시 필요할 때만, 아주 드물게 사용하는 편이 좋다.

### 10. 닫는 괄호에 다는 주석
#### &nbsp;때로는 프로그래머들이 닫는 괄호에 특수한 주석을 달아놓는다. 작고 캡슐화된 함수에서는 잡음일 뿐이다.

### 11. 공로를 돌리거나 저자를 표시하는 주석
#### &nbsp;소스 코드 관리 시스템은 누가 언제 무엇을 추가했는지 귀신처럼 기억한다. 저자 이름으로 코드를 오염시킬 필요가 ㅇ벗다.

### 12. 주석으로 처리한 코드
#### &nbsp;소스 코드 관리 시스템이 우리를 대신해 코드를 기억해준다. 이제는 주석으로 처리할 필요가 없다.

### 13. HTML 주석
#### &nbsp;HTML 주석은 IDE에서조차 읽기가 어렵다. 사용하지 말자.

### 14. 전역 정보
#### &nbsp;주석을 달아야 한다면 근처에 있는 코드만 기술하라. 코드 일부에 주석을 달면서 시스템의 전반적인 정보를 기술하지 마라.

### 15. 너무 많은 정보
#### &nbsp;주석에다 흥미로운 역사나 관련 없는 정보를 장황하게 늘어놓지 마라.

### 16. 모호한 관계
#### &nbsp;주석과 주석이 설명하는 코드는 둘 사이 관계가 명백해야 한다.

### 17. 함수 헤더
#### &nbsp;짧은 함수는 긴 설명이 필요 없다. 짧고 한 가지만 수행하며 이름을 잘 붙인 함수가 주석으로 헤더를 추가한 함수보다 훤씬 좋다.

### 18. 비공개 코드에서 Javadocs
#### &nbsp;공개하지 않을 코드라면 Javadocs는 쓸모가 없다.

* ## 마무리
#### &nbsp;주석을 지양하는 편이 좋다고 생각하지만, 내가 속해있는 조직의 규칙을 따라야 한다.