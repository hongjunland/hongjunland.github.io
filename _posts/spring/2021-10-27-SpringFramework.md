---
title: "[Spring] SpringFramework"
last_modified_at: 2021-10-27 00:55
categories:
    - spring
tags:
    - spring
    - Java
toc: true
toc_sticky: true
toc_label: "Contents"
---

![image](https://spring.io/images/spring-logo-9146a4d3298760c2e7e49595184e1975.svg)


## 1. 스프링이란

>자바 플랫폼을 위한 오픈 소스 애플리케이션 프레임워크로서 간단히 스프링(Spring)이라고도 한다. 동적인 웹 사이트를 개발하기 위한 여러 가지 서비스를 제공하고 있다. 대한민국 공공기관의 웹 서비스 개발 시 사용을 권장하고 있는 전자정부 표준프레임워크의 기반 기술로서 쓰이고 있다.

## 2. 주요 특징
* 경량컨테이너
* DI(Dependency Injection)
* AOP(Aspect Oriented Programming)
* POJO(Plain Old Java Object)
* IoC(Inversion of Control)
* 트랜잭션 처리 관리
* 영속성과 관련된 다양한 API(iBatis, MyBatis, Hibernate, JPA)
* 다양한 API에 대한 연동 지원(JMS, 메일, 스케쥴링등)


## 3 주요 모듈

![images](https://ww.namu.la/s/4f5a6277bde1b6a6bd13cbed717728b140df08a0e8e4e6ef904d162f71082a206adca523e24ab1f6489b025cca9c44e528324f6a0981066ed8e620d95fb5fdd416339983cf511aab2c60a98decea4ee9934761aafc160055efd75095437712a9)

* **Spring Core** : 스프링프레임워크의 핵심 기능을 제공하며, Core컨테이너의 주요 컴포넌트는 BeanFactory.
* **Spring Context** : 국제화된 메시지, Application의 Life Cycle, Event, 유효성 검증 등을 지원함으로써 BeanFactory 확장. 
* **Spring AOP** : 비즈니즈 로직 이외에 공통 설정 관리 기능을 통해 스프링프레임워크에 직접 통합.
* **Spring DAO** : Spring JDBC DAO 추상 계층은 다른 DB벤더들의 예외 핸들링과 오류 메시지를 관리하는 중요한 예외계층을 제공.
* **Spring ORM** : 스프링프레임워크는 여러 ORM에 플러그인되어, 객체 관계 툴(Hibernate,JPA, iBatis)등을 제공한다.
* **Spring Web** : Web Context 모듈은 Application Context상위에 구현되서, Web 기반 어플리케이션에 context를 제공.
* **Spring Web MVC** : 자체적으로 MVC 프레임워크를 제공하며, 스프랑만으로도 MVC기반의 웹 어플리케이션을 어렵지 않게 개발이 가능하다.
