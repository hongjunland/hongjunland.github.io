---
title: "[Spring] IoC&Container"
last_modified_at: 2021-10-27 00:55
categories:
    - spring
tags:
    - spring
    - Java
---
# 1. IoC (Inversion of Control)
## 1.1 IoC란?
> 객체의 모든 제어권한을 다른 클래스(컨테이너)에게 넘긴다. 
* 객체간의 연결 관계를 Runtime에 결정
* 객체간의 관계가 느슨하게 연결됨
* ex) DL, DI

## 1.2 IoC유형

* DL(Dependency Lookup)
    * 컨테이너가 lookup context를 통해서 필요한 리소스나 객체를 얻는 방식이다.
    * 사용시 컨테이너 종속성 증가. (그래서 주로 DI를 사용)
    * Lookup한 객체를 캐스팅 해주어야 함.

* DI(Dependecy Injection)
    * 객체에 lookup코드를 사용하지않고 컨테이너가 직접 의존 구조를 객체에 설정 할 수 있도록 지정해주는 방식.
    * 객체가 컨테이너의 존재여부 알 필요 없음
    * ex)Setter, 생성자, Method

# 2. Container
## 2.1 Container란?
> 객체의 라이프 사이클을 담당하며, 애플리케이션 사용에 필요한 주요기능을 제공한다.

## 2.2 기능
* 생명주기 관리
* Dependency 객체 제공
* Thread 관리
* 기타 App 실행에 필요한 환경

## 2.3 필요성
* 비즈니스 로직 외에 부가적인 기능들에 대해 독립적으로 관리되어야함.
* 서비스 look up 이나 Configuration에 대한 일관성 유지
* 서비스 객체를 사용하기 위해 각각 Factory, Singleton등을 직접 구현할 필요 없음.

## 2.4 IoC Container
* 객체의 생성, 관계설정, 제거등의 작업을 소스코드 대신 독립된 컨테이너가 담당.
* 컨테이너가 소스코드 대신에 객체에 대한 제어권을 담당
* ex) BeanFactory, ApplicationContext 등

