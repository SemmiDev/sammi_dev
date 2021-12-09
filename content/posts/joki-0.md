---
author:
name: "Sammi Aldhi Yanto"
description: Joki Coding 💸 SD Array - Universitas Gunadarma
date: 2021-09-09
linktitle: Joki Coding - SD Array - Universitas Gunadarma
type:
- post
- posts
title: Joki Coding ~ SD Array
weight: 10
series:
- Java
- Joki
tags:
- Java
- Joki
categories:
- Java
- Joki
---

## Soal
 1. Desainlah kode java untuk operasi penghapusan data.

## Penyelesaian

```java
import java.util.stream.IntStream;

public class Hapus {
    public static void main(String[] args) {
        int A[] = new int[5];
        A[0] = 9;
        A[1] = 8;
        A[2] = 7;
        A[3] = 6;
        A[4] = 5;

        System.out.println("The original array elements are: ");
        for(int i = 0; i < A.length; i++){
            System.out.println("A["+i+"] = " + A[i]);
        }

        int angkaYangInginDihapus = 7;
        int indexAngkaYangInginDiHapus = 0;
        for (int i = 0; i < A.length; i++) {
            if (A[i] == angkaYangInginDihapus) {
                indexAngkaYangInginDiHapus= i;
            }
        }

        A = cara1(A, indexAngkaYangInginDiHapus);
        System.out.println("The array elements after deletion: angka = " + angkaYangInginDihapus);
        for(int i = 0; i < A.length; i++){
            System.out.println("A["+ i +"] = " + A[i]);
        }

        // cara 2
        // (A, indexAngkaYangInginDiHapus)
    }

    public static int[] cara1(int[] arr, int index) {
        if (arr == null || index < 0 || index >= arr.length) {
            return arr;
        }
        int[] arrayLain = new int[arr.length - 1];
        for (int i = 0, k = 0; i < arr.length; i++) {
            // skip, karena data ke-index ini yang ingin kata hapus
            if (i == index) {
                continue;
            }
            arrayLain[k++] = arr[i];
        }
        return arrayLain;
    }


    public static int[] cara2(int[] arr, int index) {
        if (arr == null || index < 0 || index >= arr.length) {
            return arr;
        }
        return IntStream.range(0, arr.length)
                .filter(i -> i != index)
                .map(i -> arr[i])
                .toArray();
    }
}
```
