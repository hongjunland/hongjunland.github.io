---
title: "[Spring] Spring Security"
last_modified_at: 2022-01-08 17:10
categories:
    - spring
tags:
    - spring
    - Java
    - Spring Security
---

## 학습 목표

* Spring Security에 대해서 간략히 설명할 수 있다.
* Spring Security 기본 용어 4가지에 대해 설명 할 수있다.
* Spring Security의 아키텍처에 대해 설명할 수 있다.
* Spring Security의 처리 과정에 대해 설명할 수 있다.


## 1. Spring Security

> 스프링 기반의 애플리케이션의 보안을 담당하는 스프링 하위 프레임워크이다. <br>
주로 Servlet Filter와 이들로 구성된 filter chain으로의 위임 모델을 이용한다.<br>
그리고 보안과 관련해서 체계적츠로 많은 옵션을 제공해주기 때문에 개발자 입장에서는 일일이 보안관련 로직을 작성할 필요가 없다는 장점이 있음.

## 2. 기본 용어

1. Principal(접근 주체): 보호된 리소스에 접근하는 대상
2. Authentication(인증) : 보호된 리소스에 접근한 대상에 대해 어떤 user인지, 애플리케이션의 작업을 수행해도 되는 주체인지 확인하는 과정(ex: 폼 기반 login) 
3. Authorize(인가) : 해당 리소스에 대해 접근 가능한 권한을 가지고 있는지 확인하는 과정(after Authentication, 인증 이후)
4. Authority(권한) : 인증된 주체가 동작을 수행할 수 있도록 허가 되었는지 결정

    * 권한 승인이 필요한 부분으로 접근하려면, 인증 과정을 통해 주체가 증명되어야만 한다.
    * 권한 부여에도 두가지 영역이 존재하는데, 웹 요청 권한, 메소드 호출 및 도메인 인스턴스에 대한 접근 권한 부여.

## 3. Architecture

### 3.1 Login Form 과정(session-cookie방식 인증)

![spring-security-authentication-architecture.png](/assets/img/spring/spring-security-authentication-architecture.png)

1. User가 Form 을 통해 정보를 입력하고 로그인 요청을함.
2. AuthenticationFilter가 HttpServletRequest에서 User가 보낸 로그인정보를 인터셉트한다. HttpServletRequest에서 꺼내온 사용자 아이디와 패스워드를 인증을 담당할 AuthenticationManager 인터페이스에게 인증용 객체(UsernamePasswordAuthenticationToken)로 만들어서 위임한다. 

3. AuthenticationFilter에게 인증용 객체를 전달받는다.

4. 실제 인증알 할 AuthenticationProvider에게 인증용 객체를 다시 전달한다.

5. DB에서 사용할 인증 정보를 가져올 객체(UserDetailsService)에게 사용자 아이디를 넘겨 주고, DB에서 인증에 사용할 사용자 정보를 객체(UserDetails)로 전달받는다.

6. AuthenticationProvider는 UserDetails 객체를 전달 받은 이후, 실제 사용자의 입력 정보와 UserDetails 객체를 가지고 인증을 시도한다.

7. 인증이 완료 되면 사용자 정보를 가진 객체(Authentication)를 SecurityContextHolder에 담은 이후 AuthenticationSuccessHandle를 실행한다.(실패시 AuthenticationFailureHandler를 실행)

### 3.2 filter

![spring-security-authentication-architecture.png](/assets/img/spring/spring-security-filters.png)

|Filter name| Description
|--|--
|SecurityContextPersistenceFilter|SecurityContextRepository에서 SecurityContext를 로드하고 저장하는 일을 담당
|LogoutFilter|로그아웃 URL로 지정된 가상 URL에 대한 요청을 감시하고, 매칭되는 요청이 있다면 사용자를 로그아웃시킴.
|UsernamePasswordAuthenticationFilter|Form 기반 인증에 사용하는 가상 URL요청을 감시하고, 요청이 있으면 사용자의 인증을 진행
|DefaultLoginPageGeneratingFilter|Form 기반 또는 OpenID기반 인증에 사용하는 가상 URL에 대한 요청을 감시하고 Login Form 기능을 수행하는데 필요한 HTML을 생성
|BasicAuthenticationFilter|HTTP 기본 인증 헤더 감시 및 처리
|RequestCacheAwareFilter | 로그인 성공 후, 원래 요청 정보를 재구성 하기 위해 사용
|SecurityContextHolderAwareRequestFilter | HttpServletRequestWrapper를 상속한 SecurityContextHolderAwareRequestWapper 클래스로 HttpServletRequest정보를 감싼다. SecurityContextHolderAwareRequestWapper는 필터 체인상의 다음 필터들에게 부가정보 제공
|AnonymousAuthenticationFilter|이 필터가 호출되는 시점까지 사용자 정보가 인증되지 않았다면, 인증토큰에 사용자가 익명 사용자로 나타난다.
|SessionManagermentFilter|인증된 사용자와 관련된 모든 세션을 추적한다.
|ExceptionTranslationFilter|보호된 요청을 처리하는 도중에 발생할 수 있는 예외를 위임하거나 전달하는 역할
|FilterSecurityInterceptor|AccessDecisionManager로 권한부여 처리를 위임함으로써, 접근 제어 결정을 쉽게 해준다.

