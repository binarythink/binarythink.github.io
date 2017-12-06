---
permalink: /junit4-parameterized-test
title: "junit4를 이용한 파라미터화 테스트"
excerpt: "다수개의 파라미터를 사용하여 테스트를 여러 번 반복 실행하는 기능을 제공하는 `Parameterized` 러너를 사용하는 방법에 대해서 알아봅니다."
header:
  #image: /assets/images/common/teaser.jpg
  og_image: /assets/images/content/2017/01/junit4-parameterized-test/teaser.png
  overlay_image: /assets/images/content/2017/01/junit4-parameterized-test/teaser.png
  overlay_filter: 0.5
categories:
  - java
tags: 
  - junit
  - 단위테스트
  - 파라미터화테스트
  - Parameterized.class
toc : true
---

다양한 파라미터를 사용하여 테스트를 여러 번 반복 실행하는 기능을 제공하는 `Parameterized` 러너를 사용하는 방법에 대해서 알아봅니다.

## Parameterized 러너

커스텀 러너인 `Parameterized.class` 는 파라미터화 테스트를 구현합니다.
파라미터화 테스트가 수행되면 러너는 입력된 파라미터의 `@Parameters` 에서 반환하는 컬랙션의 길이 만큼 반복 테스트를 수행합니다.


## 주요 클래스 및 어노테이션

`org.junit.runners.Parameterized.class`
: 파리미터화 테스트를 구현한 러너

`@Parameters`
: 테스트를 수행할 다수개의 파라미터를 `Collection<T[]>` 타입으로 반환하는 **정적 메서드(`static`)** 위에 선언합니다.

`@Parameter`
: `@Parameters` 에서 제공하는 다수개의 파라미터를 주입합니다.


## 케이스 스터디

공식 junit 팀의 juni4 레파지토리의 위키에서 파라미터화 테스트를 구현하는 방법은 총 2가지가 소개되고 있습니다.[^1]

[^1]: [https://github.com/junit-team/junit4/wiki/parameterized-tests](https://github.com/junit-team/junit4/wiki/parameterized-tests)

케이스 스터디에서는 간단하게 두 값을 더해 `expected` 값과 같은가를 비교하는 파라미터화 테스트를 하고 있으며, 위키에서 소개한 2가지 방법(생성자를 이용한 방법, `@Parameter` 주입을 이용한 방법)을 가지고 작성되었습니다.


### 생성자를 이용한 파라미터 주입
```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.junit.runners.Parameterized;
import org.junit.runners.Parameterized.*;

import java.util.Arrays;
import java.util.Collection;

import static org.junit.Assert.*;

/**
 * 생성자를 이용한 파라미터 주입
 */
@RunWith(Parameterized.class)
public class ParameterizedTestCase1 {
    // 생성자를 사용하는 경우 맴버 변수는 private 여도 무관
    private int expected;
    private int valueOne;
    private int valueTwo;

    // @Parameters 에서 반환하는 배열의 길이와 생성자의 파라미터수가 같아야 함
    // 주입은 배열의 순서와 파라미터의 순서와 같음
    // expected = 2, valueOne = 1, valueTwo = 1
    // expected = 3, valueOne = 2, valueTwo = 1
    // expected = 4, valueOne = 3, valueTwo = 1
    public ParameterizedTestCase1(int expected, int valueOne, int valueTwo) {
        this.expected = expected;
        this.valueOne = valueOne;
        this.valueTwo = valueTwo;
    }

    // 입력할 파라미터들을 넣어줍니다
    // 반드시 Collection<T[]> 를 반환하는 정적 메서드여야 함
    @Parameters
    public static Collection<Object[]> getTestParameters() {
        return Arrays.asList(new Object[][]{
                {2, 1, 1},
                {3, 2, 1},
                {4, 3, 1}
        });
    }

    @Test
    public void sum() {
        assertEquals(expected, valueOne + valueTwo);
    }
}
```
* 파라미터화 테스트 수행을 위해 `@RunWith(Parameterized.class)` 를 선언해 테스트 러너를 지정해줍니다.
* `@Parameters`라 표시된 **정적 메서드**가 하나 필요합니다. 이 메서드의 시그너처는 반드시 `public static java.util.Collection<T[]> methodName()` 이어야 하며, 이 정적 메서드는 어떤 파라미터도 입력받아서는 안됩니다.
* `Collection` 의 원소는 배열이고, 길이는 모두 같아야 합니다. 테스트 클래스의 `public 생성자`가 받는 파라미터의 수와도 일치해야 합니다.
* `public 생성자`의 파라미터의 순서에 따라 `Collection` 의 배열의 순서대로 값이 주입됩니다.

### 동작 순서
1. `@Parameters` 가 선언된 정적 메서드인 `getTestParameters()`를 호출해 컬렉션 객체를 얻는다.
1. 컬렉션에 저장된 배열의 수 만큼 반복한다.
    1. 유일한 public 생성자를 찾아 생성자에 배열의 원소를 파라미터로 넣어 호출한다.
    1. `@test` 메서드를 호출한다.


### @Parameter 를 이용한 파라미터 주입
```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.junit.runners.Parameterized;
import org.junit.runners.Parameterized.*;

import java.util.Arrays;
import java.util.Collection;

import static org.junit.Assert.*;

/**
 * @Parameter 를 이용한 파라미터 주입
 */
@RunWith(Parameterized.class)
public class ParameterizedTestCase2 {
    /*
    어노테이션을 사용하는 경우 맴버 변수는 public 이여야 합니다.
    */
    //Parameter.value 의 기본값이 0 이므로 @Parameter 와 같이 사용해도 무관함니다.
    @Parameter(0)
    public int expected;
    @Parameter(1)
    public int valueOne;
    @Parameter(2)
    public int valueTwo;

    // 입력할 파라미터들을 넣어줍니다
    // 반드시 Collection<T[]> 를 반환하는 정적 메서드여야 함
    @Parameters
    public static Collection<Object[]> getTestParameters() {
        return Arrays.asList(new Object[][]{
                {2, 1, 1},
                {3, 2, 1},
                {4, 3, 1}
        });
    }

    @Test
    public void sum() {
        assertEquals(expected, valueOne + valueTwo);
    }
}
```
* 파라미터화 테스트 수행을 위해 `@RunWith(Parameterized.class)` 를 선언해 테스트 러너를 지정해줍니다.
* `@Parameters`라 표시된 **정적 메서드**가 하나 필요합니다. 이 메서드의 시그너처는 반드시 `public static java.util.Collection<T[]> methodName()` 이어야 하며, 이 정적 메서드는 어떤 파라미터도 입력받아서는 안됩니다.
* `Collection` 의 원소는 배열이고, 길이는 모두 같아야 합니다. 테스트 클래스의 `public 생성자`가 받는 파라미터의 수와도 일치해야 합니다.
* `@Parameter` 의 `value` 에 선언된 숫자에 따라 `Collection` 의 배열의 `index`에 있는 값이 주입됩니다.

### 동작 순서
1. `@Parameters` 가 선언된 정적 메서드인 `getTestParameters()`를 호출해 컬렉션 객체를 얻는다.
1. 컬렉션에 저장된 배열의 수 만큼 반복한다.
    1. `@Parameter(n)` 가 선언된 public 맴버변수를 찾아 배열의 원소에 n 번째 값을 주입한다.
    1. `@test` 메서드를 호출한다.


## 참고자료
* [https://github.com/junit-team/junit4/wiki/parameterized-tests](https://github.com/junit-team/junit4/wiki/parameterized-tests)
* 피터 타치브 외 .『JUnit in Action』. 인사이트, 2011.