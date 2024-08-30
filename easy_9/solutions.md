# Easy 9 Solutions

## Ex1

### My Solution

Notes:

- problem
  - input
    - `name`: string array
    - `role`: hash
  - output
    - `res`: string
  - `name`
    - has 2+ string elements
  - `role`
    - has 2 symbol keys each mapped to a string
- data structures
  - none
- algorithm
  - set `name_str` to string created from joining all elements in `name` with a ` ` space char delimiter
  - set `role_str` to string created from concatenating
    - the value of `:title` in `role`
    - a ` ` space char
    - the value of `:occupation` in `role`
  - return a interpolated string that uses `name_str` and `role_str` in the following form
    - `'Hello, {name_str}! Nice to have a {role_str} around.'`

```ruby
def greetings(name, role)
  name_str = name.join(' ')
  role_str = role[:title] + ' ' + role[:occupation]
  "Hello, #{name_str}! Nice to have a #{role_str} around."
end

p greetings(['John', 'Q', 'Doe'], { title: 'Master', occupation: 'Plumber' }) == "Hello, John Q Doe! Nice to have a Master Plumber around."
# => "Hello, John Q Doe! Nice to have a Master Plumber around."
```

### Official Solution

```ruby
def greetings(name, status)
  "Hello, #{name.join(' ' )}! Nice to have a #{status[:title]} #{status[:occupation]} around."
end
```

We have two variables here, an Array and a Hash. We can use `Array#join` on the Array, and supply it with a `' '` to change the Array into a full name with appropriate spacing. For the Hash, we simply access the items by their keys.

Finally we use string interpolation to combine everything into a single string and allow the method to return it automatically.

## Ex2

### My Solution

Notes:

- problem
  - input
    - `number`: integer
  - output
    - `res`: integer
  - `number`
    - assume to be non-negative
  - `res`
    - if `number` is a double number,
      - `res` is `number`
    - else,
      - `res` is `number` times 2
  - double number `n` meaning
    - must have even number of digits
    - when visually split in half,
      - its left side's digits must be the same as its right side's digits
- data structures
  - string
- algorithm for `twice(number)`
  - if `number` is double number,
    - return `number`
  - else,
    - return `number` times 2
- algorithm for `double_number?(number)`
  - set `digits_count` to count of digits in `number`
  - if `digits_count` is p odd,
    - return false
  - set `mid` to `digits_count` divided by 2
  - set `number_str` to `number` converted to a string
  - set `left` to substring of `number_str` from index 0 to `mid` - 1
  - set `right` to substring of `number_str` from index `mid` to `number_str`'s last index
  - return true if `left` is the same string as `right`
  - else return false

```ruby
def twice(number)
  double_number?(number) ? number : number * 2
end

def double_number?(number)
  number_str = number.to_s
  digits_count = number_str.size
  return false if digits_count % 2 != 0

  mid = digits_count / 2
  left = number_str[0..mid - 1]
  right = number_str[mid..-1]
  left == right
end

p twice(37) == 74
p twice(44) == 44
p twice(334433) == 668866
p twice(444) == 888
p twice(107) == 214
p twice(103103) == 103103
p twice(3333) == 3333
p twice(7676) == 7676
p twice(123_456_789_123_456_789) == 123_456_789_123_456_789
p twice(5) == 10
```

### Official Solution

```ruby
def twice(number)
  string_number = number.to_s
  center = string_number.size / 2
  left_side = center.zero? ? '' : string_number[0..center - 1]
  right_side = string_number[center..-1]

  return number if left_side == right_side
  return number * 2
end
```

First turn the number into a string, then find the center of the string. Next, create a variable for the right and left sides. Finally compare and return based upon that.

Note that there is an edge case for single digit numbers; in this case, `center` is calculated as 0. If we don't pay attention to this case, we end up setting both `left_side` and `right_side` to `string_number[0..-1]`, which, in the case of a single character string, is equal to that character.

## Ex3

### My Solution

Notes:

