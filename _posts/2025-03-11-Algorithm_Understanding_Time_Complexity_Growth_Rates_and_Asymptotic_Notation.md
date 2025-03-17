---
title: Algorithm - Understanding Time Complexity, Growth Rates, and Asymptotic Notation
author: Doorimul-KJH
date: 2025-03-10 00:00:00 +0900
categories: [Blogging, Algorithm]
tags: [blogging, Algorithm]
pin: true
render_with_liquid: false
use_math: true
---

# 주제: 알고리즘 분석과 재귀 알고리즘 이해하기

이번 포스팅에서는 알고리즘을 분석하는 방법, 특히 **시간 복잡도**와 **재귀(Recursion)**에 대해 알아보겠습니다.

---

## 1. 알고리즘 분석이란?

알고리즘 분석이란 주어진 문제에 대해 알고리즘을 설계한 후, 해당 알고리즘이 얼마나 효율적인지 평가하는 과정입니다. 같은 문제를 해결하는 알고리즘이라 하더라도 효율성은 매우 큰 차이를 보일 수 있습니다.

예를 들어 n개의 자료를 정렬할 때:

- $(n^2)$에 비례하는 시간이 걸리는 알고리즘(버블 정렬 등)이 있고,
- $(n log_2 n)$에 비례하는 시간이 걸리는 알고리즘(병합 정렬 등)이 있습니다.

알고리즘을 분석함으로써 자원과 시간을 효율적으로 사용할 수 있는 최적의 알고리즘을 선택할 수 있습니다.

---

## 2. 시간 복잡도(Time Complexity)란?

알고리즘이 문제를 해결하는 데 소요되는 시간을 **입력 크기(n)에 따라** 표현한 것을 **시간 복잡도**라고 합니다. 주로 다음과 같은 방식으로 시간 복잡도를 계산합니다.

- 반복문(`for`, `while`)의 반복 횟수
- 특정 코드 라인의 수행 횟수
- 함수의 호출 횟수

---

## 2-1. 시간 복잡도의 다양한 예시

### 예시 1: 시간 복잡도 $O(1)$ – 상수 알고리즘
```java
public class ConstantTime {
    public static void main(String[] args) {
        int[] array = {10, 20, 30};
        System.out.println(array[0]);
    }
}
```
배열의 특정 원소에 접근하는 것은 항상 동일한 시간이 걸립니다.

### 예시 2: 시간 복잡도 $O(n)$ – 선형 알고리즘
```java
public class LinearAlgorithm {
    public static void main(String[] args) {
        int[] array = {10, 20, 30, 40, 50};
        for (int i = 0; i < array.length; i++) {
            System.out.println(array[i]);
        }
    }
}
```
배열의 길이(n)만큼 시간이 걸리는 알고리즘입니다.

### 예시 3: 시간 복잡도 $O(log_2 n)$ – 이진 탐색 알고리즘
```java
public class BinarySearchExample {
    public static void main(String[] args) {
        int[] array = {10, 20, 30, 40, 50};
        int item = 30;
        int index = binarySearch(array, 0, array.length - 1, item);
        System.out.println(index);
    }

    private static int binarySearch(int[] array, int from, int to, int item) {
        if (from > to) return -1;

        int mid = (from + to) / 2;

        if (array[mid] == item) {
            return mid;
        } else if (array[mid] > item) {
            return binarySearch(array, from, mid - 1, item);
        } else {
            return binarySearch(array, mid + 1, to, item);
        }
    }
}
```
이진 탐색 알고리즘은 탐색 범위를 절반씩 줄여가기 때문에 수행 시간이 $( log_2 n )$에 비례합니다.

### 예시 4: 시간 복잡도 $O(n log_2 n)$ – 병합 정렬 알고리즘
```java
public class MergeSortExample {
    public static void main(String[] args) {
        int[] array = {50, 20, 40, 10, 30};
        mergeSort(array, 0, array.length - 1);
    }

    private static void mergeSort(int[] array, int left, int right) {
        if (left < right) {
            int mid = (left + right) / 2;
            mergeSort(array, left, mid);
            mergeSort(array, mid + 1, right);
            merge(array, left, mid, right);
        }
    }

    private static void merge(int[] array, int left, int mid, int right) {
        // 병합 과정 로직 (생략)
    }
}
```

병합 정렬은 데이터를 반으로 나눠 각각 정렬하고 다시 합치는 방식으로, 수행 시간이 $(n log_2 n)$에 비례하여 증가합니다.

