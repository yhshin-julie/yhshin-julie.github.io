---
title: "[post04] Service sample"
date: 2020-01-13 16:40:00-0400
categories: dev-spring
toc: true
toc_sticky: true
---

## Service 선언부 

```java  
①
package yhshin.julie.post.projectEx.service;
②
import java.util.List;
import java.util.Map;
③
import javax.servlet.http.HttServletRequest;
④
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.transaction.annotation.Transactional;
import org.springframework.stereotype.Service;
import egovframework.rte.fdl.cmmn.EgovAbstractServiceImpl;
⑤
import yhshin.framework.exception.BusinessException;
import yhshin.framework.mybatis.CommonMapper;
⑥
/**
* @Usecase Name : 프로젝트 관리 
* @Class Name : PgManageService.java
* @author : yhshin
* @since : 2020.01.14
**/

⑦
@Service 
⑧
public class pgManageService extends EgovAbstractServiceImple {
       ⑨
       private static final Logger logger = LoggerFactory.getLogger(pgManageService.class);
       ⑩
       @Autowired
       ⑪
       private CommonMapper commonMapper;
}
```

1) 패키지 선언 : 프로젝트별 명명규칙에 따라 구성된다.                                
2) j2se API import                                                                   
3) j2ee API import                                                                   
4) 오픈소스 및 전자정부 표준 프레임워크 API import                                   
5) 프로젝트 내 공통 API import                                                       
6) 클래스 주석                                                                       
7) Service annotation                                                                
8) Service 클래스명 : EgovAbstractServiceImple 클래스를 상속받아야 한다.             
9) logger 생성 : log4j의 logger 클래스를 생성한다.                                   
10) CommonMapper Class 선언 : 데이터 처리를 담당하는 CommonMapper Class를 선언한다.  

## 조회 메서드   

```java
/**
* 업무 설명 : 프로젝트 관리 리스트 조회
* 작성자 : yhshin
* History : 1. 2020-01-14 신규작성 
**/

①
public List selectPgList(Map param) {
       ②
       return commonMapper.selectList("yhshin.julie.post.projectEx.pgManageMapper.selectPgList", param);

}
```

1) Service 메서드 선언 : Controller 클래스의 메서드와 동일하게 작성                                                  
    - 메서드 원형 : 조회 메서드는 기본적으로 List를 return하도록 선언하며, 결과값인 List는 DB 조회결과로 각 row를 EgovMap 으로 변환 후 List로 통합한 List<Map>구조로 되어 있다.             
    - IN 파라미터 : 클라이언트로 부터의 요청은 Map 객체에 Map<String,String> 구조로 데이터가 저장되어서 넘어오고, 이것을 MyBatis에서 SQL변수에 바인딩한다.                    
       
2) Mapper 메서드 호출 : Mapper 조회 메서드 호출 시                                                                   
    - 첫번째 파라미터 값 : SqlMap의namespace와 queryId를 사용.                                                       
    - 두번째 파라미터 값 : 검색조건 값을 담고 있는 Map을 전달한다.                                                   
    
## 저장 메서드 

```java
/**
* 업무 설명 : 프로젝트 관리 리스트 저장 
* 작성자 : yhshin
* History : 1. 2020-01-14 신규작성 
**/

①
@Transactional
②
public void savePgList(List list) {
       ③
       commonMapper.saveList("yhshin.julie.post.projectEx.pgManageMapper.", list, "pgMnage");
}
``` 
1) Transactional annotation 선언                                                                                
    - context-datasource.xml에 생성,수정,저장,삭제 메서드에 대한 transaction-manager로 설정할 수 있다.          
    - transaction 처리를 위해 annotation을 명시적으로 선언할 수 있다.     
    
2) 저장 메서드 선언                                                                                             
    - 메서드 원형 : 기본적으로 void로 선언.                                                                     
    결과값이 필요 한 경우에는, 알맞은 자료구조(Map, List, String 등)로 return 하도록 정의한다.                  
   - IN 파라미터 : 클라이언트로부터의 요청은 UI의 그리드 구조를 반영한 List<Map>형태로 데이터를 저장하고 있다.    
       
3) Mapper 메서드 호출                                                                                           
   - List 객체(Grid 데이터값)를 파라미터로 Mapper의 저장 메서드를 호출한다.                                     
   - 저장 건수를 원할 경우, Map으로 결과값을 받도록 한다.                                                       
   - saveList 메서드 구성 : 파라미터에 따라 내부적으로 insert,update,delete 분기 처리하도록 구성. 
   (SqlMap namespace, List객체, queryId) 
   
   ```java
   public Map saveList(String queryId, List list, String addName){
          Map result = new HashMap(); //객체생성 
          
          //초기화
          int insertCnt = 0;
          int updateCnt = 0;
          int deleteCnt = 0;
          
          //파라미터 list로 들어오는 데이터 체크 
          //1.null 이거나 사이즈0일때, 각 key에 0으로 셋팅.
          if(list == null || list.size() == 0){
             result.put("SATUS","S");
             result.put("ICNT","insertCnt");
             result.put("UCNT","updateCnt");
             result.put("DCNT","deleteCnt");
             
             return result;
          }
          
          //2.파라미터 list로 넘어오는 데이터 존재할때 for문 실행.
          for(int i=0; i<list.size(); i++;){
             Map doMap =  (Map)list.get(i); //list 를 돌면서 map으로 꺼내옴.
             
             //UI의SW 처리상태표시.
             String rowStatus = (String)doMap.get("rowStatus"); //map에서 rowStatus의 데이터를 꺼내옴.
          
             //rowStatus에 따라 분기처리(해당하는 Cnt++)
             if(rowStatus.equals(yhshin.MYBATIS.INSERT)){
                //doMap 파라미터를 가지고, 해당하는 쿼리를 찾아가도록 설정.
                insertCnt += this.insert(queryId + "insert" + addName, doMap);
             }else if(rowStatus.equals(yhshin.MYBATIS.UPDATE)){
             }else if(rowStatus.equals(yhshin.MYBATIS.DELETE)){
             }
          }
          
          result.put("SATUS","S");
          result.put("ICNT","insertCnt");
          result.put("UCNT","updateCnt");
          result.put("DCNT","deleteCnt");
          
          return result;
   }
   ```   
   
   - 비즈니스 로직 수행이 필요한 경우 ex)  
   
   ```java
   @Transactional
   public void savePgList(List list){ //List 형태로 파라미터가 넘어옴 
          for(int i=0; i<list.size(); i++){ //List의 사이즈 만큼 for문
              Map map = (Map)list.get(i); //List를 Map 형태로 꺼내옴 
              
              String rowType = (String)map.get("rowType"); //클라이언트 UI에서 넘어오는 row타입을 구분함.
              
              //row 타입별로 비즈니스 로직 처리
              if(MYBATIS.INSERT.equals(rowType)){ //rowType이 insert일 경우.
                  //map형태의 파라미터를 Mapper의 해당하는 queryId로 전달.
                  commonMapper.insert("yhshin.julie.post.projectEx.pgManageMapper.queryId", map); 
              }else if(MYBATIS.UPDATE.equals(rowType)){
              }else if(MYBATIS.DELETE.equals(rowType)){
              }
          }
   }
   ```   
