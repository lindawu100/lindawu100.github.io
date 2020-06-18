---
layout: post
title: Ruby - Symbol
---

Symbol是帶有有名字的物件，寫法是前方有個冒號，在Ruby可以自由建立Symbol。

## Symbol在記憶體的位置相同

每次要產生Symbol的object id時，都是去找同一個物件；在找string時，則是找不同的兩個物件：

* Symbol會有相同的object id
* String會有不同的object id

```ruby
# Use .object_id to get the id (integer) of the object
puts :symbol.object_id          #123424
puts :symbol.object_id          #123424

puts "string".object_id         #21938983
puts "string".object_id         #21938829

# Use .freeze to fix the "string"
puts "string".freeze.object_id  #21938935
puts "string".freeze.object_id  #21938935

```

## Symbol 效能 > String

Symbol效能比字串好，Symbol會直接比較是不是同一個物件；String則是一個一個字母逐一比對。

## Symbol內容不可變，String內容可變

```ruby
puts :symbol       # symbol
puts "string"      # string

puts :symbol[3]    # b
puts "string"[3]   # i

:symbol[3] = "u"   #  returnerror
"string"[3] = "u"
```

## 查詢Symbol

```ruby
Symbol.all_symbols   # return symbol table
```

## Symbol無法當作變數

```ruby
:symbol = "Hello, world"   # return error
```

Symbol具有不可變且效能好的特性，經常作為辨識器使用：
* 方法
* 變數
* hash key

Ruby在2.2以後加入Symbol GC([Garbage collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)))，讓Symbol可以被回收，而不是堆積在記憶體。



References:
* [Ruby Guides](https://www.rubyguides.com/2018/02/ruby-symbols/)
* [為你自己學Ruby on Rails](https://railsbook.tw/chapters/06-ruby-basic-2.html)
