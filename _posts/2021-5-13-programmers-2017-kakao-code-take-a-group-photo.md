---
layout: post
title: "📖 [프로그래머스 알고리즘 문제 풀이] 단체사진 찍기 문제 풀이 (2017 카카오 코드)"
category : Algorithm
tags: [Algorithm,programmers,brute force, backtracking, permutation, kakao, map]
---
# 📖 문제
단체사진 찍기 (2017 카카오 코드)
<https://programmers.co.kr/learn/courses/30/lessons/1835>


# 🔍 접근법

단체사진 찍기 문제입니다.

8명의 카카오프렌즈가 단체사진을 찍기 위해 일렬로 서야 합니다. 일렬로 세우기 위한 경우의 수를 구하는 문제입니다.

처음에는 경우의 수를 구하는 문제이기에 수학적으로 접근했으나 실패하였고

총 8명 밖에 되지 않기 때문에 brute force로 접근했습니다.

순열을 이용해 8명의 인원으로 세울 수 있는 모든 경우의 수를 만들고

단, 사진을 찍기 위해 일렬로 세우기 위한 조건들에 모두 부합해야 하므로

일렬로 세우는 각 경우의 수 마다 일렬로 세우기 위한 조건들에 모두 부합하는지 확인해주었습니다.

조건들을 확인할 때 자기 자신과 비교 대상의 위치를 확인해야 하는데

이 위치를 바로 확인할 수 있도록 

각 경우의 수마다 줄을 세울 때 Map 을 이용해서

각 프렌즈의 위치를 저장해주었습니다.

그래서 조건들을 확인할 때 

비교 대상 두명에 대해서 바로 Map(lineUpInfo) 에서 위치를 꺼내 확인할 수 있도록 하였습니다.

이 조건에 일치할 때마다 count를 1씩 늘려주면 답을 구할 수 있습니다.
              
# 💻 코드

```
package problem.programmers;

import java.util.HashMap;
import java.util.Map;

class Solution_단체사진찍기 {
    private static final int FRIENDS_COUNT = 8;
    static char[] sequence;
    static boolean[] visited;
    static char[] friends;
    static int count;

    public static int solution(int n, String[] data) {
        init();
        permutation(0, data);
        return count;
    }

    private static void init() {
        sequence = new char[FRIENDS_COUNT];
        visited = new boolean[FRIENDS_COUNT];
        friends = new char[]{'A', 'C', 'F', 'J', 'M', 'N', 'R', 'T'};
        count = 0;
    }

    private static void permutation(int depth, String[] data) {
        if (depth == FRIENDS_COUNT) {
            Map<Character, Integer> lineUpInfo = lineUp();
            if (isAbleLineUp(lineUpInfo, data)) {
                count++;
            }
            return;
        }
        for (int i = 0; i < FRIENDS_COUNT; i++) {
            if (!visited[i]) {
                visited[i] = true;
                sequence[depth] = friends[i];
                permutation(depth + 1, data);
                visited[i] = false;
            }
        }
    }

    private static HashMap<Character, Integer> lineUp() {
        HashMap<Character, Integer> map = new HashMap<>();
        for (int i = 0; i < FRIENDS_COUNT; i++) {
            map.put(sequence[i], i);
        }
        return map;
    }

    private static boolean isAbleLineUp(Map<Character, Integer> lineUpInfo, String[] conditions) {
        for (int i = 0; i < conditions.length; i++) {
            if (!isCorrectToCondition(conditions[i], lineUpInfo)) {
                return false;
            }
        }
        return true;
    }

    private static boolean isCorrectToCondition(String condition, Map<Character, Integer> map) {
        char origin = condition.charAt(0);      //origin 
        char target = condition.charAt(2);      //orign과 비교대상
        char sign = condition.charAt(3);        //조건 부호
        int num = condition.charAt(4) - '0';    //조건에 해당되는 거리

        if (sign == '=') {
            int distance = Math.abs(map.get(origin) - map.get(target)) - 1;
            if (distance == num) {
                return true;
            }
        }
        if (sign == '<') {
            int distance = Math.abs(map.get(origin) - map.get(target)) - 1;
            if (distance < num) {
                return true;
            }
        }
        if (sign == '>') {
            int distance = Math.abs(map.get(origin) - map.get(target)) - 1;
            if (distance > num) {
                return true;
            }
        }
        return false;
    }
}

```