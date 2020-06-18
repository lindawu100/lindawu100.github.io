---
layout: post
title: Ruby & Kata (method - grep, grep_v & Enumerable)
---

### [Kata] Delete occurrences of an element if it occurs more than n times

### Task

Given a list lst and a number N, create a new list that contains each number of lst at most N times without reordering. For example if N = 2, and the input is [1,2,3,1,2,1,2,3], you take [1,2,3,1,2], drop the next [1,2] since this would lead to 1 and 2 being in the result 3 times, and then take 3, which leads to [1,2,3,1,2,3].

### Example
```ruby
delete_nth ([1,1,1,1],2)
# return [1,1]
delete_nth ([20,37,20,21],1)
# return [20,37,21]
```

題目：給定一組數字陣列及一個數字 N ，寫一個方法讓新陣列中的每個數字出現的次數小於 N 。

在方法定義變數 i & new_order ，使用迴圈搭配 if 條件判斷式：
```ruby
def delete_nth(order, max_e)
  i = 0
  new_order = []
  while i < order.length
    new_order << order[i] if new_order.grep(order[i]).count < max_e
    i += 1
  end
  return new_order
end
```
```ruby
p delete_nth([20,37,20,21], 1)
# [20, 37, 20]
p delete_nth([1,1,3,3,7,2,2,2,2], 3)
# [1, 1, 3, 3, 7, 2, 2, 2]
p delete_nth([1,3,3,1,2,2,2,2,3,1,3,7,2,3,2,2,2], 4)
# [1, 3, 3, 1, 2, 2, 2, 2, 3, 1, 3, 7]
```
第二組改用 map 取陣列中的每個數值與新的陣列比對：
```ruby
def delete_nth(order, max_e)
    new_order = []
    order.map { |i| new_order.push(i) if new_order.grep(i).count < max_e }
    return new_order
end
```
```ruby
p delete_nth([20,37,20,21], 1)
# [20, 37,21]
p delete_nth([1,1,3,3,7,2,2,2,2], 3)
# [1, 1, 3, 3, 7, 2, 2, 2]
p delete_nth([1,3,3,1,2,2,2,2,3,1,3,7,2,3,2,2,2], 4)
# [1, 3, 3, 1, 2, 2, 2, 2, 3, 1, 3, 7]
```

## Ruby method: grep
grep 可以回傳等於指定條件 (pattern)的陣列。
`grep(pattern) → array`
`grep(pattern) { |obj| block } → array`

換句話說，grep 可以在給定的內容中，比對有沒有符合條件的，找到的話，就會將符合條件的項目放在陣列中回傳：

在範圍 1~10 尋找符合條件(2~3)
```ruby
(1..10).grep 2..3
# [2, 3]
```
也可以給一個陣列，尋找符合條件(2~3)
```ruby
[2,3,4,5,6].grep 2..3
# [2, 3]
```
也能夠搭配 block 使用，尋找符合條件(2~3)，並讓每個結果都乘以二
```ruby
[2,3,4,5,6].grep(2..3) { |i| i * 2 }
# [4, 6]
```

## Ruby method: grep_v
grep_v 與 grep 相反，會回傳不等於指定條件 (pattern)的陣列。
`grep_v(pattern) → array`
`grep_v(pattern) { |obj| block } → array`

在範圍 1~10 尋找不符合條件(2~3)
```ruby
(1..10).grep_v 2..3 
# [1, 4, 5, 6, 7, 8, 9, 10]
```
也可以給一個陣列，尋找不符合條件(2~3)
```ruby
[2,3,4,5,6].grep_v 2..3
# [4, 5, 6]
```
也能夠搭配 block 使用，尋找不符合條件(2~3)，並讓每個結果都乘以二
```ruby
[2,3,4,5,6].grep_v(2..3) { |i| i * 2 }
# [8, 10, 12]
```

##  Enumerable Module

Enumerable 可說是一堆遞迴方法的集合體，也是 Ruby 的一個 Module ， Enumerable 中有很多好用的方法，像是 map 、 reduce。

整體來說， Enumerable Module 方法有三大種類：
* Traversal methods
* Searching methods
* Sorting methods

### 使用 Enumerable 時需要 block：
```ruby
[1,2,3].map { |n| n * n } # [1, 4, 9]
```
在 Ruby 中， 有幾個重要類別都有引入 Enumerable Module ，像是 Array, Hash & Range
```ruby
Array.included_modules
# [Enumerable, Kernel]
Hash.included_modules
# [Enumerable, Kernel]
Range.included_modules
# [Enumerable, Kernel]
```
Enumerator 是引入 Enumerable Module 的一個類別：
```ruby
Enumerator.class
# Class
Enumerator.included_modules
# [Enumerable, Kernel]
```
使用 Enumerable Module 方法時，在沒有給 block 的情況下會出現：
```ruby
[2, 3, 4].map
#<Enumerator: [2, 3, 4]:map>
```

### 如何在自訂類別中引入 Enumerable Module
首先要引入 Enumerable Module：
```ruby
class Users
  include Enumerable
  def initialize
    @user = "John"
  end
end

p Users.new.map { |user| user.upcase }
# undefined method `each' for #<Users:0x00007faae1831858 @user="John"> (NoMethodError)
```
引入 Enumerable Module 的類別需要有 each 方法回傳每一個值：
```ruby
class Users
  include Enumerable
  def initialize
    @user = "John"
  end

  def each
    yield @user
  end
end

p Users.new.map { |user| user.upcase }
# ["JOHN"]
```