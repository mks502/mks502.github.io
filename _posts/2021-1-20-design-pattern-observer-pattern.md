---
layout: post
title: 📖 [디자인패턴] 💻 Observer Pattern (옵저버 패턴/관찰자 패턴)
category : Design Pattern
tags: [design pattern, observer]
---

# 💻 Observer Pattern (옵저버 패턴/관찰자 패턴)

    안녕하세요. 👋
    
    오늘은 Observer Pattern (관찰자 패턴 ) 에 대해
    
    알아보려고 합니다.

## What is Observer Pattern? (옵저버 패턴이란?)

- Observer Pattern 이란

      옵서버 패턴(observer pattern)은 객체의 상태 변화를 관찰하는 관찰자들, 즉 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다
      메서드 등을 통해 객체가 직접 목록의 각 옵저버에게 통지하도록 하는 디자인 패턴이다. 주로 분산 이벤트 핸들링 시스템을 구현하는 데 사용된다.
      발행/구독 모델로 알려져 있기도 하다.
    
      출처(위키백과) https://ko.wikipedia.org/wiki/%EC%98%B5%EC%84%9C%EB%B2%84_%ED%8C%A8%ED%84%B4

### Observer Pattern의 활용

옵저버 패턴의 정의처럼 보통 발행/구독 모델로 알려져있습니다.
또한 관찰자패턴이라고도 합니다.
하나의 Publisher(발행자)에게 여러 Subscriber (구독자)가 있을 수 있기 때문에
일대일이나 일대다 관계를 이룰수 있습니다. 그리고 Publisher(발행자)의 상태가
업데이트된다면 이 Publisher(발행자)를 구독(관찰)하고 있는 옵저버들에게 업데이트가
되었음을 알려줍니다.

옵저버 패턴은 푸시 방식과 풀 방식이 있습니다.
또한 Publisher의 정보나 상태가 변경될 때마다 Observer에게 전파해주는 것이 Push방식이고
Observer에서 정보가 필요할 때마다 Publisher에게 가져오는 방식을 Pull방식이라고 합니다.

이 옵저버 패턴의 대표적인 사용처는 자바의 Swing이나 안드로이드의 View나 Button 등의 
위젯에서 각종 이벤트를 받을 때 쓰입니다.

---------

## Observer Pattern 적용 예시

옵저버 패턴을 좀 더 이해하기 쉽게 하기위해 뉴스 구독이나 유튜브 구독등을 예시로 들수있습니다.
유튜브 구독을 예시로 보여드리겠습니다.

보통 흔히 많이들 하는 것처럼 어떤 유튜브 채널을 구독하고 알림을 설정해두면
구독한 채널(Publihser)의 영상이 새로 올라올 때 (Publisher 정보 업데이트)
구독한 사람들에게(Subscriber / Observer) 알려줍니다. (push 방식)

또한 구독해제를 통해 구독을 그만둘 수도 있습니다. (unsubscribe)

![diagram](/images/2021-1-20/diagram.JPG)

#### Publisher 인터페이스

```java
package observer;

public interface Publisher {
    public void add(Observer observer);

    public void delete(Observer observer);

    public void notifyObserver();

}

```

#### Observer 인터페이스
```java
package observer;

public interface Observer {
    public void update(String title, String content);
}

```
#### YoutubeChannel 클래스 (Publisher)

```java
package observer;
import java.util.ArrayList;
import java.util.List;

public class YoutubeChannel implements Publisher{
    private List<Observer> observerList;
    private String title;
    private String content;

    public YoutubeChannel() {
        this.observerList = new ArrayList<>();
    }

    @Override
    public void add(Observer observer) {
        observerList.add(observer);
    }

    @Override
    public void delete(Observer observer) {
        int targetIndex = observerList.indexOf(observer);
        observerList.remove(targetIndex);
    }

    @Override
    public void notifyObserver() {
        for(Observer observer : observerList) {
            observer.update(title, content);
        }
    }

    public List<Observer> getObserverList() {
        return observerList;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }

    public void setNewsInfo(String title, String content) {
        this.title = title;
        this.content = content;
        notifyObserver();
    }
}

```

#### AnnualSubscriber 클래스 (Subscriber / Observer)

```java
package observer;

public class AnnualSubscriber implements Observer {
    private String newsString;
    private Publisher publisher;

    public AnnualSubscriber(Publisher publisher) {
        this.publisher = publisher;
        publisher.add(this);
    }

    @Override
    public void update(String title, String content) {
        this.newsString = "title : " + title + "\n" + "content : " + content + "\n========";
        display();
    }

    public void unsubscribe() {
        this.publisher.delete(this);
    }

    private void display() {
        System.out.println("AnnualSubscriber \n");
        System.out.println("Today News\n" + newsString);
    }
}
```

#### EventSubscriber 클래스 (Subscriber / Observer)

```java
package observer;

public class EventSubscriber implements Observer {
    private String newsString;
    private Publisher publisher;

    public EventSubscriber(Publisher publisher) {
        this.publisher = publisher;
        publisher.add(this);
    }

    @Override
    public void update(String title, String content) {
        this.newsString = "title : " + title + "\n" + "content : " + content + "\n========";
        display();
    }

    public void unsubscribe() {
        this.publisher.delete(this);
    }

    private void display() {
        System.out.println("EventSubscriber \n");
        System.out.println("Today News\n" + newsString);
    }
}
``` 

#### Main문
```java
package observer;

public class Main {
    public static void main(String[] args) {
        YoutubeChannel youtubeChannel = new YoutubeChannel();
        AnnualSubscriber annualSubscriber = new AnnualSubscriber(youtubeChannel);
        EventSubscriber eventSubscriber = new EventSubscriber(youtubeChannel);
        youtubeChannel.setNewsInfo("첫 영상입니다.", "안녕하세요!");
        youtubeChannel.setNewsInfo("두번째 영상입니다.", "또 봐주셔서 감사합니다!");

        eventSubscriber.unsubscribe();
        System.out.println("eventSubscriber 탈퇴 후\n\n");
        youtubeChannel.setNewsInfo("구독자가 100만명이 되었습니다", "감사합니다!");
    }
}
```

#### 실행결과

![result](/images/2021-1-20/result.JPG)

-----------------


### 끝

    오늘은 옵저버(관찰자) 패턴 (observer pattern)에 대해 알아보았습니다.
    
    감사합니다. 🙏

-------------------------------------------------

Reference

https://ko.wikipedia.org/wiki/%EC%98%B5%EC%84%9C%EB%B2%84_%ED%8C%A8%ED%84%B4

https://flowarc.tistory.com/entry/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EC%98%B5%EC%A0%80%EB%B2%84-%ED%8C%A8%ED%84%B4Observer-Pattern