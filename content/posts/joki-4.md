---
author:
  name: "Sammi Aldhi Yanto"
description: Joki Coding 💸 Matrix - Universitas Gunadarma
date: 2021-09-09
linktitle: Joki Coding - Matrix - Universitas Gunadarma
type:
- post
- posts
title: Joki Coding ~ Matrix
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

## Problem
**Bikin operasi perkalian matrix & matrix transpose**

## Solution
```Java
package ddp2.lab06;

import java.util.Scanner;

public class MatrixOperations {
    public static void main(String[] args) {

        // ORDO MATRIX
        int n = 0;

        // ASSIGN ORDO MATRIX DENGAN ARGUMENT YANG DI PASS USER INDEX KE 0
        n = Integer.parseInt(args[0]);

        // UKURAN MATRIX / ORDO HARUS DIANTARA 2 DAN 10
        if (!(n >= 2 && n <= 10)) {
            System.out.println("The size of the square matrix must be between 2 and 10");
            System.exit(1);
        }

        // SCANNER UNTUK AMBIL INPUT USER UNTUK MENGISI NILAI MATRIX
        Scanner input = new Scanner(System.in);

        // ENTER MATRIX 1
        System.out.printf("Enter matrix1 of size %d X %d: \n", n, n);
        int[][] matrix1 = new int[n][n];

        // LOOP [BARIS][KOLOM] SECARA SEQUENTIAL
        for (int i = 0; i < matrix1.length; i++) {
            for (int j = 0; j < matrix1[i].length; j++) {
                matrix1[i][j] = input.nextInt();
            }
        }

        // ENTER MATRIX 2
        System.out.printf("Enter matrix2 of size %d X %d: \n", n, n);
        int[][] matrix2 = new int[n][n];


        // LOOP [BARIS][KOLOM] SECARA SEQUENTIAL
        for (int i = 0; i < matrix2.length; i++) {
            for (int j = 0; j < matrix2[i].length; j++) {
                matrix2[i][j] = input.nextInt();
            }
        }

        // MULTIPLY TWO MATRIX AND PRINT THE RESULT
        int[][] resultMatrix = multiplyMatrix(matrix1, matrix2);
        System.out.println("\nThe multiplication result of the matrices is ");
        printResult(resultMatrix);

        // FIND THE TRANSPOSE OF MATRIX1 AND PRINT THE RESULT
        System.out.println("\nThe transpose of the matrix1 is ");
        printResult(transpose(matrix1));
    }

    public static int[][] multiplyMatrix(int[][] m1, int[][] m2) {
        // RESULT ARRAY
        int[][] result = new int[m1.length][m2[0].length];

        for (int i = 0; i < m1.length; i++) {
            for (int j = 0; j < result.length; j++) {
                // SET result[i][j] = 0 TERLEBIH DAHULU
                result[i][j] = 0;
                for (int k = 0; k < result.length; k++) {
                    //        NGE LOOP BARIS & NGE LOOP KOLOM
                    result[i][j] += m1[i][k] * m2[k][j];
                }
            }
        }
        return result;
    }

    public static int[][] transpose(int[][] m) {
        // RESULT ARRAY
        int [][] result = new int[m.length][m[0].length];

        for (int i = 0; i < m.length; i++) {
            for (int j = 0; j < m[0].length; j++) {
                // BARIS -> KOLOM & KOLOM -> BARIS
                result[i][j] = m[j][i];
            }
        }
        return result;
    }

    public static void printResult(int[][] m) {
        // PRINT [BARIS][KOLOM]
        for (int i = 0; i < m.length; i++) {
            for (int j = 0; j < m[0].length; j++)
            System.out.printf("%4d ", m[i][j]);
            System.out.println();
        }
    }
}
```
