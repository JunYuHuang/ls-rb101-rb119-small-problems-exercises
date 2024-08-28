# Easy 6 Solutions

## Ex1 (Redo)

### My Solution

Notes:

- problem
  - input
    - `num`: float
  - output
    - `res`: string
  - `num`
    - a float in range \[0, 360]
  - `res`
    - a string in the format `'%(ddd°mm'ss")'`
      - `ddd`
        - is the degree truncated as an int
        - 1 to 3 digits long
        - no leading zeroes
      - `mm`
        - whole minutes from `num`
        - 2 digits
        - may have 1 leading zero
        - ranges from 0 to 59
      - `ss`
        - whole remaining seconds from `num`
        - 2 digits
        - may have 1 leading zero
        - ranges from 0 to 59
  - seconds in `res` may be off by +/- 2
  - 1 deg = 60 min
  - 1 min = 60 sec
- algorithm
  - set `MINUTES_PER_DEGREE` to `60`
  - set `SECONDS_PER_MINUTE` to `60`
  - set `mins` to `num` \* `MINUTES_PER_DEGREE` % `MINUTES_PER_DEGREE`
  - set `secs` to `mins` - `mins` rounded down to nearest int
  - return formatted string with `Kernel#format` call

```ruby
DEGREE = "\u00B0"
MINUTES_PER_DEGREE = 60
SECONDS_PER_MINUTE = 60

def dms(num)
  mins = (num * MINUTES_PER_DEGREE) % MINUTES_PER_DEGREE
  secs = ((mins - mins.floor) * SECONDS_PER_MINUTE).round(0)
  format('%d%s%02d\'%02d"', num.to_i, DEGREE, mins, secs)
end

# All test cases should return true
puts dms(30) == %(30°00'00")
puts dms(76.73) == %(76°43'48")
puts dms(254.6) == %(254°36'00")
puts dms(93.034773) == %(93°02'05")
puts dms(0) == %(0°00'00")
puts dms(360) == %(360°00'00") || dms(360) == %(0°00'00")
```

### Official Solution

```ruby
DEGREE = "\u00B0"
MINUTES_PER_DEGREE = 60
SECONDS_PER_MINUTE = 60
SECONDS_PER_DEGREE = MINUTES_PER_DEGREE * SECONDS_PER_MINUTE

def dms(degrees_float)
  total_seconds = (degrees_float * SECONDS_PER_DEGREE).round
  degrees, remaining_seconds = total_seconds.divmod(SECONDS_PER_DEGREE)
  minutes, seconds = remaining_seconds.divmod(SECONDS_PER_MINUTE)
  format(%(#{degrees}#{DEGREE}%02d'%02d"), minutes, seconds)
end
```

## Ex2

### My Solution

Notes:

- problem
  - input
    - `array`: array
  - output
    - `res`: array
  - `text`
    - string array
    - may be empty
  - `res`
    - array with all upper and lowercase vowel chars removed
  - words that are of all vowels should be transformed into an empty string
- algorithm
  - set `res` to empty array
  - set `vowel_regex` to one that matches all uppercase and lowercase vowel letter chars
  - loop thru each string `word` in `text`
    - set `new_word` to empty string
    - loop thru each char `c` in `word`
      - if `c` does not match `vowel_regex`
        - push `c` to `new_word`
    - push `new_word` to `res`
  - return `res`

```ruby
def remove_vowels(array)
  vowel_regex = /[aeiouAEIOU]/
  res = []
  array.each do |word|
    new_word = ''
    word.chars.each do |char|
      new_word << char unless char.match?(vowel_regex)
    end
    res << new_word
  end
  res
end

p remove_vowels(%w(abcdefghijklmnopqrstuvwxyz)) == %w(bcdfghjklmnpqrstvwxyz)
p remove_vowels(%w(green YELLOW black white)) == %w(grn YLLW blck wht)
p remove_vowels(%w(ABC AEIOU XYZ)) == ['BC', '', 'XYZ']
```

### Official Solution

```ruby
def remove_vowels(strings)
  strings.map { |string| string.delete('aeiouAEIOU') }
end
```

## Ex3 (Redo)

