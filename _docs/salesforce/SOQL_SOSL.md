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
