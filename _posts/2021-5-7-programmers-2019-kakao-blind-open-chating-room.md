---
layout: post
title: 📖 [프로그래머스 알고리즘 문제 풀이] 2019 카카오 블라인드 - 오픈 채팅방 문제 풀이
category : Algorithm
tags: [Algorithm,programmers,2019 kakao blind, kakao, map]
---
# 📖 문제
2019 카카오 블라인드 - 오픈 채팅방 문제

<https://programmers.co.kr/learn/courses/30/lessons/42888>


# 🔍 접근법

2019 카카오 블라인드 - 오픈 채팅방 문제입니다.

채팅방에 들어오고 나가거나, 닉네임을 변경한 기록이 담긴 문자열 배열 record가 매개변수로 주어질 때, 모든 기록이 처리된 후,

최종적으로 방을 개설한 사람이 보게 되는 메시지를 구하는 문제입니다.

각 유저마다 고유한 uid가 있고 별명 같은 경우에는 나갔다가 다시 들어오거나

Change로 별명을 바꿀 수 있습니다.

여기서 확인해야할 것은 최종적으로 방을 개설한 사람이 보게 되는 메시지입니다.

유저가 들어가고 나가는 모든 메시지들을 표시해줘야 하는데

단, 모든 닉네임 변경이 끝난 후 해당 최종 닉네임으로 표시해줘야합니다.

우선, 각 Enter, Leave 같은 명령마다 문자열 메시지로 표시해주기 위해

commandMessageMap 에 각 명령마다 메시지를 초기화 해줍니다.

그리고 nicknameMap 에 사용자의 uid , 닉네임을 넣어 최종적으로는

마지막 닉네임이 각 uid에 저장되게 합니다.

그 후,

record 들을 탐색하며 Change인 경우를 제외하고 

각 기록들을 문자열 메시지로 저장해주면 답을 구할 수 있습니다. 

Map 을 잘 이용하면 되는 문제였습니다.
              
# 💻 코드

```
package problem.programmers;

import java.util.*;

class Solution {
    static Map<String,String> commandMessageMap;
    static Map<String,String> nicknameMap = new HashMap<>();
    public String[] solution(String[] record) {
        List<String> answerList = new ArrayList<>();
        
        commandMessageMapInit();
        setNicknameMap(record);

        for(int i=0; i<record.length; i++){
            String[] temp = record[i].split(" ");
            StringBuilder sb = new StringBuilder();
            String command = temp[0];
            String uid = temp[1];
            if(!command.equals("Change")) {
                String commandMessage = commandMessageMap.get(command);
                sb.append(nicknameMap.get(uid) + "님이 " + commandMessage);
                answerList.add(sb.toString());
            }
        }
        String[] answer = new String[answerList.size()];
        answerList.toArray(answer);
        return answer;
    }

    private void setNicknameMap(String[] record){
        for(int i=0; i<record.length; i++){
            String[] temp = record[i].split(" ");
            String command = temp[0];
            String uid = temp[1];

            if(temp.length>2) {
                String nickname = temp[2];
                nicknameMap.put(uid, nickname);
            }
        }
    }

    private void commandMessageMapInit(){
        commandMessageMap = new HashMap<>();
        commandMessageMap.put("Enter","들어왔습니다.");
        commandMessageMap.put("Leave","나갔습니다.");
    }
}
```