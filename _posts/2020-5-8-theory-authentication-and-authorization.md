---
layout: post
title: 📖 [Theory] 💻 인증(authentication) & 인가 (authorization)
category : Theory
tags: [theory, security , authentication , authorization]
---
# 💻 🔐 인증(authentication) & 인가 (authorization)

    인증(authentication) 과 인가 (authorization)는 API 구현 중 보안을 위해 꼭 등장하는
    요소들입니다.
    인증과 인가가 무엇인지 알아보려고 합니다.
    서버에서 자원을 제공할 때 보안을 강화하기 위해 사용됩니다.
    
## 🔑 What is authentication?
    인증(authentication)은 유저가 누구인지 식별하는 과정입니다.
    서버에 저장되어 있는 자원에 대해 아무나 무차별적으로 접근하지 못하도록
    하기 위해서 인증이 필요합니다.
    
    간단히 말하자면 회원가입 / 로그인을 하여 내가 누구인지를 확인하는 것입니다.
    또한 회사에 출근하기 위해 출입증을 찍는 것과도 같습니다.
    
## 🔓 What is authorization?
    
    인가 (authorization)란 인증(authentication)을 통해서 어떤 유저인지 식별이 되고
    해당 유저에 따라 권한을 부여 받는 것을 말합니다.
    
    부여 받은 권한에 따라 자원에 접근이 가능합니다.
    
    간단히 말하면 아이디와 비밀번호를 통해 인증(authentication)을 거친 유저는
    일반 사용자로서의 권한을 부여받고 마이페이지에서 자기에 대한 정보를 확인할 수 있습니다.
    
    또한 admin의 아이디와 비밀번호를 통해 인증(authentication)을 거친 admin 유저는
    admin 으로서 권한을 부여받고 모든 일반 유저들의 정보를 확인할 수 있습니다.
    
## 결론

    인증 (authentication) 과 인가 (authorization)는 서버의 자원에 접근할 때 아무나 접근 할 수 없도록 보안을 위해 존재합니다.

    서버의 자원에 접근할 권한을 받기 위해 (authorization) 서버측에 인증 (authentication)을 받습니다.

    인증 (authentication)에 성공하여 인가 (authorization) 받은 권한에 따라 서버의 자원에 접근할 수 있습니다.


------------------------------------------------------
### References

https://hanee24.github.io/2018/04/21/authentication-authorization/