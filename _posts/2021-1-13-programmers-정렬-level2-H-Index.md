---
layout: post
title: 📖 [프로그래머스 알고리즘 문제 풀이] level2 정렬 H-Index 문제 풀이
category : Algorithm
tags: [Algorithm,programmers,sorting]
---
# 📖 문제
https://programmers.co.kr/learn/courses/30/lessons/42747

H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다.

위키백과1에 따르면, H-Index는 다음과 같이 구합니다.

어떤 과학자가 발표한 논문 n편 중, h번 이상 인용된 논문이 h편 이상이고 나머지 논문이 h번 이하 인용되었다면 h의 최댓값이

이 과학자의 H-Index입니다.

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록

solution 함수를 작성해주세요.


# 🔍 접근법

프로그래머스 문제입니다.

H-Index 라는 것을 구하는 문제입니다.

문제 설명을 잘 읽고 풀이하는 것이 중요합니다.

문제 그대로 n편 중, h번 이상 인용된 논문이 h편 이상이고 나머지 논문이 h번 이하 인용되었을 경우
h의 최댓값을 구하면 됩니다. 여기서 h번 이상 된 논문만을 확인하면 됩니다. 나머지 논문은 자연스럽게
h번 이하로 논문 인용이 되었기 때문에 고려할 필요가 없습니다.
논문의 인용 횟수를 정렬한 후 많이 인용 된 횟수의 논문부터 확인하면 됩니다.
위에서 말했던 것처럼 인용이 h번 이상 된 논문까지만 확인을 하여
그 중 h번 이상 된 논문이 h편 이상인 
h 값 들 중에서 최대값을 구하면 됩니다. 

              
# 💻 코드

```
import java.util.*;
class Solution {
    public int solution(int[] citations) {
        int answer = 0;
        int max = 0;
        
        Arrays.sort(citations);
        for(int h=0; h<citations[citations.length-1]; h++){
            int count =0;
            for(int j=citations.length-1; j>=0; j--){
                if(citations[j] >= h){
                    count++;
                }
                else{
                    break;
                }
            }
            if(count>=h){
                max = Math.max(max,h);
            }
        }
        return max;
    }
}

```