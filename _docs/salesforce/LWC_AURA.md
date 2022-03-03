---
title: LWC AURA
category: salesforce
order: 3
---
# LWC AURA

### helper, controller

helper는 controller에서 가져다 쓰는 작은 조각 소스

### 컴포넌트에서 attribute변수 가져오기

`{!v.newMessage}`

### aura handler init method

```html
<aura:handler name="new Message" type="{!this}" action="{!c.doInit}" />
```

### communicate with salesforce~

- 컴포넌트안에서 apxc 컨트롤러 부르기

```html
<aura:component controller="MyOpportunityListController" implements="flexipage...." />
```

- `@AuraEnabled`

apxc 컨트롤러 안에 method 상단에 위 어노테이션을 선언해주어야 컴포넌트안에서 부를수있다

- 컴포넌트 id 정의

```jsx
aura:id="something"
```

- 컴포넌트 안에서 정의된 아이디로 컨트롤러에서 value 가져오고 helper 함수로 넘겨주는 예제

```jsx
searchOpportunities: function(component, event, helper) {
		var searchValue = component.find("searchField").get("v.value");
		helper.fetchOppHelper(searchValue, component);
}
```

- creating the action (헬퍼에서 apxc 컨트롤러의 메소드를 부를때)

```jsx
var action = component.get("c.fetchOpportunity");
action.setParams({
		"searchKeyword": searchCal
});
```

- apxc 컨트롤러 반환값 component attribute 값에 세팅

```jsx
action.setCallback(this, function(response){
		var state = response.getState();
		if (state === "SUCCESS") {
				component.set("v.lstOpportunity", response.getReturnValue());
		} else {
				alert("An error occurred while fetching the data");
		}
});
$A.enqueueAction(action);
```

### parent to child communication

- attribute를 정의해주면서 넘겨주면 된다.

```jsx
<aura:application extends="force:slds">
		<c:MyOpportunityListComponent newMessage="This is a new message" />
</aura:application>
```

### Debugging

- debugger statement  == breakpoint
- response.getReturnValue() 를 콘솔에 타이핑하면 반환갑 볼 수 있음

> Reference
> 
- [http://www.apexhours.com/introduction-to-aura/](http://www.apexhours.com/introduction-to-aura/)
- [https://www.youtube.com/watch?v=Yz_IJzC6yfU&list=PLaGX-30v1lh1e8roeCUumUEel5ukdPubj&index=17&ab_channel=SalesforceApexHours](https://www.youtube.com/watch?v=Yz_IJzC6yfU&list=PLaGX-30v1lh1e8roeCUumUEel5ukdPubj&index=17&ab_channel=SalesforceApexHours)