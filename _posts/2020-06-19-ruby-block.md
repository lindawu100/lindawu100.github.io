---
layout: post
title: Ruby - Block, Proc, Lambda
---

程式碼區塊(Block)在Ruby大量被使用，寫法有`do...end`, 花括號`{...}`兩種

```ruby
# do...end
5.times do |i| 
  puts i
end

# {}
(1..100).select { |n| n.odd? }
```

### `{...}`, `do...end`差異
花括號{...}結合率強、優先順位較高，do...end結合率弱
```ruby
list = [1, 2, 3, 4, 5]

p list.map { |item|
  item * 2
}
# [2, 4, 6, 8, 10]

p list.map do |item|
  item * 2
end
# <Enumerator: [1, 2, 3, 4, 5]:map>
```

### Block 無法單獨存活，不是物件、不是參數
```ruby
{ puts 'Hello world' }
# syntax error

do 
  puts "I'm do...end block"
end
# syntax error
```

### 如何使用程式碼區塊(Block)？
轉讓控制權(yield)，暫時將控制權交給 block ，執行完 block 程式碼後再傳回控制權
```ruby
def say_hello
  puts "Hello, 你好"
  yield
  puts "Hello, everyone"
end

say_hello { 
  puts "I'm here!" 
}

# Hello, 你好
# I'm here!
# Hello, everyone
```

Block 最後一行執行結果會自動變成回傳值，且 Block 內不能使用 return
```ruby
def say_hello
  puts "Hello, 你好"
  yield
  puts "Hello, everyone"
end

say_hello { 
  return "I'm here!" 
}
# LocalJump error
```

### Block 物件化後可以單獨存活
Ruby 有兩個方法可以讓 Block 物件化

- Proc
  - `Proc.new`後接 block 就可以物件化 block，之後可以使用`call`執行 block 內程式碼

```ruby
add_two = Proc.new { |n| n + 2 }

p add_two.call(3)  # 5
p add_two[3]       # 5
p add_two.(3)      # 5
p add_two.===(3)   # 5
```

- Lambda

```ruby
# Lambda建立物件化的block, call使用Lambda包住的block
add_two = lambda { |n| n + 2 }
add_two = -> (n) { n + 2 }


p add_two.call(3)  # 5
p add_two[3]       # 5
p add_two.(3)      # 5
p add_two.===(3)   # 5
```

### Proc & Lambda 差異
Proc 的 block 內加入 `return` ，就會立即跳出
Lambda 的 block 內加入 `return` ，會交回控制權，繼續執行方法後續的城市碼
```ruby
def hi_proc
  say_hi = Proc.new { return "hi" }
  say_hi.call
  "hello!"
end

puts hi_proc         # hi!

def hi_lambda
  say_hi = lambda { return "hi!" }
  say_hi.call
  "hello!"
end

puts hi_lambda        # hello!
```

Lambda會檢查代入的引數(Argument)數量，但 Proc 不會
```ruby
say_hi_proc = Proc.new { |first, second| puts "Hi, #{first} and #{second}" }
say_hi_lambda = lambda { |first, second| puts "Hi, #{first} and #{second}" }

say_hi_proc.call('Andy')   # Hi, Andy and
say_hi_lambda.call('Andy') # wrong number of arguments (given 1, expected 2)
```