- problem
  - input
    - `num`: integer
  - output
    - `res`: integer array
  - `num`
    - guaranteed to be a positive integer (excludes 0)
  - `res`
    - array of all integers from 1 to `num` inclusive
    - in ASC order
- data structures
  - array to hold `res`
- algorithm
  - set `res` to empty array
  - set `i` to 1
  - while `i` <= `num`,
    - push `i` to `res`
    - increment `i` by 1
  - return `res`

```ruby
def sequence(num)
  res = []
  i = 1
  while i <= num
    res.push(i)
    i += 1
  end
  res
end

p sequence(5) == [1, 2, 3, 4, 5]
p sequence(3) == [1, 2, 3]
p sequence(1) == [1]
```

### Official Solution

```ruby
def sequence(number)
  (1..number).to_a
end
```

This simply takes advantage of `Range` combined with `to_a` which creates a range from `1` up to the value of the supplied parameter `number` and then converts it to an array.

## Ex4

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: string array
  - `text`
    - may be empty
    - has 0+ words each separated by 1 space char
  - word meaning
    - any substring in `text` that is a series of non-space chars
  - `res`
    - each element `el`
      - is a string concatenated of
        - a word from `text` AND
        - a ` ` space char AND
        - the word's length as a number
- data structures
  - array to hold `res`
- algorithm
  - set `words` to an array created from `text` using the ` ` space char as a delimiter
  - set `res` to an empty array
  - for each element `word` in `words,
    - set `sub` to a string concatenated of
      - `word` and a ` ` space char and `word`'s length
    - push `sub` to `res`
  - return `res`

```ruby
def word_lengths(text)
  text.split.map { |word| word + ' ' + word.size.to_s }
end

p word_lengths("cow sheep chicken") == ["cow 3", "sheep 5", "chicken 7"]

p word_lengths("baseball hot dogs and apple pie") ==
  ["baseball 8", "hot 3", "dogs 4", "and 3", "apple 5", "pie 3"]

p word_lengths("It ain't easy, is it?") == ["It 2", "ain't 5", "easy, 5", "is 2", "it? 3"]

p word_lengths("Supercalifragilisticexpialidocious") ==
  ["Supercalifragilisticexpialidocious 34"]

p word_lengths("") == []
```

### Official Solution

Solution 1:

```ruby
def word_lengths(string)
  words = string.split

  words.map do |word|
    word + ' ' + word.length.to_s
  end
end
```

Solution 2:

```ruby
def word_lengths(string)
  string.split.map { |word| "#{word} #{word.length}" }
end
```

## Ex5 (Redo)

### My Solution

Notes:

- problem
  - input
    - `name`: string
  - output
    - `res`: string
  - `name`
    - non-empty string
    - has a space char that splits `name`'s 2 substrings
      - substring before space is the first name
      - substring after space is the last name
  - `res`
    - new string that returns a string that changes the first and last name substrings in `name` in the form
      - `{last_name}, {first_name}`
- data structures
  - array to store `name` as an array
- algorithm
  - create array from splitting `text` using the ` ` space char
  - reverse the array
  - create string that joins the array with the substring `, `
  - return the string

```ruby
def swap_name(name)
  name.split.reverse.join(', ')
end

p swap_name('Joe Roberts') == 'Roberts, Joe'
```

### Official Solution

```ruby
def swap_name(name)
  name.split.reverse.join(', ')
end
```

## Ex6

### My Solution

Notes:

- problem
  - input
    - `count`: integer
    - `first`: integer
  - output
    - `res`: array
  - `count`
    - 0 or a positive integer
  - `first`
    - any integer
  - `res`
    - empty array if `count` is 0
    - if `count` is 1 or more,
      - has `count` number of elements
      - has `count` multiples of `first`
      - in ASC order
- data structures
  - array to hold `res`
- algorithm
  - optional: if `count` is 0, return empty array
  - set `res` to empty array
  - loop for `multiplier` from 1 to `count` inclusive
    - push the product of `first` and `multiplier` to `res`
  - return `res`

```ruby
def sequence(count, first)
  return [] if count == 0
  res = []
  for multiplier in (1..count)
    res.push(first * multiplier)
  end
  res
