---
title: QuickSort
date: 2019-03-20 15:19:36
tags:[排序]
---
# 快速排序
1. 时间复杂度是O(n*logn)
2. 不稳定

``` java
  private void help(int[] array, int left, int right) {
        if (left >= right) {
            return;
        }
        int n = partition(array, left, right);
        help(array, left, n - 1);
        help(array, n + 1, right);
    }

    private int partition(int[] array, int left, int right) {
        int v = array[left];
        while (true) {
            while (v > array[left]) {
                left++;
            }
            while (v < array[right]) {
                right--;
            }
            if (left >= right) {
                break;
            }
            pairs++;
            int tmp = array[left];
            array[left] = array[right];
            array[right] = tmp;
        }
        return left;
    }
```