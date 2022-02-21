---
title: 데이터베이스와 엘로퀀트
category: Laravel
order: 1
---
# 5장 데이터베이스와 엘로퀀트

### 마이그레이션 작업 파일 정의

- up()메서드에는 새로운 변경 사항을 어떠헥 실행할지 정의하고, down() 메서드에는 적용한 변경 사항을 어떻게 되돌릴지 정의한다.

### migrate:refresh , migrate:fresh 차이

`migrate:refresh`

- 적용된 전체 마이그레이션을 되돌린 후 다시 마이그레이션을 적용한다.
- migrate:reset을 실행하고 migrate를 실행한 것과 동일하다

`migrate:fresh`

- 모든 테이블을 삭제하고 전체 마이그레이션을 다시 적용한다.
- refresh와 동일하지만, 마이그레이션의 down() 메서드 내용을 실행하지 않는다. 단순히 테이블을 삭제하고 up()메서드 내용을 샐행한다

### 모델 팩토리 사용하기

1. 인스턴스 여러개 생성

```php
//기본적인 형태
$post = (Post::factory())->create([
		'title' => 'My greatest post ever',
]);

//조금 복잡한 형태의 팩토리 사용
User::factory()->count(20)->create()->each(function ($u) use ($post) {
		$post->comments()->save(Comment::factory()->make(
				[
						'user_id' => $u->id,
				]
		));
});
```

- `count()` 메서드를 체이닝하면 모델 인스턴스를 여러 개 생성할 수 있다.
- 이 경우 하나의 인스턴스가 반환되지 않고 인스턴스의 컬렉션이 반환된다.
- 반환된 컬렉션은 배열과 같이 취급해도 되고 각각의 인스턴스를 다른 엔티티와 연결하거나 인스턴스 별로 각기 다른 메서드를 호출할 수도 있다. `each()` 메서드와 팩토리를 사용해 생성한 사용자가 작성한 댓글을 글과 연결했다.

1. 모델 팩토리를 정의할 때 연관관계 모델과 연결 추가하기
- 모델의 속성값에 클로저를 전달하고 이 클로저 안에서 연결하고자 하는 모델의 키를 사용한다.

```php
//ContactFactory.php 클래스 파일

use App\Models\Compnay;

public function definition()
{
		return [
				'name' => 'Lupita Smith',
				'email' => 'lupita@gmail.com',
				'company_id => Company::factory()
		];
}
```

- 각각의 클로저는 속성값을 나타내는 파라미터로 사용된다. 그리고 이 클로저는 해당 코드가 실행되는 시점에 도달하기 전까지 생성된 다른 속성값을 사용할 수 있다.

```php
//ContactFactory.php 클래스 파일

use App\Models\Compnay;

public function definition()
{
		return [
				'name' => 'Lupita Smith',
				'email' => 'lupita@gmail.com',
				'company_id => Company::factory()
		];
}
```