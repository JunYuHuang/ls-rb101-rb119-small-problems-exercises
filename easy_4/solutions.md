# Easy 4 Solutions

## Ex1

### My Solution

P:

- input:
  - `str1`: string that may be empty
  - `str2`: string that may be empty
- output:
  - `res`: string
- needs:
  - get max-lengthed string from `str1` and `str2`
  - `str1` and `str2`
    - guaranteed to be of unlike lengths
    - guaranteed to be a string?
  - `res`: a new string of equal to concatenating the following:
    - `shorter` + `longer` + `shorter`
    - where `shorter` is the shorter string of `str1` and `str2`
    - where `longer` is the longer string of `str1` and `str2`

E:

- 'abc', 'defgh' => 'abcdefghabc'
- 'abcde', 'fgh' => 'fghabcdefgh'
- '', 'xyz' => 'xyz'

D:

- none; just use local variables

A:

- compare `str1` with `str2` to find and shorter and longer string
- assign shorter and longer string to variables `shorter` and `longer` respectively
- return the new string concatenated as follows: `shorter + longer + shorter`

```ruby
def short_long_short(str1, str2)
  shorter = ''
  longer = ''

  if str1.size > str2.size
    longer = str1
    shorter = str2
  else
    longer = str2
    shorter = str1
  end

  shorter + longer + shorter
end

p short_long_short('abc', 'defgh') == "abcdefghabc"
p short_long_short('abcde', 'fgh') == "fghabcdefgh"
p short_long_short('', 'xyz') == "xyz"
```

### Official Solution

```ruby
def short_long_short(string1, string2)
  if string1.length > string2.length
    string2 + string1 + string2
  else
    string1 + string2 + string1
  end
end
```

There are many ways to shorten the logic of the method. For example, we could use a ternary operator instead of the if/else/end construct. Further, we could use string interpolation rather than concatenation. But those are minor improvements and are not necessary. Clarity is more important than density.

There are also other algorithms to use other than the if/else logic. For example, we could put string1 and string2 in an array, and then sort the array according to the element's length. Then just concatenate the elements in the array, knowing that the shortest one is first.

Example:

```ruby
def short_long_short(string1, string2)
  arr = [string1, string2].sort_by { |el| el.length }
  arr.first + arr.last + arr.first
end
```

This method is perhaps too clever for its own good and makes it hard to understand what this method is trying to do. Though a little longer, the if/else/end solution from earlier is much clearer.

## Ex2

### My Solution (Should Redo)

Notes:

- problem
  - input:
    - `year`: an integer
  - output:
    - `res`: a string representing the century of `year`
  - `year`: a non-negative integer
  - new centuries start when `year`'s last digit is the integer `1`
  - `res`
    - ends with
      - `th` for
        - centuries that end with 0, 4-9
        - centuries whose 2nd-last digit is a 1
      - `st` for centuries ending with 1
      - `nd` for centuries ending with 2
      - `rd` for centuries ending with 3
  - century equals `year` divided by 100 with special rules
    - if `year`'s last digit is in the range \[1, 9],
      - add 1 to the century
    - else, do nothing
- examples
  - year to century
    - 0 => 0 century
    - 1 => 1 century
    - 2 => 1 century
    - 100 => 1 century
    - 101 => 2 century
    - 1901 => 20 century
  - number suffix
    - 0 => th
    - 1, 21, 31 => st
    - 2, 22, 33 => nd
    - 3, 23, 33 => rd
    - 4 to 10 => th
    - 11 to 20 => th
- data structures
  - none; just use variables to integers
- algorithm
  - determine century from `year`
    - century equals `year` / 100 with decimal truncated with no rounding
    - if `year`'s last digit > 0, add 1 to the century
  - get the right suffix ending for the century
    - get last digit from century from century modulus 10
    - return correct string suffix ending
  - return the new concatenated string equal to the century appended with the century suffix

```ruby
def century_suffix(century_count)
  century_ones_digit = century_count % 10
  century_tens_digit = (century_count / 10) % 10

  if century_tens_digit == 1
    century_count.to_s + "th"
  elsif century_ones_digit == 1
    century_count.to_s + "st"
  elsif century_ones_digit == 2
    century_count.to_s + "nd"
  elsif century_ones_digit == 3
    century_count.to_s + "rd"
  else
    century_count.to_s + "th"
  end
end

def century(year)
  century_count = year / 100
  year_ones_digit = year % 10

  if year_ones_digit > 0
    century_count += 1
  end

  century_suffix(century_count)
end
```

### Official Solution

