# Eager loading vs Lazy loading

**Lazy Loading은 연관된 데이터들을 실제로 접근하기 전에는 로딩하지 않는다.** Lazy loading의 예시를 보자.

```text
users = User.where(status: 'active').joins(:profile)
users.each do |user|
  user_date << {
    name = user.name
    age = user.profile.age
  }
end
```

데이터에 접근하기 위해 users를 호출하는 1번의 쿼리를 날리고, 이후 do 루프에서 `user.profile.age` 구문에 의해 profile에 접근할 때마다 N번의 쿼리를 추가로 날리게된다.

**이런 문제를 N+1 Problem라고 한다. 근본적으로 ORM에서 Lazy Loading을 사용할 때 발생되는 문제인 것이다.**

Rails는 이런 문제를 해결하기 위해 `includes`라는 메서드를 제공해준다.

> With includes, Active Record ensures that all of the specified associations are loaded using the minimum possible number of queries

자 아래처럼 코드를 수정해보자.

```text
users = User.where(status: 'active').includes(:profile)
users.each do |user|
  user_date << {
    name = user.name
    age = user.profile.age
  }
```

위 코드에서는 두 번의 쿼리가 발생한다. users 데이터를 찾을 때 한번, users에 대한 profile을 찾는데 한번. 이러한 로딩 방식을 eager loading이라고 부른다.

그럼 `joins`가 `includes`보다 효율적인 경우는 무엇일까? 아래처럼 필터의 역할을 할 경우 효율적이다.

```text
users = User.joins(:profile).where('profile.status = ?', 'active')
users.each do |user|
  user_date << {
    name = user.name
  }
end
```

do 루프에서 profile의 데이터를 사용하지 않고, profile은 단지 filter의 역할을 하는 것이다.

* [참고 1](https://medium.com/sjk5766/laravel-n-1-problem-88e1674e652e)

