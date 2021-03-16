---
layout: post
title: 📖 [백준알고리즘 풀이] Q.2003 수들의 합 2 풀이 투 포인터 방식 - java
category : Algorithm
tags: [Algorithm,boj,prefix sum, two pointer]
---
# 📖 문제
https://www.acmicpc.net/problem/2003

N개의 수로 된 수열 A[1], A[2], …, A[N] 이 있다. 이 수열의 i번째 수부터 j번째 수까지의

합 A[i] + A[i+1] + … + A[j-1] + A[j]가 M이 되는 경우의 수를 구하는 프로그램을 작성하시오.

# 🔍 접근법

Q.2003 수들의 합 2 문제입니다.

i번째 수부터 j번째 수까지의 연속 된 합이 M이 되는 경우의 수를 구하는 문제입니다.

여기서 i번째 수부터 j번째 수까지 연속 된 합을 구하는 문제이므로

누적합을 이용해서 쉽게 풀이가 가능했습니다. 

누적합을 구하는데 까지는 O(N) 시간만에 가능하지만

이 누적합을 이용해 각각의 i번째부터 j번째까지의 합을 구하는 데는 

이중 for문으로 O(N^2)의 시간이 걸리게 됩니다.

이를 좀 더 효율적으로 풀이할 수 있는 방법이 있습니다.

바로 two pointer 방식입니다.

투 포인터 방식은 이름 그대로 두개의 포인터를 가지고

시작점과 끝점으로 둬서 풀이하는 방식을 말합니다.

이 투 포인터 방식을 이용하면

o(N) 시간만에 답을 쉽게 구할 수 있습니다.

Q.2003 수들의 합 2 문제 같은 경우에는

<b> 모두 자연수로 이루어져있기 때문에! </b>

m보다 합이 작다면 end를 (끝 지점) 하나씩 늘려주며 수를 더 해주면 되고

반대로 m보다 합이 커진다면 start를 (앞 지점) 하나씩 늘려주며 앞 지점의 수를 빼주면 

쉽고 빠르게 i (start) 부터 j (end) 까지의 합이 되는 경우의 수를 

구할 수 있습니다.
               
# 💻 코드

```

package problem.twopointer;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main_2003 {
    static int n,m;
    static int numbers[];
    static int count = 0;


    public static void main(String[] args) throws IOException {
        input();
        solve();
        System.out.println(count);
    }

    public static void solve(){
        int p1,p2;
        int sum = 0;
        p1 = 0;
        p2 = 0;

        while (true){
            if(sum >= m){
                sum -= numbers[p1++];
            }
            else if(p2 == n){
                break;
            }
            else {
                sum+= numbers[p2++];
            }
            if(sum == m){
                count++;
            }
        }

    }


    public static void input() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String temp[] = br.readLine().split( " ");
        n = Integer.parseInt(temp[0]);
        m = Integer.parseInt(temp[1]);
        numbers = new int[n];
        temp = br.readLine().split(" ");
        for(int i=0; i<n; i++){
            numbers[i] = Integer.parseInt(temp[i]);
        }
    }

}


```