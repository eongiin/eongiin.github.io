---
layout: post
title: permutaion and combination
# subtitle: 부제목
# description: >
#   설명
date: '2022-04-17 14:0:0'
categories:
  - coding-test
  - appendix
tags: [java]
related_posts:
  - 
sitemap: true
published: true
---
# [Java] 순열과 조합

* toc 
{:toc}

## 순열
### 개수

$$ _n P _r  = \frac{n!}{(n-r)!} = n(n-1)(n-2)(n-3)\dots(n-r+1) \; (단, 0<r\leq n)$$

```java
public static void main(String[] args) {
    int n = 4;
    int r = 3;
    System.out.println(numberOfPermutation(n, r));
}

static int numberOfPermutation(int n, int r) {
    int result = 1;
    for (int i = n - r + 1; i <= n; i++) {
        result *= i;
    }
    return result;
}
```

### 케이스

#### 방법1 (swap)
```java
static void permutation(int[] arr, int cnt, int n, int r) {
    if (cnt == r) {
        for (int i = 0; i < r; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
        return;
    }

    for (int i = cnt; i < n; i++) {
        swap(arr, cnt, i);
        permutation(arr, cnt + 1, n, r);
        swap(arr, cnt, i);
    }
}

static void swap(int[] arr, int cnt, int idx) {
    int tmp = arr[cnt];
    arr[cnt] = arr[idx];
    arr[idx] = tmp;
}

public static void main(String[] args) {
    int[] arr = {1, 2, 3, 4};
    permutation(arr, 0, arr.length, 3);
}
```

#### 방법2 (visited)

```java
static void permutation(int[] arr, int cnt, int n, int r, boolean[] visited, int[] result) {
    if (cnt == r) {
        System.out.println(Arrays.toString(result));
        return;
    }

    for (int i = 0; i < n; i++) {
        if (visited[i])
            continue;
        visited[i] = true;
        result[cnt] = arr[i];
        permutation(arr, cnt + 1, n, r, visited, result);
        visited[i] = false;
    }
}

public static void main(String[] args) {
    int[] arr = {1, 2, 3, 4};
    int n = arr.length;
    int r = 3;
    boolean[] visited = new boolean[n];
    int[] result = new int[r];
    permutation(arr, 0, n, r, visited, result);
}
```

```
[1, 2, 3]
[1, 2, 4]
[1, 3, 2]
[1, 3, 4]
[1, 4, 2]
[1, 4, 3]
.
.
.
[4, 1, 2]
[4, 1, 3]
[4, 2, 1]
[4, 2, 3]
[4, 3, 1]
[4, 3, 2]
```
## 중복 순열

### 개수

$$ _n\Pi _r = n^r$$

`Math.pow(n,r)`

### 케이스

중복체크를 안 하면 된다

```java
static void permutationWithRepetition(int[] arr, int cnt, int n, int r, int[] result) {
    if (cnt == r) {
        System.out.println(Arrays.toString(result));
        return;
    }

    for (int i = 0; i < n; i++) {
        result[cnt] = arr[i];
        permutationWithRepetition(arr, cnt + 1, n, r, result);
    }
}

public static void main(String[] args) {
    int[] arr = {1, 2, 3, 4};
    int n = arr.length;
    int r = 3;
    permutationWithRepetition(arr, 0, n, r, new int[r]);
}
```
```
[1, 1, 1]
[1, 1, 2]
[1, 1, 3]
[1, 1, 4]
[1, 2, 1]
[1, 2, 2]
[1, 2, 3]
[1, 2, 4]
[1, 3, 1]
[1, 3, 2]
[1, 3, 3]
[1, 3, 4]
[1, 4, 1]
[1, 4, 2]
[1, 4, 3]
[1, 4, 4]
.
.
.
[4, 1, 1]
[4, 1, 2]
[4, 1, 3]
[4, 1, 4]
[4, 2, 1]
[4, 2, 2]
[4, 2, 3]
[4, 2, 4]
[4, 3, 1]
[4, 3, 2]
[4, 3, 3]
[4, 3, 4]
[4, 4, 1]
[4, 4, 2]
[4, 4, 3]
[4, 4, 4]
```

