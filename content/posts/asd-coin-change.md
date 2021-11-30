+++
author = "Sammi"
title = "Coins Change with Greedy Algorithm"
date = "2021-11-30"
description = "Coins Change with Greedy Algorithm Using Go"
+++

> Author: **Sammi Aldhi Yanto**\
> Date: **Senin,30 November 2021**

# Source Code
![code](/assets/asd/4.png)
```Go
package main

import "fmt"

// Nama  : Sammi Aldhi Yanto
// NIM   : 2003113948
// Kelas : ASD A

func main() {
    coins := []int{1, 2, 5}
    n := 12

    fmt.Println("--------------------------------------------------")
    fmt.Printf("n: %d\n", n)
    fmt.Printf("coins: %v\n\n", coins)

    total := coinChangeGreedy(coins, n)
    fmt.Printf("uang $%d dapat ditukar menjadi %d coin\n", n, total)
    fmt.Println("--------------------------------------------------")

    coins = []int{1, 4, 5}
    fmt.Printf("n: %d\n", n)
    fmt.Printf("coins: %v\n\n", coins)

    total = coinChangeGreedy(coins, n)
    fmt.Printf("uang $%d dapat ditukar menjadi %d coin\n", n, total)
    fmt.Println("--------------------------------------------------")
}

func coinChangeGreedy(coins []int, n int) int {
    total := 0
    for n != 0 {
        for i := len(coins) - 1; i >= 0; i-- {
            if coins[i] <= n {
                n = n - coins[i]
                fmt.Printf("Pilih koin %d tersisa %d \n", coins[i], n)
                i++
                total++
            }
        }
    }
    return total
}

```
# Output
![output](/assets/asd/5.png)
