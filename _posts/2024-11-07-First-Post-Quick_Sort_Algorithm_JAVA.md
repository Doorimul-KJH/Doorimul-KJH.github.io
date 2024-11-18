---
title: How to find the number of comparisons for the quick sort algorithm (Java)
author: Doorimul-KJH
date: 2024-11-07 00:00:00 +0900
categories: [Blogging]
tags: [writing]
render_with_liquid: false
---

# 퀵 정렬 알고리즘 비교 횟수 계산

이 문서는 퀵 정렬 알고리즘을 구현하여 정렬 과정에서 발생하는 비교 횟수를 계산하는 방법을 설명합니다. 코드는 Java로 작성되었으며, 주요 정렬 함수와 분할 로직이 포함되어 있으며 피벗과 원소를 비교할 때만 비교 횟수를 증가시키도록 설계되었습니다.



## Quick.java

`Quick.java` 파일에는 정렬 로직과 비교 횟수를 저장하는 카운터가 포함되어 있습니다.

```java
// Quick.java
public class Quick {
    private static int comparisonCount;  // 비교 횟수 카운터

    public static void sort(Comparable[] a) {
        comparisonCount = 0;  // 비교 횟수 초기화
        sort(a, 0, a.length - 1);
    }

    public static int getComparisonCount() {
        return comparisonCount;
    }

    private static void sort(Comparable[] a, int low, int high) {
        if (high <= low) return;
        int j = partition(a, low, high);
        sort(a, low, j - 1);  // 피벗보다 작은 부분 재귀 정렬
        sort(a, j + 1, high); // 피벗보다 큰 부분 재귀 정렬
    }

    private static int partition(Comparable[] a, int pivot, int high) {
        int i = pivot + 1;
        int j = high;
        Comparable p = a[pivot]; // 피벗은 왼쪽의 첫 요소

        while (true) {
            // 피벗과 비교할 때만 비교 횟수 증가
            while (i <= high) {
                comparisonCount++;
                if (a[i].compareTo(p) >= 0) break;
                i++;
            }
            while (j > pivot) {
                comparisonCount++;
                if (a[j].compareTo(p) <= 0) break;
                j--;
            }
            if (i >= j) break;  // i와 j가 교차하면 종료
            swap(a, i, j);      // i와 j 요소를 교환
        }
        swap(a, pivot, j);      // 피벗을 a[j] 위치로 이동
        return j;               // 피벗 위치 반환
    }

    private static void swap(Comparable[] a, int i, int j) {
        Comparable temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }
}
```

---

## main.java
'main.java' 파일은 'Quick.java'를 사용하여 배열을 정렬하고 각 정렬 작업에서 발생하는 비교 횟수를 출력합니다.

```java
// main.java
public class main {
    public static void main(String[] args) {
        String a[] = {"1", "2", "3", "4", "5", "6"}; // 최악의 경우
        String b[] = {"4", "2", "1", "3", "6", "5"}; // 최적의 경우

        System.out.println("배열 a 정렬 과정:");
        Quick.sort(a);
        System.out.print("정렬 결과: ");
        for (int i = 0; i < a.length; i++) {
            System.out.printf(a[i] + "  ");
        }
        System.out.println("\n비교 횟수: " + Quick.getComparisonCount());

        System.out.println("\n배열 b 정렬 과정:");
        Quick.sort(b);
        System.out.print("정렬 결과: ");
        for (int i = 0; i < b.length; i++) {
            System.out.printf(b[i] + "  ");
        }
        System.out.println("\n비교 횟수: " + Quick.getComparisonCount());
    }
}
```

---

## 설명
비교 횟수
최악의 경우: 배열 a = {"1", "2", "3", "4", "5", "6"}에서 퀵 정렬을 수행할 때 비교 횟수는 15번 발생합니다.
최적의 경우: 배열 b = {"4", "2", "1", "3", "6", "5"}에서 퀵 정렬을 수행할 때 비교 횟수는 8번으로 최적화된 경우입니다.
핵심 사항
피벗 선택: 배열의 가장 왼쪽 요소를 피벗으로 사용합니다.
비교 횟수 계산: 피벗과 원소를 비교할 때만 비교 횟수를 증가시킵니다.
이 방법은 입력 배열 순서에 따라 퀵 정렬의 성능을 평가할 수 있도록 각 경우의 비교 횟수를 보여줍니다.