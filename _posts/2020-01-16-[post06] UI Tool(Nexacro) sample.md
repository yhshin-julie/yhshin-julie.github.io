---
title: "[post06] Nexacro(UI Tool) sample"
date: 2020-01-16 17:00:00 -0400
categories: dev-nexacro
published: false
---

### Nexacro 환경 구성 

#### TypeDefinition/Services
- 업무별로 개발 디렉토리 명칭을 설정한다.
- sbvcUrl : 서버 호출에 사용할 URL을 지정한다.

#### App Information/AppVariables
- 각 업무별 화면에서 공통적으로 사용할 global DataSet 및 global 변수를 설정한다. 

#### 자주 사용하는 cmmnFunction (Nexacro 공통 라이브러리)
- comp.js : 콤포넌트와 관련되는 CommonFuntion 파일 
- excel.js : 엑셀 import/export 와 관련되는 CommonFuntion 파일 
- util.js : String, Dataset, Date 와 관련되는 CommonFuntion 파일 
- frame.js : 프레임과 관련되는 CommonFuntion 파일 
- grid.js : 그리드와 관련되는 CommonFuntion 파일 
- popup.js : 팝업과 관련되는 CommonFuntion 파일 
- transaction.js : 트랜잭션 처리와 관련되는 CommonFuntion 파일 

### 개발 예제

#### Onload 이벤트 
화면 로딩이 완료 된 후, 가장먼저 수행되는 Funtion.

```javascript
this.form_onload = funtion(obj:nexacro.Form, e:nexacro.LoadEventInfo)
{
  //화면 로딩 완료후, UI 디버깅을 위한 창 호출하는 공통 Funtion
  this.gfnFormOnLoad(this);
  
  //업무별 화면 로딩 후 가장 먼저 수행되어야 할 Funtion 작업.
  //
  //
  
  
  //화면에서 사용할 공통코드 조회
  this.gfnGetCommonList(this.dsCommonList, "fnCallBack"); //공통코드 조회 대상이 되는 dataSet 지정 AND 콜백함수 호출.
  
  //초기 수행 로직 Funtion 호출 
  this.fnStartSet();
};
```

#### Transaction 처리
server의 서비스를 호출하는 작업 (Service Call이라고 명칭함.)

```javascript
this.fnSelectList = function()
{
  var strSvcId  = "select" ; //서비스 호출 ID 
  var strSvcUrl = "yhshin :: transactionSelectList.do" ; //server에서 호출할 .do 의 url 
  var inData    = "dsSelectList=dsSelectList:A" ; // server단의로 넘길 dataset 
  var outData   = "dsList=dsList" ; //server에서 결과로 넘어오는 명칭 
  var callBackFnc = "fnCallback";   //콜백함수명 지정
  var isAsync     = true;           //동기 OR 비동기 설정 
  
  //Service call 
  ① this.gfnTransaction(strSvcId, strSvcUrl, inData, outData, callBackFnc, isAsync);
}
```

`1` 글로벌 Transactioin funtion 호출 
- strSvcId : 트랜잭션을 구분하기 위한 ID값
- strSvcUrl : 트랜잭션을 요청 할 주소 
- inData : 입력값으로 보낼 데이터셋 ID (실제 dataset 명칭)
- outData : 트랜잭션 결과값으로 받을 데이터셋 ID 
- callBackFnc : 트랜잭션 결과를 받을 Funtion 명 (콜백함수)
- isAsync : 비동기통신 여부(생략가능, 기본적으로 비동기통신) 
