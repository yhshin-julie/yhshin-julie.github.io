---
title: "[post01] Development-environment-configuration"
date: 2020-01-10 19:00:00 -0400
categories: dev-info
published: false
---

### 개발 환경 구성 (개인기록 용도)

#### 개요
프로그램 개발과 환경구성 방법을 정리하는데 목적을 가진다.

#### 사용 PC 사양 
  * CPU    : i5-6500 cpu @ 3.20GHz 
  - SSD    : 256GB   
  - 메모리  : 16GB  
  - OS     : Window 10  
  - 해상도 : 1280 x 1024

#### SW 구성
  - WAS            : Jeus   
  - DBMS           : Oracle 12c 
  - 형상관리        : SVN  
  - 배포관리        : Jenkins  
  - 라이브러리 관리 : Nexus  
  - 배치 서버       : Tomcat 8.0  

#### 로컬 개발환경 
  - 빌드 Tool      : apache-maven  
  - 로컬 WAS       : apache-tomcat  
  - 프레임워크      : eclipse (전자정부 표준 프레임워크 3.7 기반)  
  - Oracle JDK 1.8  
  - 로컬 Lib 저장소 : maven  
  - 형상서버와 로컬 개발 디렉토리 연결 : warkspace 
  - UI 개발 Tool   : Nexacro17  
  - 출력물 Tool    : oz Report 7.0

#### 개발 환경 설정  
* 01.링크 설정  
SVN 형상서버에서 CheckOut 받은 프로젝트의 'properties' > 'Java Build Path' > Link Source 에서    
링크할 개발 폴더 선택.

* 02.pom.xml 설정 

* 03.Datasource 설정  
로컬 개발 환경 Datasource 설정은 각 프로젝트의 globals.properties에서 설정한다.  

* 04.로그 설정  
로컬 개발 환경 로그 설정은 각 프로젝트으 logback.xml 에서 설정한다. 
  
* 05.로컬 환경 설정  
  - Web Module 등록 : Tomcat에서 웹 모듈 등록은 톰캣서버의 'Modules' 탭에서 추가 및 수정한다.  
(Path 명은 프로젝트 룰에 따른다.)    
   - UI Tool (Nexacro) 설정  
     + eclipse 내 연결된 Nexacro 프로젝트를 실행 > 'tools' > 'Options' 에서 Base Libary Path 를 프로젝트 폴더 안의 경로로 설정한다.   
     + Nexcro에서 Project > Generate > Generate Path 를 프로젝트 폴더로 설정한다.  

