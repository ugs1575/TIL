---
title: Apex Data Type과 접근제어자
category: salesforce
order: 1
---

### Salesforce Application Anatomy

![스크린샷_2022-03-01_오후_7.33.13.png](1%20Apex%20Dat%20428a6/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-01_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.33.13.png)

### Execute Anonymous Window

- Developer Console > Debug > Open Execute Anonymous Window
- 저장 필요없이 apex 코드를 실행해 볼 수 있다.
- debug statement만 보려면 execute 후 debug only 체크박스 체크

### Data Types in Apex

![스크린샷 2022-03-01 오후 7.52.49.png](1%20Apex%20Dat%20428a6/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-01_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.52.49.png)

![스크린샷 2022-03-01 오후 7.54.43.png](1%20Apex%20Dat%20428a6/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-01_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.54.43.png)

### 접근제어자

```java
private | public | global
[virtual | abstract | with sharing | without sharing]
class ClassName [implements InterfaceNameList] [extends ClassName]
{
// The body of the class
}
```

- `global`
  - `global`로 정의되어진 클래스는 어디서든 접근 가능하다.
  - salesforce에서는 주로 managed package(예를들면 exe파일 같은 설치파일 같은 것)를 만들때 클래스를 global로 정의한다.
  - `global` 또는 `webservice` 로 정의되어진 메소드를 가진 클래스들은 반드시 `global` 로 정의해야한다.
- `virtual`
  - `virtual` 로 정의된 클래스는 상속과 오버라이드를 허용한다.
  ```java
  public virtual class Marker {
  		public virtual void write() {
  				System.debug('Writing some text');
  		}
  }
  ```
  ```java
  //Extension for the Marker class
  public class YellowMarker extends Marker {
  		public override void write() {
  				System.debug('Writing some text using the yellow marker');
  		}
  }
  ```

> references

- [https://www.youtube.com/watch?v=68X85SxAU1g&list=PLaGX-30v1lh1e8roeCUumUEel5ukdPubj&ab_channel=SalesforceApexHours](https://www.youtube.com/watch?v=68X85SxAU1g&list=PLaGX-30v1lh1e8roeCUumUEel5ukdPubj&ab_channel=SalesforceApexHours)

>

- [https://www.youtube.com/watch?v=TCxOlvxT8l4&list=PLaGX-30v1lh1e8roeCUumUEel5ukdPubj&index=2&ab_channel=SalesforceApexHours](https://www.youtube.com/watch?v=TCxOlvxT8l4&list=PLaGX-30v1lh1e8roeCUumUEel5ukdPubj&index=2&ab_channel=SalesforceApexHours)