## 조합

### 개수

$$ _n C _r  = \frac{n!}{(n-r)!r!} = \frac{_nP_r}{r!} \; (단, 0<r\leq n)$$

#### 방법 1

$$ _n C _r  = \frac{n!}{(n-r)!r!} = \frac{n(n-1)(n-2)\dots(n-r+1)}{r!} \; (단, 0<r\leq n)$$

```java
public static void main(String[] args) {
    int n = 4;
    int r = 3;
    System.out.println(numberOfCombination(n, r));
}

static int numberOfCombination(int n, int r) {
    int result = 1;
    for (int i = 1; i <= r; i++) {
        result *= (n - i + 1) / (double) i;
    }
    return result;
}
```

#### 방법 2 (memoization)

$$ \begin{align} \\
&_n C _r  = _{n-1}C_{r-1} + _{n-1}C_r \;\; (단, 0<r\leq n) \\
&_nC_0 = _nC_n = 1 \\
\end{align} $$

- $$ _{n-1}C_{r-1} $$ : 어떤 특정한 원소를 포함 시키고 뽑았을 때
- $$ _{n-1}C_{r} $$ : 어떤 특정한 원소를 포함시키지 않고 뽑았을 때

```java
static int[][] memo;

public static void main(String[] args) {
    int n = 4;
    int r = 3;
    memo = new int[n + 1][r + 1];
    System.out.println(numberOfCombination(n, r));
}

static int numberOfCombination(int n, int r) {
    if (memo[n][r] > 0) {
        return memo[n][r];
    }
    if (n == r || r == 0) {
        return 1;
    }
    memo[n][r] = numberOfCombination(n - 1, r - 1) + numberOfCombination(n - 1, r);
    return memo[n][r];
}
```

### 케이스

```java
static void combination(int[] arr, int[] result, int cnt, int idx, int n, int r) {
    if (cnt == r) {
        System.out.println(Arrays.toString(result));
        return;
    }

    for (int i = idx; i < n; i++) {
        result[cnt] = arr[i];
        combination(arr, result, cnt + 1, i + 1, n, r);
    }
}

public static void main(String[] args) {
    int[] arr = {1, 2, 3, 4};
    int n = arr.length;
    int r = 3;
    combination(arr, new int[r], 0, 0, n, r);
}
```

```
[1, 2, 3]
[1, 2, 4]
[1, 3, 4]
[2, 3, 4]
```

## 중복 조합

### 개수

$$ _n H _r  = _{n+r-1}C _r = \frac{(n+r-1)!}{(n+r-1-r)!r!} = \frac{(n+r-1)(n+r-2)\dots n}{r!} $$

조합 수 구하던 방식에서 `n` 대신 `n+r-1`을 넣으면 된다

### 케이스

idx를 넘길 때 `i+1`이 아닌 `i`를 넘기면 된다

```java
static void combinationWithRepetition(int[] arr, int[] result, int cnt, int idx, int n, int r) {
    if (cnt == r) {
        System.out.println(Arrays.toString(result));
        return;
    }

    for (int i = idx; i < n; i++) {
        result[cnt] = arr[i];
        combinationWithRepetition(arr, result, cnt + 1, i, n, r);
    }
}

public static void main(String[] args) {
    int[] arr = {1, 2, 3, 4};
    int n = arr.length;
    int r = 3;
    combinationWithRepetition(arr, new int[r], 0, 0, n, r);
}
```

```
[1, 1, 1]
[1, 1, 2]
[1, 1, 3]
[1, 1, 4]
[1, 2, 2]
[1, 2, 3]
[1, 2, 4]
[1, 3, 3]
[1, 3, 4]
[1, 4, 4]
[2, 2, 2]
[2, 2, 3]
[2, 2, 4]
[2, 3, 3]
[2, 3, 4]
[2, 4, 4]
[3, 3, 3]
[3, 3, 4]
[3, 4, 4]
[4, 4, 4]
```