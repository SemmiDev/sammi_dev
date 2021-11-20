---
author:
  name: "Sammi Aldhi Yanto"
description: Semester 3 📜 Quiz 1 - Struktur Data Array
date: 2021-09-08
linktitle: Quiz 1 - Struktur Data Array 
type:
- post
- posts
title: ASD ~ Quiz 1 Struktur Data Array
weight: 10
series:
- Java
- ASD
tags:
- Java
categories:
- Java
- Assignments
---

## Soal
 1. Desainlah kode java untuk operasi penghapusan data.
 2. Kode yang di submit dalam format txt.

## Penyelesaian

> Quiz-1.txt

```java
import java.util.stream.IntStream;

public class Hapus {
    public static void main(String[] args) {

        /**
         * Nama: Sammi Aldhi Yanto
         * NIM: xxxxxxxxxx
         * Prodi-Kelas: SI-A
         * Matkul: ASD
         * Quiz: 1 (pertama)
         * Studi kasus struktur data array statis: penghapusan data
         */

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

        // Misal, data/angka yang ingin kita hapus = 7
        // Sammi mempunyai dua ide pak
        // 1. Menggunakan array lain sbg alat bantu
        // 2. Menggunakan Fitur java 8

        int angkaYangInginDihapus = 7;
        int indexAngkaYangInginDiHapus = 0;
        for (int i = 0; i < A.length; i++) {
            if (A[i] == angkaYangInginDihapus) {
                indexAngkaYangInginDiHapus= i;
            }
        }

        // cara pertama
        A = cara1(A, indexAngkaYangInginDiHapus);
        System.out.println("The array elements after deletion: angka = " + angkaYangInginDihapus);
        for(int i = 0; i < A.length; i++){
            System.out.println("A["+ i +"] = " + A[i]);
        }

        // cara2(A, indexAngkaYangInginDiHapus)
    }

    public static int[] cara1(int[] arr, int index) {
        // check constraint/batasan
        // jika array nya kosong
        // atau tidak dibawah 0
        // atau index besar sama dengan panjang array
        if (arr == null || index < 0 || index >= arr.length) {
            return arr;
        }

        // membuat arrayLain, yang kapasitas nya kurang - 1 dari panjang array yang di input kan
        int[] arrayLain = new int[arr.length - 1];

        // copy setiap angka di array asli ke arrayLain, kecuali index yang sudah ditentukan
        for (int i = 0, k = 0; i < arr.length; i++) {
            // skip, karena data ke-index ini yang ingin kata hapus
            if (i == index) {
                continue;
            }

            // tambahkan data ke arrayLain
            arrayLain[k++] = arr[i];
        }
        return arrayLain;
    }


    public static int[] cara2(int[] arr, int index) {
        // check constraint/batasan
        // jika array nya kosong
        // atau tidak dibawah 0
        // atau index besar sama dengan panjang array
        if (arr == null || index < 0 || index >= arr.length) {
            return arr;
        }

        // fitur java 8 (stream)
        return IntStream.range(0, arr.length)
                .filter(i -> i != index)
                .map(i -> arr[i])
                .toArray();
    }
}
```
