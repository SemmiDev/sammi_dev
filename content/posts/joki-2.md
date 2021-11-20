---
author:
  name: "Sammi Aldhi Yanto"
description: Joki Coding 💸 Struktur Data & Algoritma | UI
date: 2021-09-08
linktitle: Joki Coding - Struktur Data & Algoritma - Universitas Indonesia
type:
- post
- posts
title: Joki Coding ~ Masih Jualan
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
![image](/assets/joki/d.png)
![image](/assets/joki/e.png)
![image](/assets/joki/f.png)
![image](/assets/joki/g.png)

### Solution
```java
import java.io.IOException;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.PrintWriter;
import java.util.*;

class Lab2 {
    private static InputReader in;
    private static PrintWriter out;

    static ArrayList<String> antrians = new ArrayList<>();
    static ArrayList<String> antriansTool = new ArrayList<>();

    static int totalDatang= 0;
    static private int handleDatang(String Gi, int Xi) {
        antrians.add(Gi + "_" + Xi);
        antriansTool.add(Gi + "_" + Xi);
        totalDatang += Xi;
        return totalDatang;
    }

    static private String handleLayani(int Yi) {
        int temp = 0;
        int c = 0;
        String [] r;
        int elapsed;
        int antriansSize = antrians.size();

        for (int i = 0; i < antriansSize; i++) {
            r = antrians.get(i).split("_");
            temp += Integer.parseInt(r[1]);

            elapsed = temp - Yi;
            if (elapsed >= 0 ){
                antrians.set(i, r[0] + "_" + elapsed);
                totalDatang -= Yi;
                return r[0];
            }
            antrians.set(i, r[0] + "_" + 0);
            continue;
        }
        return "";
    }

    static private int handleTotal(String Gi) {
        int total = 0;
        String[] data;
        String[] firstData;
        int dataConvert;
        int firstDataConvert;

        int antriansSize = antrians.size();
        for (int i = 0; i < antriansSize; i++) {
            data = antrians.get(i).split("_");
            firstData = antriansTool.get(i).split("_");

            dataConvert = Integer.parseInt(data[1]);
            firstDataConvert = Integer.parseInt(firstData[1]);
            if ((data[0].equalsIgnoreCase(Gi)) && (dataConvert != firstDataConvert)) {       
                if (dataConvert == 0) {
                    total += firstDataConvert;
                }else {
                    total += firstDataConvert - dataConvert;
                }
            }
        }
        return total;
    }

    public static void main(String args[]) throws IOException {
        InputStream inputStream = System.in;
        in = new InputReader(inputStream);
        OutputStream outputStream = System.out;
        out = new PrintWriter(outputStream);

        int N;

        N = in.nextInt();

        for(int tmp=0;tmp<N;tmp++) {
            String event = in.next();

            if(event.equals("DATANG")) {
                String Gi = in.next();
                int Xi = in.nextInt();

                out.println(handleDatang(Gi, Xi));
            } else if(event.equals("LAYANI")) {
                int Yi = in.nextInt();

                out.println(handleLayani(Yi));
            } else {
                String Gi = in.next();

                out.println(handleTotal(Gi));
            }
        }

        out.flush();
    }

    // taken from https://codeforces.com/submissions/Petr
    // together with PrintWriter, these input-output (IO) is much faster than the usual Scanner(System.in) and System.out
    // please use these classes to avoid your fast algorithm gets Time Limit Exceeded caused by slow input-output (IO)
    static class InputReader {
        public BufferedReader reader;
        public StringTokenizer tokenizer;

        public InputReader(InputStream stream) {
            reader = new BufferedReader(new InputStreamReader(stream), 32768);
            tokenizer = null;
        }

        public String next() {
            while (tokenizer == null || !tokenizer.hasMoreTokens()) {
                try {
                    tokenizer = new StringTokenizer(reader.readLine());
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
            return tokenizer.nextToken();
        }

        public int nextInt() {
            return Integer.parseInt(next());
        }

    }
}
```