end

p sequence(5, 1) == [1, 2, 3, 4, 5]
p sequence(4, -7) == [-7, -14, -21, -28]
p sequence(3, 0) == [0, 0, 0]
p sequence(0, 1000000) == []
```

### Official Solution

Solution 1:

```ruby
def sequence(count, first)
  sequence = []
  number = first

  count.times do
    sequence << number
    number += first
  end

  sequence
end
```

Solution 2:

```ruby
def sequence(count, first)
  (1..count).map { |value| value * first }
end
```

## Ex7

### My Solution

Notes:

- problem
  - input
    - `grade1`: integer
    - `grade2`: integer
    - `grade3`: integer
  - output
    - `res`: string
  - all inputs
    - are integers in the range \[0, 100] inclusive
  - `res`
    - grade letter of the average of all inputs
    - is average the normal or integer division of the inputs?
- data structures
  - none
- algorithm for `get_grade(grade1, grade2, grade3)`
  - set `average` to sum of `grade1`, `grade2`, `grade3`
  - set `average` to itself divided by 3.0
  - return `'F'` if average < 60
  - return `'D'` if average < 70
  - return `'C'` if average < 80
  - return `'B'` if average < 90
  - return `'A'`

```ruby
def get_grade(grade1, grade2, grade3)
  average = (grade1 + grade2 + grade3) / 3.0
  return 'F' if average < 60
  return 'D' if average < 70
  return 'C' if average < 80
  return 'B' if average < 90
  'A'
end

p get_grade(95, 90, 93) == "A"
p get_grade(50, 50, 95) == "D"
```

### Official Solution

```ruby
def get_grade(s1, s2, s3)
  result = (s1 + s2 + s3) / 3

  case result
  when 90..100 then 'A'
  when 80..89 then 'B'
  when 70..79 then 'C'
  when 60..69 then 'D'
  else              'F'
  end
end
```

## Ex8

### My Solution

Notes:

- problem
  - input
    - `list`: array
  - output
    - `res`: array
  - `list`
    - 2D array of arrays
    - each subarray `sub`
      - has 2 elements
      - first element is a string
      - second element is a non-negative integer
  - `res`
    - string array
- data structures
  - array to hold `res`
- algorithm
  - set `res` to empty array
  - loop thru each subarray `item` in `list`,
    - set `fruit` to `item`'s first element
    - set `quantity` to `item`'s second element
    - loop for `quantity` times,
      - push `fruit` to `res`
  - return `res`

```ruby
def buy_fruit(list)
  res = []
  list.each do |fruit, quantity|
    quantity.times { res.push(fruit) }
  end
  res
end

p buy_fruit([["apples", 3], ["orange", 1], ["bananas", 2]]) ==
  ["apples", "apples", "apples", "orange", "bananas","bananas"]
```

### Official Solution

Solution 1:

```ruby
def buy_fruit(list)
  expanded_list = []

  list.each do |item|
    fruit, quantity = item[0], item[1]
    quantity.times { expanded_list << fruit }
  end

  expanded_list
end
```

Solution 2:

```ruby
def buy_fruit(list)
  list.map { |fruit, quantity| [fruit] * quantity }.flatten
