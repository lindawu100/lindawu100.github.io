---
layout: post
title: Ruby & Kata (method - digits, permutation)
---

## [Kata] Next bigger number with the same digits
### Target
Create a function that takes a positive integer and returns the next bigger number that can be formed by rearranging its digits.
給定數字 N ，寫一個方法使用數字 N 中的數字們排列組合，回傳比數字 N 大的下一個數字。
### Example
```ruby
12 ==> 21
513 ==> 531
2017 ==> 2071
```
If the digits can't be rearranged to form a bigger number, return -1 or nil:
如果沒有下一個比較大的數字，回傳 -1 或 nil
```ruby
9 ==> -1
111 ==> -1
531 ==> -1
```
解法：將數字 N 轉成陣列使用 split, permutation, map 製造出數字 N 排列組合的結果，使用 uniq 篩掉重複的，再用 sort 排序，使用 index 找數字 N 在陣列中的排序，回傳下一個數字。
若沒有下一個數字，回傳 -1 。

以下的解法都沒出錯，但不論哪種解法，在 codewar 結果都是 time out 。

方法一：
```ruby
def next_bigger(n)
    new_arr = []
    n.to_s.split('').permutation.map { |arr| new_arr << arr.join }
    new_arr = new_arr.uniq.sort
    n = new_arr.index(n.to_s)
    if n < (new_arr.length - 1)
        res = new_arr[n+1]
    else
        res = -1
    end
    return res.to_i
end

p next_bigger(12) #21
p next_bigger(513) #531
p next_bigger(144) #414
```
方法二：
```ruby
def next_bigger(n)
    new_arr = []
    n.digits.permutation.map { |arr| new_arr << arr.join }
    new_arr = new_arr.uniq.sort
    n = new_arr.index(n.to_s)
    if n < (new_arr.length - 1)
        res = new_arr[n+1]
    else
        res = -1
    end
    return res.to_i
end

p next_bigger(12) #21
p next_bigger(513) #531
p next_bigger(144) #414
```
方法三：
```ruby
def next_bigger(n)
    new_arr = []
    n.digits.permutation.map { |arr| new_arr << arr.join }
    new_arr = new_arr.uniq.sort
    n = new_arr.index(n.to_s)
    n < (new_arr.length - 1) ? new_arr[n+1].to_i : -1
end
```
如果有比較好的解法，歡迎分享。

## Ruby method: digits (Integer)
將給定的數字，預設以 10 為基數，回傳整數的陣列。
這個方法的說明在英翻後，還是沒有很好理解。
根據手冊的範例，這個方法我理解為若指定基數，會依序回傳餘數及最後的商。

`digits → array`
預設 10 為基數，給定的數字 123 會先除以 10 ，並將餘數 3  裝在陣列中，再將商 12 繼續除以 10 ，並回傳餘數 2 ，此時商為 1 ，再除以 10 就會變成浮點數，所以就不再進行除法運算，直接將最後的商也裝入陣列回傳。
```ruby
(123).digits
# [3, 2, 1]
# 123 / 10 = 12 ... 3
# 12 / 10 = 1 ... 2
```
`digits(base) → array`
也可以指定基數，將給定的數字除以指定基數，依序回傳餘數及最後的商。
```ruby
(123).digits(2)
# [1, 1, 0, 1, 1, 1, 1]
# 123 / 2 = 61 ... 1
# 61 / 2 = 30 ... 1
# 30 / 2 = 15 ... 0
# 15 / 2 = 7 ... 1
# 7 / 2 = 3 ... 1
# 3 / 2 = 1 ... 1
```

## Ruby method: permutation (Array)
將給定的陣列排列組合後回傳。
`permutation {|p| block} → ary`
`permutation → Enumerator`
給定的陣列 [1, 2, 3] 經排列組合後的結果裝在陣列中回傳，要搭配 to_a ：
```ruby
a = [1, 2, 3]
a.permutation
# <Enumerator: [1, 2, 3]:permutation>
a.permutation.to_a
# [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]
```
與方才學過的 digits 還有之前學的 map 搭配使用：
```ruby
b = 12
b.digits
# [2, 1]
b.digits.permutation
# <Enumerator: [2, 1]:permutation>
b.digits.permutation.to_a
# [[2, 1], [1, 2]]
b.digits.permutation.map { |x| x << 3 }
# [[2, 1, 3], [1, 2, 3]]
```


`permutation(n) {|p| block} → ary`
`permutation(n) → Enumerator`
也可以指定回傳的陣列長度
```ruby
c = [1, 2]
c.permutation(1)
# <Enumerator: [1, 2]:permutation(1)>
c.permutation(1).to_a
# [[1], [2]]

d = [1, 2, 3]
# [1, 2, 3]
d.permutation(1).to_a
# [[1], [2], [3]]
d.permutation(2).to_a
# [[1, 2], [1, 3], [2, 1], [2, 3], [3, 1], [3, 2]]
```