### My Solution

Notes:

- problem
  - input
    - `length`: integer
  - output
    - `res`: integer
  - `length`
    - int >= 2
  - `res`
    - 1-indexed int
    - first fib int that has `length` digits
  - how to get `length`'th fib number?
    - fib(1) = 1
    - fib(2) = 1
    - fib(3) = fib(1) + fib(2)
    - fib(n) = fib(n - 1) + fib(n - 2)
  - how to count a positive int's digits?
    - keep doing int div on it until it is 0
- algorithm
  - for `fib(n)`
    - return 1 if `n` <= 2
    - set `n_minus_1` to 1
    - set `n_minus_2` to 1
    - set `res` to `-1`
    - loop for `_` from 2 to `n` inclusive
      - set `res` to `n_minus_1` + `n_minus_2`
      - set `n_minus_2` to `n_minus_1`
      - set `n_minus_1` to `res`
    - return `res`
  - for `digits(num)`
    - return 1 if `num` is 0
    - set `res` to 0
    - while `res` > 0
      - increment `res` by 1
      - set `res` to itself int divided by 10
    - return `res`
  - for `find_fibonacci_index_by_length(length)`
    - set `res` to 1
    - set `fib_digits` to `digits(fib(res))`
    - while `fib_digits` < `length`
      - increment `res` by 1
      - set `fib_digits` to `digits(fib(res))`
    - return `res`

```ruby
def find_fibonacci_index_by_length(length)
  res = 1
  fib_digits = digits(fib(res))
  while fib_digits < length
    res += 1
    fib_digits = digits(fib(res))
  end
  res
end

def fib(n)
  return 1 if n <= 2
  n_minus_1 = 1
  n_minus_2 = 1
  res = -1
  for _ in 3..n
    res = n_minus_1 + n_minus_2
    n_minus_2 = n_minus_1
    n_minus_1 = res
  end
  res
end

def digits(num)
  return 1 if num == 0
  res = 0
  while num > 0
    res += 1
    num /= 10
  end
  res
end

p find_fibonacci_index_by_length(2) == 7          # 1 1 2 3 5 8 13
p find_fibonacci_index_by_length(3) == 12         # 1 1 2 3 5 8 13 21 34 55 89 144
p find_fibonacci_index_by_length(10) == 45
p find_fibonacci_index_by_length(100) == 476
p find_fibonacci_index_by_length(1000) == 4782
p find_fibonacci_index_by_length(10000) == 47847
```

### Official Solution

```ruby
def find_fibonacci_index_by_length(number_digits)
  first = 1
  second = 1
  index = 2

  loop do
    index += 1
    fibonacci = first + second
    break if fibonacci.to_s.size >= number_digits

    first = second
    second = fibonacci
  end

  index
end
```

## Ex4

### My Solution

Notes:

- problem
  - input
    - `array`: array
  - output
    - `res`: array
  - must mutate `array`
  - `res`
    - points to same array bound to `array`
    - has its elements reversed
      - e.g. first element is swapped with last element
  - cannot use built-in `Array#reverse` or `Array#reverse!` methods
- algorithm
  - set `left` to `0`
  - set `right` to length of `array` minus 1
  - while `l <= r`,
    - swap their elements
    - set `temp` to `array[l]`
    - `array[l]` = `array[r]`
    - `array[r]` = `temp`
    - increment `left` by 1
    - decrement `right` by 1
  - return `array`

```ruby
def reverse!(array)
  left = 0
  right = array.length - 1
  while left <= right
    temp = array[left]
    array[left] = array[right]
    array[right] = temp
    left += 1
    right -= 1
  end
  array
end

list = [1, 2, 3, 4]
result = reverse!(list)
p result == [4, 3, 2, 1] # true
p list == [4, 3, 2, 1] # true
p list.object_id == result.object_id # true

list = [1, 2, 3, 4, 1]
result = reverse!(list)
p result == [1, 4, 3, 2, 1] # true
p list == [1, 4, 3, 2, 1] # true
p list.object_id == result.object_id # true

list = %w(a b e d c)
p reverse!(list) == ["c", "d", "e", "b", "a"] # true
p list == ["c", "d", "e", "b", "a"] # true

list = ['abc']
p reverse!(list) == ["abc"] # true
p list == ["abc"] # true

list = []
p reverse!(list) == [] # true
p list == [] # true
```