end
```

## Ex9 (Redo)

### My Solution

Notes:

- problem
  - input
    - `words`: string array
  - output
    - none
  - side effects
    - prints out `res`
  - `res`
    - array of string subarrays
    - each string subarray `sub`
      - is sorted in ASC alphabetical order
    - each element `word` in a string subarray
      - is an anagram of the same letters and count
  - words `w1` and `w2` are anagrams IFF
    - they are of equal length
    - they have the same unique characters
    - they have the same count of each unique characters
- data structure
  - arrays to hold `res` and its subarrays
  - hash that maps each word in `words`'s unique chars to their count
  - hash that maps each subhash to an array of words of the same anagram
- algorithm for `print_anagrams(words)`
  - set `res` to empty array
  - set `countsToWords` to empty hash
  - loop thru each element `word` in `words`
    - get hash of `word` from calling `map_chars` with `word`
    - if hash is not a key in `countsToWords`,
      - add key-value entry (the hash, an empty array) to `countsToWords`
    - push `word` to the array value associated with the hash key in `countsToWords`
  - loop thru every value `anagrams` in `countsToWords`
    - print `anagrams` sorted in ASC order on its own line
  - optional: return `nil`
- algorithm for `map_chars(word)`
  - set `res` to empty hash
  - loop thru each char `char` in `word`,
    - if `char` is not a key in `res`,
      - add the `(char, 0)` key-value entry to `res`
    - increment the value of the key `char` in `res` by 1
  - return `res`

```ruby
def print_anagrams(words)
  res = []
  countsToWords = {}
  words.each do |word|
    char_counts = map_chars(word)
    unless countsToWords[char_counts]
      countsToWords[char_counts] = []
    end
    countsToWords[char_counts].push(word)
  end

  countsToWords.values.each do |anagrams|
    print "#{anagrams.sort}\n"
  end
end

def map_chars(word)
  res = Hash.new(0)
  word.each_char do |char|
    res[char] += 1
  end
  res
end

words =  ['demo', 'none', 'tied', 'evil', 'dome', 'mode', 'live',
          'fowl', 'veil', 'wolf', 'diet', 'vile', 'edit', 'tide',
          'flow', 'neon']

print_anagrams(words)
```

### Official Solution

```ruby
result = {}

words.each do |word|
  key = word.split('').sort.join
  if result.has_key?(key)
    result[key].push(word)
  else
    result[key] = [word]
  end
end

result.each_value do |v|
  puts "------"
  p v
end
```

## Ex10

### My Solution

Notes:

- problem
  - input
    - `num`: integer
  - output
    - `res`: integer
  - `num`
    - integer > 0
  - `res`
    - sum of all digits in `num`
  - challenge
    - write without any looping constructs
- data structures
  - none
- algorithm
  - set `res` to 0
  - while `num` is greater than 0,
    - set `last_digit` to `num` modulus 10
    - add `last_digit` to `res`
    - set `num` to itself integer divided by 10
  - return `res`

```ruby
def sum(num)
  res = 0
  while num > 0
    last_digit = num % 10
    res += last_digit
    num /= 10
  end
  res
end

puts sum(23) == 5
puts sum(496) == 19
puts sum(123_456_789) == 45
```

### Official Solution

Solution 1:

```ruby
def sum(number)
  sum = 0
  str_digits = number.to_s.chars

  str_digits.each do |str_digit|
    sum += str_digit.to_i
  end

  sum
end
```

Solution 2:

```ruby
def sum(number)
  number.to_s.chars.map(&:to_i).reduce(:+)
end
```

## Ex11

### My Solution

Notes:

- problem
  - input
    - `list`: array
  - output
    - `res`: array
  - `list`
    - may be empty
  - `res`
    - is empty if `list` is empty
    - has every even 0-indexed element in `list` in order
- data structures
  - array to hold `res`
- algorithm
  - set `res` to empty array
  - set `must_include` to boolean true
  - loop thru each element `item` in `list`,
    - if `must_include` is true,
      - push `item` to `list`
    - set `must_include` to the negation of itself
  - return `res`

```ruby
def oddities(list)
  res = []
  must_include = true
  list.each do |item|
    res.push(item) if must_include
    must_include = !must_include
  end
  res
end

p oddities([2, 3, 4, 5, 6]) == [2, 4, 6]
p oddities([1, 2, 3, 4, 5, 6]) == [1, 3, 5]
p oddities(['abc', 'def']) == ['abc']
p oddities([123]) == [123]
p oddities([]) == []
p oddities([1, 2, 3, 4, 1]) == [1, 3, 1]
```

### Official Solution

```ruby
def oddities(array)
  odd_elements = []
  index = 0
  while index < array.size
    odd_elements << array[index]
    index += 2
  end
  odd_elements
end
```
