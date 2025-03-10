---
title: Algorithm - Implementing Recursive Binary Search in Java
author: Doorimul-KJH
date: 2025-03-10 00:00:00 +0900
categories: [Algorithm, blogging]
tags: [blogging, Algorithm]
pin: true
render_with_liquid: false
---

# Java 프로그램 과제: 재귀적 이진 검색 (Binary Search)

## 문제 설명
오름차순으로 정렬된 정수 배열에서 사용자가 입력한 값을 이진검색으로 찾아 해당 값의 위치(인덱스)를 출력합니다. 이진검색은 재귀 알고리즘으로 작성해야 합니다.

### 요구사항
- 이진검색(Binary Search)을 재귀로 구현할 것
  - 매개변수: 정수 배열(`array`), 검색할 배열 범위(`from`, `to`), 검색할 정수값(`item`)
  - 반환값: 검색 성공 시 인덱스 반환, 검색 실패 시 `-1` 반환
- 배열에 동일한 원소가 여러 개 있는 경우는 아무 인덱스나 반환해도 됩니다.

### 주어진 배열
```java
int[] array = {20, 40, 60, 80, 100, 110, 120, 130, 150, 190, 200, 300, 400};
```

---

## 실행 예시

### 입력 예시 1
```
200
```

### 출력 예시 1
```
10
```

### 입력 예시 2
```
20
```

### 출력 예시 2
```
0
```

### 입력 예시 3 (찾는 값이 배열에 없는 경우)
```
99
```

### 출력 예시 2
```
-1
```

---

## 알고리즘 풀이 과정

이진검색은 **정렬된 배열**에서 특정 값을 빠르게 찾을 수 있는 알고리즘입니다. 이진검색을 재귀적으로 구현하면, 배열의 중간값을 기준으로 값을 찾아 범위를 좁혀가며 탐색합니다.

이진검색의 일반적인 과정은 다음과 같습니다:

```
1. 배열의 가운데 인덱스(mid)를 구한다.
2. 찾으려는 값(item)과 중간값을 비교한다.
   - 찾았다면 인덱스를 반환한다.
   - 중간값보다 작으면 왼쪽 부분에서 재귀적으로 탐색한다.
   - 중간값보다 크면 오른쪽 부분에서 재귀적으로 탐색한다.
3. 탐색할 범위가 없으면(-1로 반환) 종료한다.
```

---

## Java 코드 구현 및 설명

### 재귀적 이진 검색 (if-else 버전)

```java
import java.util.Scanner;

public class RecursiveBinarySearch {
    public static void main(String[] args) {
        int[] array = {20, 40, 60, 80, 100, 110, 120, 130, 150, 190, 200, 300, 400};
        Scanner scanner = new Scanner(System.in);

        int item = scanner.nextInt();
        int index = binarySearch(array, 0, array.length - 1, item);
        System.out.println(index);

        scanner.close();
    }

    private static int binarySearch(int[] array, int from, int to, int item) {
        if (from > to) {
            return -1; // 검색 실패
        }

        int mid = (from + to) / 2;

        if (array[mid] == item) {
            return mid; // 값을 찾은 경우
        } else if (array[mid] > item) {
            return binarySearch(array, from, mid - 1, item); // 왼쪽 부분 탐색
        } else {
            return binarySearch(array, mid + 1, to, item); // 오른쪽 부분 탐색
        }
    }
}
```

---

## 삼항 연산자 vs. If-Else 사용 비교

위의 코드를 삼항 연산자로도 작성할 수 있지만, 가독성의 측면에서는 if-else 문이 더 유리합니다. 비교를 위해 아래에 두 가지 방식을 나타냈습니다.

### If-Else 문을 이용한 코드

```java
private static int binarySearch(int[] array, int from, int to, int item) {
    if (from > to) {
        return -1; // 탐색 실패
    }
    int mid = (from + to) / 2;

    if (array[mid] == item) {
        return mid;
    } else if (array[mid] > item) {
        return binarySearch(array, from, mid - 1, item);
    } else {
        return binarySearch(array, mid + 1, to, item);
    }
}
```

### 삼항 연산자를 이용한 코드

```java
private static int binarySearch(int[] array, int from, int to, int item) {
    if (from > to) {
        return -1; // 탐색 실패
    }

    int mid = (from + to) / 2;

    return (array[mid] == item)
            ? mid
            : (array[mid] > item
                ? binarySearch(array, from, mid - 1, item)
                : binarySearch(array, mid + 1, to, item));
}
```

