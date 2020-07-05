---
layout: post
title: Ruby, Kata, leetcode
---

### Kata
Target: Reverse the bits in an integer

Write a function that reverses the bits in an integer.

For example, the number 417 is `110100001` in binary. Reversing the binary is `100001011` which is 267.

You can assume that the number is not negative.

Solution:

1. Using `to_s` with radix base (which can be set between 2 and 36) will return a string of the representation of integer.
2. Reverse the string.
3. Using `to_i` will return a result of interpreting the string as an integer base (between 2 and 36).

```ruby
class Integer
  def reverse
    self.to_s(2).reverse.to_i(2)
  end
end
```

### Leetcode
### 136. Single Number

Given a non-empty array of integers, every element appears twice except for one. Find that single one

Solution:
In the target array, there is only one number appears once while the other numbers appear twice.

Concept:
2*(a+b+c)-(a+a+b+b+c)=c
1. Using `uniq` to remove the duplicated values in the target array.
2. Multiply 2 to create an array with duplicated values.
3. Return the sum of the duplicated array minus the sum of origin array.

```ruby
def single_number(nums)
  2 * nums.uniq.sum - nums.sum
end
```

Others' solution:

```ruby
def single_number(nums)
  nums.reduce(:^)
end
```

### 283. Move Zeroes

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Example:
```ruby
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```
Note:

You must do this in-place without making a copy of the array.
Minimize the total number of operations.

```ruby
def move_zeroes(nums)
    x = 0
    y = 0
    while y < nums.length
        if nums[y] == 0
            y += 1
        else
            tmp = nums[y]
            nums[y] = nums[x]
            nums[x] = tmp

            x += 1
            y += 1
        end
    end
end
```