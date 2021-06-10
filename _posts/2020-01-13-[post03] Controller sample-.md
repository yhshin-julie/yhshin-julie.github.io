---
title: "[post03] Controller sample"
date: 2020-01-13 16:40:00-0400
categories: dev-spring
toc: true
toc_sticky: true
---

## Controller 선언부 

```java
①
pakage yhshin.julie.post.projectEx;

②
import java.util.HashMap;
import java.util.List;
import java.util.Map;

③
import.javax.servlet.http.HttpServletRequest;
④
import org.sl4j.Logger;
import.org.slf4j.LoggerFactory;
⑤
import.yhshin.julie.post.projectEx.service.PgMngService;
import.yhshin.framework.exception.BusinessException;
import.yhshin.framework.validation.annotation.MapValidated;
⑥
/**
* @Usecase Name : 프로젝트 관리 
* @Class Name : PgManageController.java
* @author : yhshin
* @since : 2020.01.13
**/
⑦
@Controller
⑧
public class PgManageController {
    ⑨
    private final Logger logger = 
    ⑩ 
    result.addAtrribute("데이터셋명", list);
}
```
  
1) 패키지 선언 (패키지 구성은 프로젝트의 시스템 구분에 따른다.)      
2) j2se API import       
3) j2ee API import       
4) 오픈소스 및 전자정부 표준 프레임워크 API import      
5) 소속 프로젝트의 공통 API import        
6) 클래스 주석석 선언        
7) Controller 어노테이션                  
8) Controller 클래스명       
9) logger 생성 : log4j 의 logger 클래스를 생성한다.         
10) Service 클래스 선언언 : 비즈니스 로직을 처리하는 Service Class 를 선언한다. 

## 조회 메서드 

```java
/**
* 업무 설명 : 프로젝트 관리 리스트 조회
* 작성자 : yhshin
* History : 1. 2020-01-13 신규작성 
**/

①
@RequestMappting("/yhshin/julie/post/projectEx/selectPgList.do")
②
public void selectPgList(Map param, Model result) throws BusinessException {
       ③
       logger.info("/yhshin/julie/post/projectEx/selectPgList.param : {}", param);
       ④ 
       List list = pgManageService.selectPgList((Map)param.get("데이터셋명"));
       ⑤
       result.addAttribut("데이터셋명", list);
}
```

1) URL 매핑 : 프로젝트 명명규칙에 따라 구성한다. 
2) 메서드 선언 
  - 메서드 원형 : 기본적으로 void로 선언하며, BusinessException throws한다. 
  - IN 파라미터 : 클라이언트로 부터의 요청은 Map 객체, 응답은 Model 객체에 저장한다.  
3) 파라미터 디버깅 : 테스트 및 운영 모드에서 디버깅 편의성을 위해 클라이언트에서 넘어온 값을 로그파일에 출력한다.
4) Service 메서드 호출 : Map객체(클라이언트에서 넘어온 값)를 파라미터로 Service 메서드를 호출하여 List 객체를 받는다. 
단건의 특정 컬럼조회는 Object나 String 으로 받을 수 있다.
5)결과값 저장 : Model 객체에 클라이언트로 전달 할 값을 key=value 형식으로 저장한다. (일반적인 Object나 String으로 데이터 전달도 가)능


## 저장 메서드 

```java
/**
* 업무 설명 : 프로젝트 관리 리스트 저장
* 작성자 : yhshin
* History : 1. 2020-01-13 신규작성 
**/

①
@RequestMapping(/yhshin/julie/post/projectEx/savePgList.do)
②
public void savePgList(Map param, Model result) throws BusinessException{
       ③
       logger.info("/yhshin/julie/post/projectEx/savePgList.param : {}", param);
       ④
       pgManageService.savePgList((List)param.get("데이터셋명"));
}
```

1) URL 매핑 : 프로젝트 명명규칙에 따라 구성한다.   
2) 메서드 선언   
  - 메서드 원형 : 기본적으로 void로 선언하며, BusinessException throws한다.    
  - IN 파라미터 : 클라이언트로 부터의 요청은 Map 객체, 응답은 Model 객체에 저장한다.  
3) 파라미터 디버깅 : 테스트 및 운영 모드에서 디버깅 편의성을 위해 클라이언트에서 넘어온 값을 로그파일에 출력한다.  
4) service 메서드 호출 : Map 객체(Grid 데이터값)를 파라미터로 Service 메서드를 호출한다.   
저장건수를 화면에 표시하려면 Map으로 결과값을 받도록 한다.   

```java
Map cnt = pgManageService.savePgList(list);

result.addAttribut("insertCnt", cnt.get("InsertCnt"));
result.addAttribut("updateCnt", cnt.get("updateCnt"));
result.addAttribut("deleteCnt", cnt.get("deleteCnt"));
```
