---
layout: post
title: Ruby - Public, Private, Protected 方法有什麼差別
---

類別方法的存取限制主要分為三種
- Public: 所有人可以直接存取
- Protected: 介於 public & private 之間
- Private: 只有該類別內部可以存取

寫法有兩種
- 寫在方法定義之前
```ruby
class Cat
  def eat
    puts 'I like to eat fish'
  end

  protected
  def drink
    puts 'I like to drink water'
  end

  private
  def sleep
    puts 'I like to sleep'
  end
end
```
- 寫在方法定義
```ruby
class Cat
  def eat
    puts 'I like to eat fish'
  end

  def drink
    puts 'I like to drink water'
  end

  def sleep
    puts 'I like to sleep'
  end
  
  protected :drink
  private :sleep
  
end
```

```ruby=
# protected & private method 不能被實體存取
kitten = Cat.new
kitten.eat   # I like to eat fish
kitten.drink # No method error
kitten.sleep # No method error
```

## 繼承 Inheritence

Cat & Dog 都有 sleep 方法
```ruby=
class Cat
  def sleep
    puts 'I love sleep'
  end
end

class Dog
  def sleep
    puts 'I love sleep'
  end
end
```
重複的方法可以抽出成一個 class ， 並讓 Cat & Dog 繼承，如此一來 Cat & Dog 的實體都可以使用 sleep 方法
```ruby=
class Animal
  def sleep
    puts 'I love sleep'
  end
end

class Cat < Animal
end

class Dog < Animal
end
```
```ruby=
k = Cat.new
k.sleep   # I love sleep
Cat.sleep # No method error
```
## 模組 module

### include: 實體可以繼承 module 方法
```ruby=
module Swim
  def swim
    puts 'I can swim'
  end
end

class Cat
  include Swim
end

k = Cat.new
k.swim   # I can swim
Cat.swim # No method error
```

### extend: 類別可以繼承 module 方法
```ruby=
module Swim
  def swim
    puts 'I can swim'
  end
end

class Cat
  extend Swim
end

k = Cat.new
k.swim   # No method error 
Cat.swim # I can swim
```

參考資料：

1. [Public, Protected and Private Method in Ruby](https://kaochenlong.com/2011/07/26/public-protected-and-private-method-in-ruby/)
2. [[宅] 宅男臥軌日記(2) - ruby中的include, extend及require](http://cat-son.blogspot.com/2012/10/2-rubyinclude-extendrequire.html#sthash.ltKdDLJs.dpbs)