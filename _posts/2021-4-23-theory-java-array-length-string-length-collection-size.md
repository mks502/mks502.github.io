---
layout: post
title: 📖 [Java] 💻 length 와 length() 그리고 size() 차이 
category : Theory
tags: [theory, java, inheritance]
---

# 💻 Java length 와 length() 그리고 size() 차이 

    안녕하세요. 👋
    
    오늘은 자바의 length 와 length() 그리고 size() 차이에 대해
    
    간단히 정리해보도록 하겠습니다.
    
    대부분 코딩할 때 intelliJ와 같은 IDE를 사용해서
    
    자동완성으로 인해 실수할 일이 없지만,
    
    코딩테스트 같은 IDE 환경이 아닌 곳에서 코딩할 때 햇갈리다보니
    
    정리해보게 되었습니다.

## length , length() , size() 의 구분

1. length

 - Arrays(int[], double[], String[])

 - length 는 배열의 길이를 나타냅니다.
 
 - ※ 배열의 길이는 할당 될 때 정해지는 상수이므로 메소드가 아닌 필드로 제공됩니다.

2. length()

 - String related Object(String, StringBuilder etc)

 - length()는 문자열의 길이를 알려주는 메소드입니다.

3. size()
 - Collection Object(ArrayList, Set etc)

 - size()는 컬렉션프레임워크 타입의 길이를 알려주는 메소드입니다.
 
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Main {
    public static void main(String[] args) throws Exception {
        int arr[] = new int[]{1,2,3};
        List<Integer> list = new ArrayList<>(Arrays.asList(1,2,3,4,5));
        String str = "Hi";

        System.out.println(arr.length);         //Arrays        length      출력  3
        System.out.println(list.size());        //Collection    size()      출력  5
        System.out.println(str.length());       //String        length()    출력  2
    }
}
```


### 끝

간단하게

자바의 length 와 length() 그리고 size() 차이에 대해

간단히 정리해보았습니다.

감사합니다. 🙏

-------------------------------------------------

Reference

https://enter.tistory.com/108