### Official Solution

```ruby
def reverse!(array)
  left_index = 0
  right_index = -1

  while left_index < array.size / 2
    array[left_index], array[right_index] = array[right_index], array[left_index]
    left_index += 1
    right_index -= 1
  end

  array
end
```

## Ex5

### My Solution

Notes:

- problem
  - same as ex4 but
    - cannot modify the input array
    - return new array of reversed order

```ruby
def reverse(array)
  res = []
  index = array.size - 1
  while index >= 0
    res << array[index]
    index -= 1
  end
  res
end

p reverse([1, 2, 3, 4]) == [4, 3, 2, 1]         # => true
p reverse([1, 2, 3, 4, 1]) == [1, 4, 3, 2, 1]   # => true
p reverse(%w(a b e d c)) == %w(c d e b a)       # => true
p reverse(['abc']) == ['abc']                   # => true
p reverse([]) == []                             # => true

list = [1, 3, 2]                                # => [1, 3, 2]
new_list = reverse(list)                        # => [2, 3, 1]
p list.object_id != new_list.object_id          # => true
p list == [1, 3, 2]                             # => true
p new_list == [2, 3, 1]                         # => true
```

### Official Solution

```ruby
def reverse(array)
  result_array = []
  array.reverse_each { |element| result_array << element }
  result_array
end
```

## Ex6

### My Solution

Notes:

- problem
  - input
    - `array_1`: array
    - `array_2`: array
  - output
    - `res`: array
  - `array_1` and `array_2`
    - may share duplicate values
    - may be empty
  - `res`
    - array equal to all elements from `array_1` appended with all elements from `array_2`
    - no duplicate elements
- algorithm
  - set `res` array
  - add all elements from `array_1` and `array_2` to `res`
  - remove duplicate elements

```ruby
def merge(array_1, array_2)
  (array_1 + array_2).uniq
end

p merge([1, 3, 5], [3, 6, 9]) == [1, 3, 5, 6, 9]
```

### Official Solution

```ruby
def merge(array_1, array_2)
  array_1 | array_2
end
```

## Ex7 (Redo)

### My Solution

Notes:

- problem
  - input
    - `array`: array
  - output
    - `res`: 2d array
  - `array`
    - may be empty
    - may be even or odd-lengthed
  - `res`
    - always an array of 2 arrays
    - first subarray has elements from `res` from first element to middle element
    - if `array` is odd-lengthed,
      - first subarray has middle element
      - middle element is at index `array.length` int div by 2
      - second subarray has leftover elements starting at index `array.length` int div by 2 + 1
    - else `array` is even-lengthd,
      - no "real" middle element
      - "middle" element is at index `array.length / 2 - 1`
      - first subarray includes "middle" element
      - second subarray has leftover elements starting at index `array.length` int div by 2 + 1
    - second subarray has elements from `res` in order for remaining elements to last element
- algorithm
  - set `mid` to `mid` index
  - set `first_half` to empty array
  - set `second_half` to empty array
  - loop thru each element `el` with its index `i` in `array`
    - if `i` is less than or equal to `mid`,
      - push `el` to `first_half`
    - else
      - push `el` to `second_half`
  - return array `[first_half, second_half]`

```ruby
def halvsies(array)
  first_half = []
  second_half = []
  mid = array.length / 2
  mid -= 1 if array.size.even?
  for i in 0...array.size
    if i <= mid
      first_half << array[i]
    else
      second_half << array[i]
    end
  end
  [first_half, second_half]
end

p halvsies([1, 2, 3, 4]) == [[1, 2], [3, 4]] # true
p halvsies([1, 2, 3, 4, 1]) == [[1, 2, 3], [4, 1]] # true
p halvsies([1, 5, 2, 4, 3]) == [[1, 5, 2], [4, 3]] # true
p halvsies([5]) == [[5], []] # true
p halvsies([]) == [[], []] # true
```

### Official Solution