```ruby
def century(year)
  century = year / 100 + 1
  century -= 1 if year % 100 == 0
  century.to_s + century_suffix(century)
end

def century_suffix(century)
  return 'th' if [11, 12, 13].include?(century % 100)
  last_digit = century % 10

  case last_digit
  when 1 then 'st'
  when 2 then 'nd'
  when 3 then 'rd'
  else 'th'
  end
end
```

First, notice a pattern about a century. It is equal to the current divided by 100 plus 1. The exception to this is if the year is some multiple of 100. In that case, the century is the current year divided by 100.

Next we need to understand which suffix to append for our century, the options being: 'th', 'nd', 'rd', 'st'. We decided which one to use by checking the last digit of the century. Though, before we do that, we do need to do one extra check. If the century's value mod 100 ends in either 11, 12, or 13, then we should return 'th'. Any other time, we can return a suffix determined by our case statement and the value of `century % 10`.

Finally, we combine the string representation of the century with the correctr suffix, and we have our answer.

## Ex3

### My Solution (Wrong & Redo)

Notes:

- problem
  - input:
    - `year`: integer greater than 0
  - output:
    - `res`: boolean
  - `res` is true if `year` is a leap year, else it is false
  - `year` is a leap year IFF:
    - `year` is divisible by 4 and not by 100
    - `year` is divisible by 4 and 100 and 400
  - `year` is NOT a leap year IFF:
    - `year` is not divisible by 4
    - `year` is divisible by 4 and 100
    - `year` is divisible by 4 and 100 and is not divisible by 400

```ruby
def leap_year?(year)
  if year % 4 == 0
    year % 100 == 0
  elsif year % 100 == 0
    year % 400 == 0
  else
    false
  end
end

p leap_year?(2016) == true
p leap_year?(2015) == false
p leap_year?(2100) == false
p leap_year?(2400) == true
p leap_year?(240000) == true
p leap_year?(240001) == false
p leap_year?(2000) == true
p leap_year?(1900) == false
p leap_year?(1752) == true
p leap_year?(1700) == false
p leap_year?(1) == false
p leap_year?(100) == false
p leap_year?(400) == true
```

### Official Solution

Way 1:

```ruby
def leap_year?(year)
  if year % 400 == 0
    true
  elsif year % 100 == 0
    false
  else
    year % 4 == 0
  end
end
```

Way 2:

```ruby
def leap_year?(year)
  (year % 400 == 0) || (year % 4 == 0 && year % 100 != 0)
end
```

## Ex4

### My Solution (Redo)

Notes:

- same as ex3 but
  - add guard clause at top of method
  - guard clause returns result of `year % 4 == 0` if `year` is before 1752

```ruby
def leap_year?(year)
  return year % 4 == 0 if year < 1752
  return true if year % 400 == 0
  return false if year % 100 == 0
  year % 4 == 0
end

p leap_year?(2016) == true
p leap_year?(2015) == false
p leap_year?(2100) == false
p leap_year?(2400) == true
p leap_year?(240000) == true
p leap_year?(240001) == false
p leap_year?(2000) == true
p leap_year?(1900) == false
p leap_year?(1752) == true
p leap_year?(1700) == true
p leap_year?(1) == false
p leap_year?(100) == true
p leap_year?(400) == true
```

### Official Solution

```ruby
def leap_year?(year)
  if year < 1752 && year % 4 == 0
    true
  elsif year % 400 == 0
    true
  elsif year % 100 == 0
    false
  else
    year % 4 == 0
  end
end
```

## Ex5

### My Solution

Notes:

- problem:
  - input:
    - `num`: integer greater than 1
  - output:
    - `res`: non-negative integer
  - check all numbers for `i` in the range \[1, `num`] inclusive
  - `i` is a multiple of 3 if `i` modulus 3 is 0
  - `i` is a multiple of 5 if `i` modulus 5 is 0
  - `i` is a multiple of 3 if `i` modulus 3 is 0 and `i` modulus 5 is 0
  - a multiple should be unique and be added to the sum once
  - add up all multiples to get the sum
- examples
  - 10 -> 33
    - multiples: 3, 5, 6, 9, 10
- data structures
  - none; just need an integer variable `res` to track sum result
- algorithm
  - set variable `res` to `0`
  - loop for `i` from `1` to `n` inclusive,
    - if `i` is divisible by `3` or `5`,
      - increment `res` by `i`
  - return `res`

```ruby
def multisum(num)
  res = 0
  for i in 1..num
    res += i if i % 3 == 0 || i % 5 == 0
  end
  res
end

p multisum(3) == 3
p multisum(5) == 8
p multisum(10) == 33
p multisum(1000) == 234168
```

