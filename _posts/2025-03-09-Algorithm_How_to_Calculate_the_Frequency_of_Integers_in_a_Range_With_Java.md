---
title: Algorithm - How to Calculate the Frequency of Integers in a Range With Java
author: Doorimul-KJH
date: 2025-03-09 00:00:00 +0900
categories: [Blogging, Algorithm]
tags: [blogging, Algorithm]
pin: true
# image: https://doorimul-kjh.github.io/assets/
render_with_liquid: false
---

# Java 프로그램 과제: 범위 내 정수 개수 출력

## 문제 설명
사용자가 원하는 개수의 정수값을 입력받아, 사용자가 원하는 범위 내의 정수값이 각각 몇 개인지 출력하는 Java 프로그램을 작성하세요.

## 입력
- **정수 개수 (n)**
- **범위 (lb, ub)**: 하한(lb)와 상한(ub)
- **n개의 정수**

> *참고: n, lb, ub가 타당하지 않은 값으로 입력되는 경우는 고려하지 않아도 됩니다.*

## 출력
- **lb ~ ub 범위의 정수 개수**  
  각 정수별 빈도를 공백으로 구분하여 한 줄에 출력합니다.

## 실행 예 (console)
```java
 10 -2 2 // 입력 n, lb, ub
 -3 -2 1 3 4 -2 -2 99 -99 1 // n개의 정수
 3 0 0 2 0 // lb ~ ub 범위의 정수 개수
```

## 목적
- 입출력, 배열, 반복문, 조건문을 연습하여 알고리즘 구현 문제 수행을 준비한다.

## 유의사항
- 들여쓰기를 철저히 할 것
- 식별자 이름을 자바 관례에 맞게 적절히 붙일 것
- 입출력 형식을 실행 예와 동일하게 맞출 것

---

## 알고리즘 풀이 과정

이 문제를 해결하기 위해 두 가지 방법으로 접근해 보았습니다.  
먼저 기존의 알고리즘을 도식화하여 표현하고, 이후 개선한 알고리즘을 다시 도식화하여 설명하겠습니다.

### 기존 알고리즘 순서
```
1. Scanner 객체 생성
2. 정수 개수(n), 하한(lb), 상한(ub) 입력받기
3. 입력받은 정수의 개수(n)만큼 배열(numbers)을 생성
4. 배열에 n개의 정수를 모두 저장
5. lb부터 ub까지 반복하면서,
   5-1. 현재 값(value)을 기준으로 배열 전체를 탐색하며 같은 값을 찾기
   5-2. 같은 값이 있으면 count 증가
   5-3. 탐색이 끝나면 count 값을 출력
```

### 기존 코드 (Java)

```java
import java.util.Scanner;  // Scanner 클래스 불러오기


public class RangeFrequencyCounter {
    public static void main(String[] args) {
    // Scanner 객체 생성
    Scanner scanner = new Scanner(System.in);

    // 정수 개수 n, 범위의 하한 lb와 상한 ub를 입력받음
    int n = scanner.nextInt();
    int lb = scanner.nextInt();
    int ub = scanner.nextInt();

    // n개의 정수를 저장할 배열 생성
    int[] numbers = new int[n];

    // n개의 정수를 입력받아 배열에 저장
    for (int i = 0; i < n; i++) {
        numbers[i] = scanner.nextInt();
    }

    // lb부터 ub까지 각 정수의 빈도 계산 후 출력
    for (int value = lb; value <= ub; value++) {
        int count = 0;
        for (int i = 0; i < n; i++) {
            if (numbers[i] == value) {
                count++;
            }
        }
        System.out.print(count + " ");
    }
    //줄바꿈
    System.out.println();

    // Scanner 객체 종료
    scanner.close();
    }
}
```

---

### 개선된 알고리즘 순서
```
1. Scanner 객체 생성
2. 정수 개수(n), 하한(lb), 상한(ub) 입력받기
3. (상한 - 하한 + 1) 크기만큼 빈도 배열(freq) 생성
4. n번 반복하면서 입력받은 값(number)이 범위(lb~ub)에 있는지 확인
   4-1. number가 범위 내에 있다면 freq[number - lb]의 값을 1 증가
5. 빈도 배열(freq)의 값을 순서대로 출력
```

### 개선 코드 (Java)

```java
import java.util.Scanner;  // Scanner 클래스 불러오기


public class RangeFrequencyCounter {
    public static void main(String[] args) {
    // Scanner 객체 생성
    Scanner scanner = new Scanner(System.in);

    // 정수 개수 n, 범위의 하한 lb와 상한 ub 입력받기
    int n = scanner.nextInt();
    int lb = scanner.nextInt();
    int ub = scanner.nextInt();

    // lb~ub까지의 빈도 수를 저장할 배열 생성
    int[] freq = new int[ub - lb + 1];

    // 입력받는 정수마다 바로 빈도 수 계산
    for (int i = 0; i < n; i++) {
        int number = scanner.nextInt();
        if (lb <= number && number <= ub) {
            freq[number - lb]++;
        }
    }

    // 빈도 배열 출력
    for (int i = 0; i < freq.length; i++) {
        System.out.print(freq[i] + " ");
    }
    System.out.println();

    // Scanner 종료
    scanner.close();
    //줄바꿈
    System.out.println();

    // Scanner 객체 종료
    scanner.close();
    }
}
```

---

## 알고리즘 개선에 대한 생각

기존 알고리즘에서는 입력된 정수들을 배열에 **모두 저장**하고, 매번 범위 내 각 정수의 개수를 세기 위해 배열 전체를 탐색해야합니다. 이 방식은 단순하지만, 매번 반복적으로 배열 전체를 확인해야 한다는 비효율성이 있습니다.

개선된 알고리즘에서는 입력받은 값을 배열에 저장하는 단계를 생략하고, 바로 빈도 배열을 이용해 범위 내 정수 빈도를 계산했습니다. 이로 인해 약간 더 효율적인 알고리즘이 되었습니다.

하지만, 향후 다른 작업을 추가적으로 수행해야 한다면 입력된 정수들을 모두 저장하는 **기존 방식이 더 유연하게 대응할 수 있는 장점**도 있습니다. 예를 들어, 나중에 중복 값을 제거하거나, 입력된 값을 다시 활용해야 하는 경우라면 기존의 방식이 더 적합할 수 있습니다.