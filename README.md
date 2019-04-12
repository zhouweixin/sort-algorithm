# 常用排序算法的java实现

## 1.冒泡排序
```java
public void bubbleSort(int[] a) {
    for (int i = 0; i < a.length; i++) {
        for (int j = 0; j < a.length - 1 - i; j++) {
            if (a[j] > a[j + 1]) {
                int t = a[j];
                a[j] = a[j + 1];
                a[j + 1] = t;
            }
        }
    }
}
```

## 2.选择排序
```java
public void selectionSort(int[] a) {
    for (int i = 0; i < a.length; i++) {
        int minIdx = i;
        for (int j = i + 1; j < a.length; j++) {
            if (a[j] < a[minIdx]) {
                minIdx = j;
            }
        }
        int t = a[i];
        a[i] = a[minIdx];
        a[minIdx] = t;
    }
}
```
## 3.插入排序
```java
public void insertionSort(int[] a) {
    for (int i = 1; i < a.length; i++) {
        int t = a[i];
        int j = i - 1;
        for (; j >= 0 && t < a[j]; j--) {
            a[j + 1] = a[j];
        }
        a[j + 1] = t;
    }
}
```

## 4.希尔排序
```java
public void shellSort(int[] a) {
    for (int gap = a.length / 2; gap > 0; gap /= 2) {
        for (int i = gap; i < a.length; i++) {
            int t = a[i];
            int j = i - gap;
            for (; j >= 0 && t < a[j]; j -= gap) {
                a[j + gap] = a[j];
            }
            a[j + gap] = t;
        }
    }
}
```

## 5.1.归并排序(递归)
```java
public void mergeSort(int[] a, int l, int r) {
    if (l < r) {
        int mid = (l + r) / 2;
        mergeSort(a, l, mid);
        mergeSort(a, mid + 1, r);
        merge(a, l, mid, r);
    }
}

public void merge(int[] a, int l, int mid, int r) {
    int[] temp = new int[a.length];
    int k = l;
    int i = l;
    int j = mid + 1;
    while (i <= mid && j <= r) {
        if (a[i] < a[j]) {
            temp[k++] = a[i++];
        } else {
            temp[k++] = a[j++];
        }
    }

    while (i <= mid) {
        temp[k++] = a[i++];
    }

    while (j <= r) {
        temp[k++] = a[j++];
    }

    for (i = l; i <= r; i++) {
        a[i] = temp[i];
    }
}
```

## 5.2.归并排序(非递归)
```java
public void mergeSort(int[] a) {
    for (int step = 1; step < a.length; step *= 2) {
        for (int i = 0; i + step < a.length; i += 2 * step) {
            merge(a, i, i + step - 1, Math.min(i + 2 * step - 1, a.length - 1));
        }
    }
}

public void merge(int[] a, int l, int mid, int r) {
    int[] temp = new int[a.length];
    int k = l;
    int i = l;
    int j = mid + 1;
    while (i <= mid && j <= r) {
        if (a[i] < a[j]) {
            temp[k++] = a[i++];
        } else {
            temp[k++] = a[j++];
        }
    }

    while (i <= mid) {
        temp[k++] = a[i++];
    }

    while (j <= r) {
        temp[k++] = a[j++];
    }

    for (i = l; i <= r; i++) {
        a[i] = temp[i];
    }
}
```

## 6.快速排序
```java
public void quickSort(int[] a, int l, int r) {
    if (l < r) {
        int m = partition(a, l, r);
        quickSort(a, l, m - 1);
        quickSort(a, m + 1, r);
    }
}

public int partition(int[] a, int l, int r) {
    int t = a[l];
    while (l < r) {
        while (a[r] >= t && l < r) r--;
        a[l] = a[r];
        while (a[l] <= t && l < r) l++;
        a[r] = a[l];
    }
    a[l] = t;
    return l;
}
```