### Official Solution

```ruby
def multiple?(number, divisor)
  number % divisor == 0
end

def multisum(max_value)
  sum = 0
  1.upto(max_value) do |number|
    if multiple?(number, 3) || multiple?(number, 5)
      sum += number
    end
  end
  sum
end
```

Our solution begins with a `multiple?` method that returns `true` if the given `number` is an exact multiple of `divisor`, `false` if it's not. This method isn't necessary, but it makes the `multisum` a bit more readable.

`multisum` does nothing fancy; it rushes headlong into the problem, and comes out the other end with the correct result. It adds each value that is a multiple of 3 or 5 in the range to the `sum` variable.

Investigate Enumerable.reduce. How might this method be useful in solving this problem? (Note that Enumerable methods are available when working with Arrays.) Try writing such a solution. Which is clearer? Which is more succinct?

## Ex6

### My Solution

Notes:

- problem:
  - input:
    - `arr`: array of integers
  - output:
    - `res`: array of integers
  - `arr`
    - guaranteed to be of only integers or empty
  - running total
    - sum of all element integers from start of `arr` to and including the current element `arr[i]`
- examples
  - [2, 5, 13] => [2, 7, 20]
- data structures
  - create new array for `res`
  - local integer variable `total` set to 0 to track of running total
- algorithm
  - create empty `res` array
  - initialise `total` variable to `0`
  - loop from each element `el` in `arr`
    - increment `total` by `el`
    - push `total` to `res`
  - return `res` array

```ruby
def running_total(arr)
  res = []
  total = 0
  for el in arr
    total += el
    res << total
  end
  res
end

p running_total([2, 5, 13]) == [2, 7, 20]
p running_total([14, 11, 7, 15, 20]) == [14, 25, 32, 47, 67]
p running_total([3]) == [3]
p running_total([]) == []
```

### Official Solution

```ruby
def running_total(array)
  sum = 0
  array.map { |value| sum += value }
end
```

This solution does nothing fancy; it just walks through the array calculating the running total while building the resulting array. `#map` makes this really easy.

Try solving this problem using Enumerable#each_with_object or Enumerable#reduce (note that Enumerable methods can be applied to Arrays).

## Ex7

### My Solution

Notes:

- problem
  - input:
    - `str`: a valid string representation of an integer
  - output:
    - `res`: an integer converted from `str`
  - `str`
    - guaranteed to contains chars from `0-9` only
  - `res`
    - should be the same return value of calling `String#to_i` on `str`
  - cannot use `String#to_i` or `Integer()` methods
- data structures
  - just use local integer variable
- algorithm
  - initialise variable `res` set to `0`
  - create hashmap `charToInt` that maps all digit string chars to their equivalent integers
  - loop thru `str` in order with index / counter `i`
    - `i` starts from first index `0`
    - `i` ends at last index equal to length of `str` minus 1
    - get integer `digit` from character / substring `str[i]`
      - value of `charToInt[str[i]]`
    - set `res` to result of multiplying itself by 10
    - increment `res` by `digit`
  - return `res`

```ruby
def string_to_integer(str)
  charToInt = {
    '0' => 0, '1' => 1, '2' => 2, '3' => 3, '4' => 4,
    '5' => 5, '6' => 6, '7' => 7, '8' => 8, '9' => 9
  }
  res = 0
  i = 0

  while i < str.size
    digit = charToInt[str[i]]
    res *= 10
    res += digit
    i += 1
  end

  res
end

p string_to_integer('4321') == 4321
p string_to_integer('570') == 570
```

### Official Solution

```ruby
DIGITS = {
  '0' => 0, '1' => 1, '2' => 2, '3' => 3, '4' => 4,
  '5' => 5, '6' => 6, '7' => 7, '8' => 8, '9' => 9
}

def string_to_integer(string)
  digits = string.chars.map { |char| DIGITS[char] }

  value = 0
  digits.each { |digit| value = 10 * value + digit }
  value
end
```

As usual, this isn't the shortest or even the easiest solution to this problem, but it's straightforward. The big takeaway from this solution is our use of the DIGITS hash to convert string digits to their numeric values. This technique of using hashes to perform conversions is a common idiom that you can use in a wide variety of situations, often resulting in code that is easier to read, understand, and maintain.

The actual computation of the numeric value of `string` is mechanical. We take each digit and add it to 10 times the previous value, which yields the desired result in almost no time. For example, if we have 4, 3, and 1, we compute the result as:

```
10 * 0 + 4 -> 4
10 * 4 + 3 -> 43
10 * 43 + 1 -> 431
```

