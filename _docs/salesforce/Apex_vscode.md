# 3. salesforce 개발환경 vscode 연동

### 준비 사항

- JDK 8 이상
- salesforce CLI
    - 다운로드 링크 : [https://developer.salesforce.com/tools/sfdxcli](https://developer.salesforce.com/tools/sfdxcli)
- salesforce extensions for vscode

### 기본적인 세팅

1. salesforce CLI를 먼저 설치해준다
2. Salesforce Extension Pack 설치
    
    ![스크린샷 2022-03-05 오후 4.28.27.png](3%20salesfor%20e3b5c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.28.27.png)
    
3. Command Palette > SFDX:Create Project with Manifest 선택
    
    project 유형 standard 선택
    
    project 이름을 입력해준다.
    
4. Command Palette > SFDX:Authorize an Org 선택
    
    나는 테스트 환경을 세팅해 줄 예정이기 때문에 Sandbox 를 선택한다. 
    
    로그인 창이 뜨면 로그인 해준다. 
    
    Salesforce CLI 접근 허용창이 뜨면 허용을 눌러준다.
    

### sandbox에서 소스 내려받기

1. manifest 폴더 밑에 package.xml 파일이 있을 것이다.
    
    해당 파일을 열어 오른쪽 마우스 클릭 후 SFDX:Retrieve Source in Manifest from Org를 클릭
    
    force-app/main/default/classes 해당 경로로 들어가보면 작성했던 apex 클래스들을 볼 수 있을 것이다~!
    

### sandbox에 소스 올리기

예를 들어 apex클래스를 작성해서 올린다고 한다면, 

1. Command Palette > SFDX:Create Apex Class 선택
    
    class 이름 작성
    
    생성위치 선택 (force-app/main/default/classes)
    
2. 해당 파일 열어서 오른쪽 마우스 클릭 후 SFDX:Deploy This Source to Org 클릭

### vscode 환경에서 apex 실행

1. scripts > soql > 원하는 파일 열기
2. 원하는 query를 드래그 한 후 Command Palette > SFDX: Execute SOQL Query with Currently Selected Text 선택, 아래와 같이 결과를 볼 수 있다.
    
    ![스크린샷 2022-03-05 오후 5.03.29.png](3%20salesfor%20e3b5c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.03.29.png)
    

### vscode 환경에서 query 실행

1. scripts > apex > 원하는 파일 열기
2. 원하는 문장을 드래그 한 후 Command Palette > SFDX: Execute Anonymous Apex with Currently Selected Text 선택
    
    ![스크린샷 2022-03-05 오후 5.06.34.png](3%20salesfor%20e3b5c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.06.34.png)
    
    아래와 같이 결과를 볼 수 있다.
    
    ![스크린샷 2022-03-05 오후 5.07.34.png](3%20salesfor%20e3b5c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.07.34.png)
    

> References
> 
- [https://www.youtube.com/watch?v=fpKZZ3kT-iU&list=PLaGX-30v1lh1e8roeCUumUEel5ukdPubj&index=3&ab_channel=SalesforceApexHours](https://www.youtube.com/watch?v=fpKZZ3kT-iU&list=PLaGX-30v1lh1e8roeCUumUEel5ukdPubj&index=3&ab_channel=SalesforceApexHours)