---

## If-Else 문과 삼항 연산자의 비교

| 구분               | If-Else 문                   | 삼항 연산자                  |
|--------------------|------------------------------|-----------------------------|
| 가독성             | ✅ 명확하고 이해하기 쉬움        | ⚠️ 조건이 복잡하면 가독성 저하 |
| 코드 길이          | ⚠️ 코드 길이가 다소 길어질 수 있음 | ✅ 코드가 간결해짐            |
| 유지보수성          | ✅ 조건이 많아도 관리 용이함     | ⚠️ 복잡할 경우 유지보수 어려움 |

**결론**: 이진검색처럼 복잡한 로직에서는 **가독성과 유지보수**를 고려하여 **If-Else 문**을 사용하는 것이 바람직합니다.

---

## 삼항 연산자를 굳이 사용하지 않아도 되는 이유
삼항 연산자는 코드 길이를 줄여주지만, 조건이 복잡한 경우 **가독성을 떨어뜨리고**, 디버깅이나 유지보수가 어려워질 수 있습니다. 따라서 이진검색처럼 복잡한 알고리즘에서는 굳이 삼항 연산자를 사용할 필요가 없습니다.

---

## 삼항 연산자를 사용하는 것이 적합한 경우 (예시)

삼항 연산자는 간단한 조건의 값 할당에서 효과적입니다.

**예시**:
```java
int max = (a > b) ? a : b;
```

이러한 **단순한 조건 처리**에서는 삼항 연산자를 사용하면 간결하게 작성할 수 있습니다.

---

## 주어진 배열의 인덱스가 홀수인 경우

이진 검색 알고리즘에서는 탐색 범위의 중간 인덱스를 구할 때 다음과 같은 수식을 사용합니다.

```java
int mid = (from + to) / 2;
```

여기서 `(from + to)`가 **홀수**인 경우, 결과 값이 소수점 이하로 떨어지지 않고 정수로 자동으로 내림 처리됩니다.

예를 들어 배열의 탐색 범위가 인덱스 0부터 6까지라면:

```java
mid = (0 + 6) / 2 = 3 // 정확히 중간값으로 탐색됨
```

하지만 탐색 범위가 인덱스 0부터 5까지의 **홀수 개수**인 경우라면:

```java
mid = (0 + 5) / 2 = 2 // 중간값이 2.5가 아닌 내림 처리된 2가 됨
```

즉, 배열의 개수가 짝수 또는 홀수이든 상관없이, `(from + to)`를 나눈 결과는 항상 정수로 내림 처리되어 정확한 중간 또는 그 중간보다 한 칸 작은 위치를 기준으로 이진 탐색이 수행됩니다.

이러한 내림 처리로 인해, 중간값을 기준으로 탐색 범위를 왼쪽과 오른쪽으로 나눌 때 왼쪽 부분의 길이가 오른쪽 부분보다 작거나 같아질 수 있습니다. 이 경우에도 탐색의 효율성이나 결과에는 영향을 주지 않으며, 이진 검색의 성능과 정확도에는 문제가 없습니다.

---

## 결론 및 생각

이번 문제를 풀이하면서 이진 검색 알고리즘을 두 가지 방법으로 구현하여 각각의 장단점을 명확하게 이해할 수 있어야합니다.

- **이진 검색처럼 복잡한 로직**에서는 **If-Else 문**이 더 적합합니다. 삼항 연산자는 조건이 복잡해질 경우 가독성을 저하시켜 디버깅이나 유지보수를 어렵게 만들기 때문입니다.
- 반면, **단순한 조건에서 값만 빠르게 결정할 때**는 코드가 간결해지는 **삼항 연산자**가 효과적입니다.
- 추가적으로, 이진 검색 과정에서 배열의 중간값을 계산할 때, 탐색 범위의 크기가 홀수일 경우 정확한 중간 인덱스가 존재하지 않아 내림 처리가 됩니다. 하지만 이 과정은 탐색 효율이나 정확도에는 영향을 주지 않으며 문제될 부분은 없습니다.

즉, **조건의 복잡성**과 **코드의 가독성**을 잘 고려하여 두 방법을 적절히 선택하는 것이 중요하다는 것이 결론입니다.