## 7.堆排序
```java
public void heapSort(int[] a) {
    buildMaxHeap(a);
    for (int i = a.length - 1; i > 0; i--) {
        int temp = a[i];
        a[i] = a[0];
        a[0] = temp;
        adjustDown(a, 1, i);
    }
}

public void buildMaxHeap(int[] a) {
    for (int i = a.length / 2; i > 0; i--) {
        adjustDown(a, i, a.length);
    }
}

private void adjustDown(int[] a, int k, int len) {
    int temp = a[k - 1];
    for (int i = 2 * k; i <= len; i *= 2) {
        if (i < len && a[i - 1] < a[i]) {
            i++;
        }

        if (temp >= a[i - 1]) {
            break;
        } else {
            a[k - 1] = a[i - 1];
            k = i;
        }
    }
    a[k - 1] = temp;
}
```

## 8.计数排序
```java
public void countingSort(int[] a) {
    // 1、找最大值
    int max = 0;
    for (int i = 0; i < a.length; i++) {
        max = max < a[i] ? a[i] : max;
    }

    // 2、统计数量
    int[] c = new int[max + 1];
    for (int i = 0; i < a.length; i++) {
        c[a[i]]++;
    }

    // 3、依次存回
    int idx = 0;
    for (int i = 0; i <= max; i++) {
        while (c[i] > 0) {
            a[idx++] = i;
            c[i]--;
        }
    }
}
```


## 0.测试函数
```java
public void test1() {

    int len = 20;
    int[] a = new int[len], b;
    for (int i = 0; i < len; i++) {
        int random = (int) (Math.random() * 100);
        a[i] = random % 100;
    }

    System.out.print("原数组：\t\t\t\t");
    print(a);

    b = a.clone();
    bubbleSort(b);
    System.out.print("1、冒泡排序：\t\t\t");
    print(b);

    b = a.clone();
    selectionSort(b);
    System.out.print("2、选择排序：\t\t\t");
    print(b);

    b = a.clone();
    insertionSort(b);
    System.out.print("3、插入排序：\t\t\t");
    print(b);

    b = a.clone();
    shellSort(b);
    System.out.print("4、希尔排序：\t\t\t");
    print(b);

    b = a.clone();
    mergeSort(b, 0, b.length - 1);
    System.out.print("5.1、归并排序(递归)：\t");
    print(b);

    b = a.clone();
    mergeSort(b);
    System.out.print("5.2、归并排序(非递归)：\t");
    print(b);

    b = a.clone();
    quickSort(b, 0, b.length - 1);
    System.out.print("6、快速排序：\t\t\t");
    print(b);

    b = a.clone();
    heapSort(b);
    System.out.print("7、堆排序：\t\t\t\t");
    print(b);

    b = a.clone();
    countingSort(b);
    System.out.print("8、计数排序：\t\t\t");
    print(b);
}

/**
 * 输出
 *
 * @param a
 */
public void print(int[] a) {
    for (int i = 0; i < a.length - 1; i++) {
        System.out.print(a[i] + " ");
    }
    System.out.println(a[a.length - 1]);
}
```

测试结果

```
原数组：				56 87 6 95 89 1 38 6 31 17 60 97 33 32 99 20 84 90 91 30
1、冒泡排序：			1 6 6 17 20 30 31 32 33 38 56 60 84 87 89 90 91 95 97 99
2、选择排序：			1 6 6 17 20 30 31 32 33 38 56 60 84 87 89 90 91 95 97 99
3、插入排序：			1 6 6 17 20 30 31 32 33 38 56 60 84 87 89 90 91 95 97 99
4、希尔排序：			1 6 6 17 20 30 31 32 33 38 56 60 84 87 89 90 91 95 97 99
5.1、归并排序(递归)：	1 6 6 17 20 30 31 32 33 38 56 60 84 87 89 90 91 95 97 99
5.2、归并排序(非递归)：	1 6 6 17 20 30 31 32 33 38 56 60 84 87 89 90 91 95 97 99
6、快速排序：			1 6 6 17 20 30 31 32 33 38 56 60 84 87 89 90 91 95 97 99
7、堆排序：				1 6 6 17 20 30 31 32 33 38 56 60 84 87 89 90 91 95 97 99
8、计数排序：			1 6 6 17 20 30 31 32 33 38 56 60 84 87 89 90 91 95 97 99
```