Write a hexadecimal_to_integer method that converts a string representing a hexadecimal number to its integer value.

Example:

```ruby
hexadecimal_to_integer('4D9f') == 19871
```

## Ex8

### My Solution

```ruby
def string_to_signed_integer(str)
  charToInt = {
    '0' => 0, '1' => 1, '2' => 2, '3' => 3, '4' => 4,
    '5' => 5, '6' => 6, '7' => 7, '8' => 8, '9' => 9
  }
  res = 0
  i = 0
  sign = '+'

  if str[0] == '+'
    sign = '+'
    i += 1
  elsif str[0] == '-'
    sign = '-'
    i += 1
  end

  while i < str.size
    digit = charToInt[str[i]]
    res *= 10
    res += digit
    i += 1
  end

  sign == '+' ? res : -res
end

p string_to_signed_integer('4321') == 4321
p string_to_signed_integer('-570') == -570
p string_to_signed_integer('+100') = 100
```

### Official Solution

```ruby
def string_to_signed_integer(string)
  case string[0]
  when '-' then -string_to_integer(string[1..-1])
  when '+' then string_to_integer(string[1..-1])
  else          string_to_integer(string)
  end
end
```

In this solution, we opt to reuse the `string_to_integer` method from the previous exercise. Why waste effort reinventing the wheel? (Oh, wait. That's exactly what we're doing, isn't it?)

This solution is reasonably easy: it simply looks at the first character of `string`. If the character is a `-`, the negative of the number represented by the rest of the string is returned. If it is not a `-`, it returns the value of the rest of the string as a number, skipping over a leading `+` if present.

Note that we rely on the fact that `case` returns the value returned by the `when` (or `else`) branch selected.

The most interesting aspect of this method is the use of `string[1..-1]` to obtain the remainder of the string after a leading `+` or `-`. This notation simply means to extract the characters starting at index 1 of the string (the second character) and ending with the last character (index -1).

In our solution, we call `string[1..-1]` twice, and call `string_to_integer` three times. This is somewhat repetitive. Refactor our solution so it only makes these two calls once each.

## Ex9

### My Solution

Notes:

- problem
  - input:
    - `num`: integer greater than 0 or 0
  - output:
    - `res`: string that represents `num`
  - cannot use Ruby's STL standard conversion methods
- examples
  - 4321 => '4321'
    - 4321 % 10 = 1
    - 4321 / 10 = 432
- data structures
  - use variables to store integers for last digit and current number
- algorithm
  - if `num` is 0,
    - return '0'
  - set `digitToChar` hashmap that maps each integer 0 to 9 to their string / char'
  - set `res` to an empty string
  - loop while `num` > 0
    - set `digit` equal to `num` % 10
    - append `digitToChar[digit]` to `res`
    - set `num` to `num` divided by 10 (integer division)
  - return reversed version of `res`

```ruby
def integer_to_string(num)
  return '0' if num == 0

  digitToChar = {
    0 => '0', 1 => '1', 2 => '2', 3 => '3', 4 => '4',
    5 => '5', 6 => '6', 7 => '7', 8 => '8', 9 => '9'
  }
  res = ''
  while num > 0
    digit = num % 10
    res << digitToChar[digit]
    num /= 10
  end
  res.reverse
end

p integer_to_string(4321) == '4321'
p integer_to_string(0) == '0'
p integer_to_string(5000) == '5000'
```

### Official Solution

```ruby
DIGITS = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']

def integer_to_string(number)
  result = ''
  loop do
    number, remainder = number.divmod(10)
    result.prepend(DIGITS[remainder])
    break if number == 0
  end
  result
end
```

## Ex10

### My Solution

```ruby
def integer_to_string(num)
  return '0' if num == 0

  digitToChar = {
    0 => '0', 1 => '1', 2 => '2', 3 => '3', 4 => '4',
    5 => '5', 6 => '6', 7 => '7', 8 => '8', 9 => '9'
  }
  res = ''
  while num > 0
    digit = num % 10
    res.prepend(digitToChar[digit])
    num /= 10
  end
  res
end

def signed_integer_to_string(num)
  return integer_to_string(num) if num == 0
  num < 0 ?
    '-' + integer_to_string(-num) :
    '+' + integer_to_string(num)
end

p signed_integer_to_string(4321) == '+4321'
p signed_integer_to_string(-123) == '-123'
p signed_integer_to_string(0) == '0'
```

### Official Solution

```ruby
def signed_integer_to_string(number)
  case number <=> 0
  when -1 then "-#{integer_to_string(-number)}"
  when +1 then "+#{integer_to_string(number)}"
  else          integer_to_string(number)
  end
end
```
