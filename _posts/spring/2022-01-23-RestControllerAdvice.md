---
title: "[Spring] RestControllerAdvice"
last_modified_at: 2022-01-08 17:10
categories:
    - spring
tags:
    - spring
    - Java
    - exception

toc: true
toc_sticky: true
toc_label: "Contents"
---

## 학습 목표
* RestControllerAdvice에 대해 설명할 수 있다.
* ControllerAdvice와 차이점에 대해 설명할 수 있다.
* RestControllerAdvice의 사용법에 대해 설명할 수있다.

## 1. Spring boot에서 예외 관리하는 방법

* try-catch
* 요구사항에 의한 예외처리
* Interceptor로 예외처리

## 2. @ControllerAdvice

* try-catch의 단점인 코드 가독성 저하가 원인
* Single Responsibility Principle의한 비즈니스로직과 예외처리관련과의 역할 분리

## 3. @RestControllerAdvice

* @ControllerAdvice + @ResponseBody
* 단순한 예외처리는 @ControllerAdvice, 응답으로 json을 반환하고 싶다면 @RestControllerAdvice를 사용

* 구조

    ```java
    @Target(ElementType.TYPE)
    @Retention(RetentionPolicy.RUNTIME)
    @Documented
    @ControllerAdvice
    @ResponseBody
    public @interface RestControllerAdvice {

        /**
        * Alias for the {@link #basePackages} attribute.
        * <p>Allows for more concise annotation declarations &mdash; for example,
        * {@code @RestControllerAdvice("org.my.pkg")} is equivalent to
        * {@code @RestControllerAdvice(basePackages = "org.my.pkg")}.
        * @see #basePackages
        */
        @AliasFor(annotation = ControllerAdvice.class)
        String[] value() default {};
        ...
    }
    ```

* 예제

    ```java
    @RestControllerAdvice
    public class ExceptionAdvice{
        @ExceptionHandler(CustomException.class)
        public String custom(){
            return "custom!";
        }
    }
    ```

## 4. @ExceptionHandler
* @Controller와 @RestController사 적용된 Bean내에서 발생하는 예외를 잡아서 하나의 메소드로 처리해줌
* 구조

    ```java
    @Target(ElementType.METHOD)
    @Retention(RetentionPolicy.RUNTIME)
    @Documented
    public @interface ExceptionHandler {

        /**
        * Exceptions handled by the annotated method. If empty, will default to any
        * exceptions listed in the method argument list.
        */
        Class<? extends Throwable>[] value() default {};

    }
    ```

* 예제

    ```java
    @RestController
    public class MemberController{
        //...
        @ExceptionHandler(value = RuntimeException.class)
        public ResponseEntity<String> handle(IOException e){
            //...
        }
    }
    ```