### 예시 5: 시간 복잡도 $O(n^2)$ – 이중 반복문 알고리즘
```java
public class QuadraticAlgorithm {
    public static void main(String[] args) {
        int[] array = {10, 20, 30};
        for (int i = 0; i < array.length; i++) {
            for (int j = 0; j < array.length; j++) {
                System.out.println("Pair: " + array[i] + ", " + array[j]);
            }
        }
    }
}
```
반복문이 중첩되면 시간은 배열 크기의 제곱에 비례하여 증가합니다.

---

## 3. 재귀(Recursion) 알고리즘이란?

재귀(Recursion)란 함수가 자기 자신을 호출하는 구조를 말하며, 복잡한 문제를 작은 문제로 나누어 해결하는데 매우 유용합니다. 

### 재귀의 대표적인 예시

#### 예시 1: 팩토리얼(Factorial) 알고리즘
```java
public class FactorialExample {
    public static void main(String[] args) {
        int result = factorial(5); // 5의 팩토리얼 구하기
        System.out.println(result);
    }

    private static int factorial(int n) {
        if (n == 0) return 1; // 재귀의 종료 조건
        return n * factorial(n - 1); // 재귀 호출
    }
}
```

#### 예시 2: 피보나치(Fibonacci) 수열 알고리즘
```java
public class FibonacciExample {
    public static void main(String[] args) {
        int result = fibonacci(5); // 피보나치 수열 5번째 항 구하기
        System.out.println(result);
    }

    private static int fibonacci(int n) {
        if (n <= 1) return n; // 종료 조건
        return fibonacci(n - 1) + fibonacci(n - 2); // 재귀 호출
    }
}
```

이처럼 재귀는 작은 문제로 나누어 접근하기 쉬운 형태로 구성할 수 있습니다.

---

### 4. 시간 복잡도와 증가율 비교

알고리즘의 성능을 비교할 때 중요한 개념이 **시간 복잡도(Time Complexity)**입니다. 시간 복잡도는 **입력 크기(n)**가 증가함에 따라 **알고리즘의 수행 시간이 어떻게 변하는지**를 분석하는 개념입니다.

---

### 4.1 알고리즘의 분석

알고리즘을 분석할 때, 문제의 크기에 따라 알고리즘의 성능이 다르게 평가될 수 있습니다.

#### **1) 크기가 작은 문제**
- 알고리즘의 효율성이 크게 중요하지 않습니다.
- 입력 크기가 작다면, 비효율적인 알고리즘이라도 실행 속도가 충분히 빠를 수 있습니다.

#### **2) 크기가 충분히 큰 문제**
- 알고리즘의 효율성이 중요한 요소가 됩니다.
- 비효율적인 알고리즘을 사용할 경우 실행 시간이 급격히 증가하여 문제가 발생할 수 있습니다.

이처럼 **입력 크기가 커질수록 알고리즘의 효율성을 고려해야 하기 때문에, 점근적 분석(Asymptotic Analysis)이 필요합니다.**

---

### 4.2 점근적 분석 (Asymptotic Analysis)

**점근적 분석**은 **입력 크기 $( n )$이 충분히 큰 경우**에 대한 알고리즘의 성능을 평가하는 방법입니다. 

- 작은 ( n )에서는 알고리즘의 성능 차이가 크게 보이지 않을 수도 있지만, **$( n )$이 커지면 알고리즘의 성능 차이가 극명하게 나타납니다.**
- 특정한 상수 계수는 분석에서 무시되며, **$( n )$의 차수(비율)에 따라 알고리즘의 성능을 비교**합니다.

---

### 4.3 알고리즘 시간 복잡도 비교: $ O(n^2 / 5) $ vs. $ O(5n) $

#### **시간 복잡도가 $ O(n^2 / 5) $인 알고리즘 A와 $ O(5n) $인 알고리즘 B를 비교해 보겠습니다.**
- 알고리즘 A: **$( T_A(n) = O(n^2 / 5) )$**
- 알고리즘 B: **$( T_B(n) = O(5n) )$**

입력 크기 $( n )$이 작을 때는 **$( n^2 / 5 )$**이 $( 5n )$보다 작을 수도 있습니다. 하지만, $( n )$이 충분히 커지면 다음과 같은 현상이 발생합니다.

우리는 다음 부등식을 만족하는 $( n )$을 찾을 수 있습니다. $[
frac{n^2}{5} > 5n
]$


양변에 5를 곱하면: $[
n^2 > 25n
]$


양변을 $( n )$으로 나누면: $[
n > 25
]$



즉, $( n )$이 **25보다 커지면** 알고리즘 A의 실행 시간이 알고리즘 B보다 더 오래 걸리게 됩니다. 

이 의미는 작은 입력에서는 알고리즘 A가 더 빠를 수 있지만, **입력 크기 $( n )$이 25를 초과하면 $ O(n^2 / 5) $의 증가 속도가 급격히 빨라져 실행 시간이 $ O(5n) $보다 더 느려진다는 것**을 의미합니다. 이처럼 **점근적 분석을 통해 특정한 입력 크기 이상에서 성능 차이가 극명하게 드러난다는 사실을 확인할 수 있습니다.**