```ruby
def halvsies(array)
  middle = (array.size / 2.0).ceil
  first_half = array.slice(0, middle)
  second_half = array.slice(middle, array.size - middle)
  [first_half, second_half]
end
```

## Ex8 (Redo)

### My Solution

Notes:

- problem
  - input
    - `array`: array
  - output
    - `res`: some valid array element type e.g. integer
  - `array`
    - guaranteed
      - to be non-empty
      - have only 1 element appear twice
      - every other element appears once
  - `res`
    - single duplicate element in `array`
- algorithm
  - set `count` to empty hashmap
  - loop thru each item `item` in `array`
    - increment value of `item` by 1
    - if value of key `item` in `count` is greater than 1
      - return `item`
  - optional: return `nil`

```ruby
def find_dup(array)
  count = Hash.new(0)
  array.each do |item|
    count[item] += 1
    return item if count[item] > 1
  end
  nil
end

find_dup([1, 5, 3, 1]) == 1
find_dup([18,  9, 36, 96, 31, 19, 54, 75, 42, 15,
          38, 25, 97, 92, 46, 69, 91, 59, 53, 27,
          14, 61, 90, 81,  8, 63, 95, 99, 30, 65,
          78, 76, 48, 16, 93, 77, 52, 49, 37, 29,
          89, 10, 84,  1, 47, 68, 12, 33, 86, 60,
          41, 44, 83, 35, 94, 73, 98,  3, 64, 82,
          55, 79, 80, 21, 39, 72, 13, 50,  6, 70,
          85, 87, 51, 17, 66, 20, 28, 26,  2, 22,
          40, 23, 71, 62, 73, 32, 43, 24,  4, 56,
          7,  34, 57, 74, 45, 11, 88, 67,  5, 58]) == 73
```

### Official Solution

```ruby
def find_dup(array)
  array.find { |element| array.count(element) == 2 }
end
```

## Ex9

### My Solution

Notes:

- problem
  - input
    - `array`: array
    - `target`: any type
  - output
    - `res`: boolean
  - `array`
    - may be empty
    - can contain values of any type
  - `target`
    - any value type that may or may not be in `array`
  - `res`
    - true if `target` is in `array, else false
- algorithm
  - loop thru each item `item` in `array`
    - if `item` equals `array`,
      - return true
  - return false

```ruby
def include?(array, target)
  array.each do |item|
    return true if item == target
  end
  false
end

p include?([1,2,3,4,5], 3) == true
p include?([1,2,3,4,5], 6) == false
p include?([], 3) == false
p include?([nil], nil) == true
p include?([], nil) == false
```

### Official Solution

Easy way:

```ruby
def include?(array, value)
  !!array.find_index(value)
end
```

Harder way:

```ruby
def include?(array, value)
  array.each { |element| return true if value == element }
  false
end
```

## Ex10 (Wrong & Redo)

### My Solution

On line 1, local variable `array1` is set to an array of several strings. Local variable `array2` is set to an empty array on line 2.

On line 3, `each` is called on `array1` with a `{..}` block that takes a parameter `value` that binds to the current iterated item in the array bound to `array1`. The block appends `value` to `array2`, which is called for every element in `array1`. `array2` is now an array of the same elements in the same order as in `array1`.

On line 4, `each` is called on `array1` with a `{..}` block that takes a parameter `value` that binds to the current iterated item in `array1`. The block calls mutating method `upcase!` on `value` if `value` is a string whose first character is `'C'` or `'S'`. Any mutations caused by the `upcase!` calls only affects `array1`'s array; `array2`'s array is not affected by those calls.

On line 5, `puts` is called with `array2` which thus outputs `array2`'s element as below:

```
Moe
Larry
Curly
Shemp
Harpo
Chico
Groucho
Zeppo
```

### Official Solution

```
Moe
Larry
CURLY
SHEMP
Harpo
CHICO
Groucho
Zeppo
```

The answe lies in the fact that the first `each` loop copies a bunch of references from `array1` to `array2`. When that first loop completes, both arrays not only contain the same values, they contain the same String objects. If you modify one of those Strings, that modification will show up in both Arrays.
