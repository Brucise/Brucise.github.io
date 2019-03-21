---
title: QuickSort
date: 2019-03-20 15:19:36
tags: [排序]
---
# 介绍

## 特征
1. 时间复杂度是O(n*logn)
2. 不稳定

## 递归三要素
参数、终止条件、返回值
``` java
  private void help(int[] array, int left, int right) {
        if (left >= right) { // 终止条件
            return;
        }
        int n = partition(array, left, right);  // 切分
        help(array, left, n - 1);   //将左半部分排序
        help(array, n + 1, right);  //将右半部分排序
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
            int tmp = array[left];
            array[left] = array[right];
            array[right] = tmp;
        }
        return left;
    }
```