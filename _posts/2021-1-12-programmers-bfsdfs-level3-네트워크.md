---
layout: post
title: 📖 [프로그래머스 알고리즘 문제 풀이] level3 dfs/bfs 네트워크 문제 풀이
category : Algorithm
tags: [Algorithm,programmers,bfs ,dfs]
---
# 📖 문제
https://programmers.co.kr/learn/courses/30/lessons/43162

네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다.

예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가

직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다.

따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때,

네트워크의 개수를 return 하도록 solution 함수를 작성하시오.


# 🔍 접근법

오늘은 백준알고리즘이 아닌 프로그래머스 문제입니다.

그래프 문제의 유형입니다.

입력이 이미 인접행렬로 주어지지만

인접리스트로 만들어서 

bfs / dfs 탐색을 통해 풀어보았습니다.

bfs / dfs 탐색 모두 구현해보았고

bfs 이던 dfs 이던 둘중 하나를 이용해 그래프의 개수 (네트워크의 개수)를

구할 수 있습니다.
              
# 💻 코드

```
import java.util.*;
class Solution {
    static boolean visited[];
    static List<List<Integer>> adjList;
    static Queue<Integer> queue;
    static int count = 0;
    public int solution(int n, int[][] computers) {
        int answer = 0;
        visited = new boolean[n];
        adjList = new ArrayList<>();
        for(int i=0; i<n; i++){
            adjList.add(new ArrayList<Integer>());
        }
        for(int i=0; i<n; i++){
            for(int j=0; j<n; j++){
                if(i==j){
                    continue;
                }
                if( computers[i][j]==1){
                    adjList.get(i).add(j);
                }
            }
        }
        for(int i=0; i<n; i++){
            if(visited[i] == false){
                dfs(i);
                count++;
            }
        }
        return count;
    }
    public void bfs(int x){
        queue = new LinkedList<>();
        queue.offer(x);
        visited[x] = true;
        
        while(!queue.isEmpty()){
            int a = queue.poll();
            List<Integer> list = adjList.get(a);
            for(int i=0; i<list.size(); i++){
                int v = list.get(i);
                if(visited[v]==false){
                    queue.offer(v);
                    visited[v] = true;
                }
            }
        }
    }
    
    public void dfs(int x){
        if(visited[x]) {return;}
        visited[x] = true;
        List<Integer> list = adjList.get(x);
        for(int i=0; i<list.size(); i++){
            int v = list.get(i);
            if(visited[v] == false){
                dfs(v);
            }
        }
    }
}

```