---

## 5. 점근적 표기법 (Asymptotic Notation)

점근적 표기법은 **입력 크기가 충분히 커질 때 알고리즘의 성능을 표현하는 방법**입니다. 대표적인 표기법으로 **빅-오 표기법 $( O )$**, **빅-오메가 표기법 $( Omega )$**, **세타 표기법 $( Theta )$**, 그리고 **리틀-오 표기법 $( o )$**, **리틀-오메가 표기법 $( omega )$**가 있습니다.

---

### 5.1 점근적 표기의 종류

| 표기법 | 의미 | 개념 비교 |
|--------|-------------------------|------------------|
| **$ O(g(n)) $** | 점근적 상한 (최악의 경우 수행 시간) | **이하 $(leq)$** |
| **$ Omega(g(n)) $** | 점근적 하한 (최상의 경우 수행 시간) | **이상 $(geq)$** |
| **$ Theta(g(n)) $** | 점근적 동일 (평균적 수행 시간) | **이상$(geq)$, 이하$(leq)$** |
| **$ o(g(n)) $** | 엄격한 상한 (최악의 경우 수행 시간이 $( g(n) )$보다 낮은 경우) | **미만 $(<)$** |
| **$ omega(g(n)) $** | 엄격한 하한 (최상의 경우 수행 시간이 $( g(n) )$보다 높은 경우) | **초과 $(>)$** |

---

### 5.2 빅-오(Big-O) 표기법

빅-오 표기법 $O(g(n))$은 **기껏해야 $ g(n) $의 비율로 증가하는 함수**를 의미합니다. 즉, 알고리즘의 수행 시간이 **최대 얼마나 증가할 수 있는지**를 나타냅니다.

#### **수학적 정의**
$ O(g(n)) $은 다음을 만족하는 함수들의 집합입니다.

- $[
O(g(n)) = { f(n) | exists c > 0, n_0 geq 0 text{ such that } forall n geq n_0, f(n) leq c cdot g(n) }
]$

즉, **어떤 상수 $( c )$와 특정한 $( n_0 )$ 이상에서는** $ f(n) $이 항상 $ g(n) $보다 작거나 같으면 $ f(n) in O(g(n)) $입니다. 이는 **최악의 경우에도 $ g(n) $을 초과하지 않는다는 의미(이하, $(leq)$)**입니다.

---

### 5.3 빅-오메가(Big-Omega)와 세타(Theta) 표기법

#### **빅-오메가 - $ Omega(g(n)) $**
- **$ f(n) $이 적어도 $ g(n) $ 이상의 속도로 증가하는 경우(이상, $(geq)$)**를 의미합니다.

- $[
Omega(g(n)) = { f(n) | exists c > 0, n_0 geq 0 text{ such that } forall n geq n_0, f(n) geq c cdot g(n) }
]$

#### **세타 - $ Theta(g(n)) $**
- **$( f(n) )이 ( g(n) )$과 동일한 증가 속도를 가질 때(이상, 이하 $(geq), (leq)$)**를 의미합니다.

- $[
Theta(g(n)) = O(g(n)) cap Omega(g(n))
]$

즉, **$( g(n) )$을 벗어나지 않고 정확히 동일한 증가율을 갖는 경우**를 나타냅니다.

---

### 5.4 리틀-오(little-o)와 리틀-오메가(little-omega) 표기법

#### **리틀-오 - $ o(g(n)) $**
- **$( f(n) )$이 $( g(n) )$보다 더 느리게 증가하는 경우(미만, $(<)$)**를 의미합니다.
  
#### **리틀-오메가 - $ omega(g(n)) $**
- **$( f(n) )$이 $( g(n) )$보다 더 빠르게 증가하는 경우(초과, $(>)$)**를 의미합니다.

---

### 5.5 빅-오 표기법의 예시

**다음 함수들은 모두 $ O(n) $에 속합니다.**
- $( 7n + 100 )$
- $( 3 log_2 n )$
- $( log_2 n + n )$

---

### 5.6 빅-오 표기법을 사용할 때 주의할 점

- **알 수 있는 한 최대한 꼭 맞게(Tight 하게)!**
  - $( 3n = O(n) )$이지만, 굳이 $O(n^2)$로 쓸 필요는 없습니다.
  - $( 2nlog_2 n = O(nlog_2 n) )$이지만, $O(n^2)$로 쓰는 것은 정보 손실이 있습니다.

즉, **가능한 한 정확한 표현**을 사용하는 것이 중요합니다.