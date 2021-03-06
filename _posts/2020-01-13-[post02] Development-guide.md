---
title: "[post02] Development-guide"
date: 2020-01-13 16:00:00 -0400
categories: dev-spring
toc: true
toc_sticky: true
--- 

> 개인기록의 목적으로 작성된 게시물입니다.

## 1. 프로젝트 구성  
프로젝트 내의 업무별 관리를 위해 시스템별 코드 및 패키지 코드 명칭을 정의하여 분류한다.  

## 2. 아키텍처 구성   
아키텍처 담당자가 프로젝트 특성에 맞게 일부분을 커스터 마이징하여 구성할 수 있다.    

   2-1. 아키텍처 구성 요소    
    * `Web browser`  
    : 클라이언트 실행 영역.       
    * `DispatcherServlet`  
    : 클라이언트의 요청 처리.  
    * `HandlerMapping`  
    : 클라이언트의 요청 경로를 이용하여 처리할 Controller bean 객체를 DispatcherServlet에 전달.     
    * `HandlerAdapter`  
    : Controller의 메서드 검색 및 호출.    
    * `ArgumentRecolver`   
    : Json, xml, csv 등의 처리 구조를 파싱하여 서버 프로그램에서 사용할 수 있는 GenericMap 이라는 자료 구조를 생성.      
    * `Controller`      
    : 화면 처리 Layer (개발자 영역).   
    * `Service`      
    : 비즈니스 처리 Layer (개발자 영역).    
    * `EgovAbstractSerciceImpl`     
    : 전자정부 표준프레임워크 비즈니스 처리 클래스, 호환성 인증을 위해 필수적으로 상속 받아야 함.    
    * `ServiceImpl`     
    : 비즈니스 처리 Layer (개발자 영역)    
    * `EgovAbstractMapper`       
    : 전자정부 표준프레임워크 데이터 처리 클래스, 호환성 인증을 위해 필수적으로 상속받아야함.    
    * `Mapper`       
    : 데이터 처리 Layer.   
    * `SqlMap`       
    : 업무처리 Sql.    
    * `ViewResolver`    
    : 클라이언트의 요청 처리 결과를 화면으로 전다달하기 위해 View 객체를 검색하여 호출.    
    * `View`      
    : 표준 HTML 을 비로한 다양한 UI 소프트웨어의 특성에 맞게 처리 결과를 화면으로 전달.   
    * `Jsp`    
    : View 에 의해 각각 전달되는 화면 실행 프로그램.
   
      
## 3. Controller        
클라이언트의 서비스 요청에 대한 기본처리, 비즈니스 로직 처리를 제외한 처리를 담당한다.        
Validator를 이용한 유효성 검증 작업도 Controller에서 처리하도록 한다.      

   3-1. Method Parameter Type       
   Spring MVC에서 제공하는 파라미터 유형은 무엇이든 사용가능하며, Controller의 메소드 파라미터로 추가하면 바로 사용 할 수 있음.       
   
   * `HttpSercletRequest, HttpServletResponse, HttpSession`    
   : J2EE spec 요청, 응답, session object .    
   * `@RequestParam Annotation parameter`       
   : 특정 Request parameter key의 value에 접근시 사용.       
   * `Map, ModelMap, ModelandView`      
   : View에서도 참조하고 있는 request에 담긴 데이터를 참조시에 사용.       
   * `ModelAttribute Annotation paraeter`    
   : 요청에 담긴 데이터를 VO(command Object) 에 바인드 시에 사용.      
   * `SessionSatatus`      
   : form 처리를 완료했음을 처리하기 위해 사용.     
   : @SessionAtrribute Annotation 을 명시한 Session 속성을 제거하도록 이벤트를 발생시키기 위해 사용.
   
