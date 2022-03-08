---
title: SOQL and SOSL
category: salesforce
order: 3
---

# 4. Basic of SOQL and SOSL in Salesforce

### 자식 테이블에서 부모 테이블 데이터 불러오기

- 간단하게 테이블 명.필드명으로 불러올 수 있다.

```sql
SELECT Name, Account.Name, Account.Type
	FROM Opportunity
```

### 부모 테이블에서 자식 테이블

- 자식테이블을 개체 관리자 > 부모 필드를 클릭해보면 하위 관계 이름으로 쿼리를 작성하면 된다.

![스크린샷 2022-03-06 오후 10.40.36.png](4%20Basic%20of%203b76a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-06_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.40.36.png)

```sql
SELECT Name,
(
		SELECT Id, Name, CloseDate
		FROM Opportunities
)
FROM Account
```

### SOSL 활용

- SOSL 을 사용하면 여러개의 개체에서 한번에 검색할 수 있다.
- 예를들어 text, email, phone 필드를 여러개의 개체에서 검색할 때 사용할 수 있다.
- SOSL은 `List<List<SObject>>` 를 결과값으로 리턴한다.

```java
List<List<SObject>> searchLIst = [FIND 'Joe*' IN ALL FIELDS RETURNING Account(Id, Name), Lead];

Account[] accs = searchList[0];
Lead[] leads = searchList[1];
```

### Query Plan

- Developer Console > Help > Preferences > Enable Query Plan 체크
- Query Editor에서 Execute 오른쪽에 Query Plan 버튼이 생김 > 버튼 클릭

### DML 쿼리 결과 확인

- 쿼리결과를 확인 하고 싶을 때는 아래와 같이 Database method를 사용하면 된다. 아래 코드와 같이 insert 메서드에서 두번째 파라미터로 false를 주면 결과를 알 수 있다.

```java
Opportunity opp1 = new Opportunity(
		Name = 'New Opportunity ApexHours 0213',
		CloseDate = Date.today(),
		Amount = 1000,
		StageName = 'Prospecting',
		AccountId = '0013i000004HHTrAAO'
);

//필수 값인 StageName 이 빠져서 쿼리가 실패 할 것이다.
Opportunity opp2 = new Opportunity(
		Name = 'New Opportunity ApexHours 223',
		CloseDate = Date.today(),
		Amount = 5000,
		AccountId = '0013i000004HHTrAAO'
);

List<Opportunity> lstOpp = new List<Opportunity> { opp1, opp2 };
Database.saveResult[] sr = Database.insert(listOpp, false);

for (Database.SaveResult var : sr) {
		System.debug('Result - ' + var.isSuccess());
}
```

- output

```
Result - true
Result - false
```

### Transaction 처리

```java
Account acc = new Account(
		Name = 'Test Account 0213'
);
insert acc;

Account accQuery = [SELECT Id FROM Account WHERE Name = 'Test Account 0213'];

System.Savepoint spt = Database.setSavepoint();

try {
		Contact con = new Contact(
				LastName = 'Contact'
		);
		insert con;
} catch (Exception ex) {
		Database.rollback(spt);
}

```
