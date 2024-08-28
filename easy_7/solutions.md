# Easy 7 Solutions

## Ex1

### My Solution

Notes:

- problem
  - input
    - `array_1`: array
    - `array_2`: array
  - output
    - `res`: array
  - `array_1` and `array_2`
    - non-empty
    - have same number of elements
  - `res`
    - has all elements from `array_1` and `array_2`
    - elements alternate in order
    - first element is first element from `array_1`
    - second element is first element from `array_2`
- algorithm
  - set `res` to empty array
  - loop for `index` from 0 to `array_1`'s length - 1
    - push `array_1[index]` to `res`
    - push `array_2[index]` to `res`
  - return `res`

```ruby
def interleave(array_1, array_2)
  res = []
  for i in range 0...array1.size
    res << array_1[index]
    res << array_2[index]
  end
  res
end

p interleave([1, 2, 3], ['a', 'b', 'c']) == [1, 'a', 2, 'b', 3, 'c']
```

### Official Solution

```ruby
def interleave(array1, array2)
  result = []
  array1.each_with_index do |element, index|
    result << element << array2[index]
  end
  result
end
```

## Ex2

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: hashmap
  - `text`
    - may be empty string
    - may have any chars
  - `res`
    - always has 3 entries
      - all entry keys are symbols
      - all entry values start at 0
- algorithm
  - set `res` to hashmap with 3 preset key-value pairs for keys
    - `:lowercase`, `:uppercase`, `:neither`
  - loop thru each char `char` in `text`,
    - if `char` is a lowercase letter,
      - increment `res[:lowercase]` by 1
    - else if `char` is an uppercase letter
      - increment `res[:uppercase]` by 1
    - else `char` is some other letter
      - increment `res[:neither]` by 1
  - return `res`

```ruby
def letter_case_count(text)
  res = { lowercase: 0, uppercase: 0, neither: 0 }
  text.each_char do |char|
    if 'a' <= char && char <= 'z'
      res[:lowercase] += 1
    elsif 'A' <= char && char <= 'Z'
      res[:uppercase] += 1
    else
      res[:neither] += 1
    end
  end
  res
end

p letter_case_count('abCdef 123') == { lowercase: 5, uppercase: 1, neither: 4 }
p letter_case_count('AbCd +Ef') == { lowercase: 3, uppercase: 3, neither: 2 }
p letter_case_count('123') == { lowercase: 0, uppercase: 0, neither: 3 }
p letter_case_count('') == { lowercase: 0, uppercase: 0, neither: 0 }
```

### Official Solution

Solution 1:

```ruby
UPPERCASE_CHARS = ('A'..'Z').to_a
LOWERCASE_CHARS = ('a'..'z').to_a

def letter_case_count(string)
  counts = { lowercase: 0, uppercase: 0, neither: 0 }

  string.chars.each do |char|
    if UPPERCASE_CHARS.include?(char)
      counts[:uppercase] += 1
    elsif LOWERCASE_CHARS.include?(char)
      counts[:lowercase] += 1
    else
      counts[:neither] += 1
    end
  end

  counts
end
```

Solution 2:

```ruby
def letter_case_count(string)
  counts = {}
  characters = string.chars
  counts[:lowercase] = characters.count { |char| char =~ /[a-z]/ }
  counts[:uppercase] = characters.count { |char| char =~ /[A-Z]/ }
  counts[:neither] = characters.count { |char| char =~ /[^A-Za-z]/ }
  counts
end
```

## Ex3

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: string
  - `res` and `text` both to different string objects
  - every word in `text` should be capitalized
    - word = any space-delimited string of chars
    - capitalized word =
      - first char is uppercased
      - every other char in word is lowercased
  - should not mutate `text`'s original string
- algorithm
  - set `words` to new array created from splitting `text` using a single space char ` ` as a delimiter
  - loop thru each element `word` in `words`
    - capitalize and mutate `word`
  - return new string created from joining `words` with single space char ` ` symbol

```ruby
def word_cap(text)
  words = text.split.map do |word|
    cap(word)
  end
  words.join(' ')
end

def cap(string)
  string[0].upcase + string[1..-1].downcase
end

p word_cap('four score and seven') == 'Four Score And Seven'
p word_cap('the javaScript language') == 'The Javascript Language'
p word_cap('this is a "quoted" word') == 'This Is A "quoted" Word'
```

### Official Solution

Solution 1:

```ruby
def word_cap(words)
  words_array = words.split.map do |word|
    word.capitalize
  end
  words_array.join(' ')
end
```

Solution 2:

```ruby
def word_cap(words)
  words.split.map(&:capitalize).join(' ')
end
```

## Ex4

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: string
  - `text`
    - may be empty
  - `res`
    - new string based on `text` where
      - all lowercase letters in `text` are uppercase in `res`
      - all uppercase letters in `text` are lowercase in `res`
  - cannot use method `String#swapcase`
- algorithm
  - set `res` to empty string
  - loop thru every char `char` in `text`
    - if `char` is lowercase char
      - push uppercase version of `char` to `res`
    - else if `char` is uppercase char
      - push lowercase version of `char` to `res`
    - else `char` is some other char
      - push `char` to `res`
  - return `res`

```ruby
def swapcase(text)
  res = ''
  text.chars.each do |char|
    if 'a' <= char && char <= 'z'
      res << char.upcase
    elsif 'A' <= char && char <= 'Z'
      res << char.downcase
    else
      res << char
    end
  end
  res
end

p swapcase('PascalCase') == 'pASCALcASE'
p swapcase('Tonight on XYZ-TV') == 'tONIGHT ON xyz-tv'
```

### Official Solution

```ruby
def swapcase(string)
  characters = string.chars.map do |char|
    if char =~ /[a-z]/
      char.upcase
    elsif char =~ /[A-z]/
      char.downcase
    else
      char
    end
  end
  characters.join
end
```

## Ex5

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: string
  - `text` and `res` are different strings
  - `text`
    - should not be mutated
    - guaranteed to be non-empty
  - `res`
    - same length as `text`
    - each alphabet char should alternate casing
    - first char is always upcased
- algorithm
  - set `res` to empty string
  - loop thru each char `c` with its index `i` in `text`
    - if `i` is even,
      - append upcased char of `c` to `res`
    - else `i` is odd
      - append downcased char of `c` to `res`
  - return `res`

```ruby
def staggered_case(text)
  res = ''
  i = 0
  for i in (0...text.size)
    if i % 2 == 0
      res << text[i].upcase
    else
      res << text[i].downcase
    end
  end
  res
end

p staggered_case('I Love Launch School!') == 'I LoVe lAuNcH ScHoOl!'
p staggered_case('ALL_CAPS') == 'AlL_CaPs'
p staggered_case('ignore 77 the 444 numbers') == 'IgNoRe 77 ThE 444 NuMbErS'
```

### Official Solution

```ruby
def staggered_case(string)
  result = ''
  need_upper = true
  string.chars.each do |char|
    if need_upper
      result += char.upcase
    else
      result += char.downcase
    end
    need_upper = !need_upper
  end
  result
end
```

## Ex6 (Redo)

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: string
  - `text` and `res` are different strings
  - `text`
    - should not be mutated
    - guaranteed to be non-empty
  - `res`
    - same length as `text`
    - each alphabet char should alternate casing
      - alternating casing order ignores non-alpha chars
    - first char is always upcased
- algorithm for `staggered_case(text)`
  - set `res` to empty string
  - set `is_uppercase` to true boolean
  - loop thru each char `c` in `text`
    - if `c` is not an alpha char,
      - push `c` to `res`
      - skip to next iteration
    - if `is_uppercase` is true,
      - append upcased char of `c` to `res`
    - else `is_uppercase` is false
      - append downcased char of `c` to `res`
    - set `is_uppercase` to its negated value
  - return `res`
- algorithm for `is_alpha(char)`
  - return true if `char` is a lowercase char
  - return true if `char` is an uppercase char
  - return false otherwise

```ruby
def staggered_case(text)
  res = ''
  is_uppercase = true
  text.each_char do |char|
    if !is_alpha(char)
      res << char
      next
    elsif is_uppercase
      res << char.upcase
    else
      res << char.downcase
    end
    is_uppercase = !is_uppercase
  end
  res
end

def is_alpha(char)
  return true if 'a' <= char && char <= 'z'
  'A' <= char && char <= 'Z'
end

p staggered_case('I Love Launch School!') == 'I lOvE lAuNcH sChOoL!'
p staggered_case('ALL CAPS') == 'AlL cApS'
p staggered_case('ignore 77 the 444 numbers') == 'IgNoRe 77 ThE 444 nUmBeRs'
```

### Official Solution

```ruby
def staggered_case(string)
  result = ''
  need_upper = true
  strings.chars.each do |char|
    if char =~ /[a-z]/i
      if need_upper
        result += char.upcase
      else
        result += char.downcase
      end
      need_upper = !need_upper
    else
      result += char
    end
  end
  result
end
```

## Ex7

### My Solution

Notes:

- problem
  - input
    - `nums`: int array
  - output / return
    - none or `nil`
  - side effects
    - prints `The result is {res}`
    - `res` is a float
  - `nums`
    - guaranteed to be non-empty and only hold ints
  - `res`
    - rounded to 3 decimal places
- algorithm
  - set `prod` to 1
  - loop thru each element `num` in `nums`
    - set `prod` to itself multiplied by `num`
  - set `res` to `prod` divided (not int div) by length of `nums`
  - set `res` to itself rounded to 3 decimal places
  - print `The result is {res}` to the console

```ruby
def show_multiplicative_average(nums)
  prod = 1.0
  for num in nums
    prod *= num
  end
  res = (prod / nums.size).round(3)
  puts format('The result is %.3f', res)
end

show_multiplicative_average([3, 5])                # => The result is 7.500
show_multiplicative_average([6])                   # => The result is 6.000
show_multiplicative_average([2, 5, 7, 11, 13, 17]) # => The result is 28361.667
```

### Official Solution

```ruby
def show_multiplicative_average(numbers)
  product = 1.to_f
  numbers.each { |number| product *= number }
  average = product / numbers.size
  puts "The result is #{format('%.3f', average)}"
end
```

## Ex8

### My Solution

Notes:

- problem
  - input
    - `array1`: int array
    - `array2`: int array
  - output
    - `res`: int array
  - `array1` and `array2`
    - may be empty
    - guaranteed to
      - be the same length
      - only have int elements
  - `res`
    - same length as `array1`
    - each element `res[i]` is the sum of `array[1]` and `array[2]`
- algorithm
  - set `res` to empty array
  - loop for `i` from 0 to length of `array` exclusive
    - push result of multiplying `array[1]` with `array[2]` to `res`
  - return `res`

```ruby
def multiply_list(array1, array2)
  res = []
  for i in (0...array1.size)
    res << array1[i] * array2[i]
  end
  res
end

p multiply_list([3, 5, 7], [9, 10, 11]) == [27, 50, 77]
```

### Official Solution

```ruby
def multiply_list(list_1, list_2)
  products = []
  list_1.each_with_index do |item, index|
    products << item * list_2[index]
  end
  products
end
```

## Ex9

### My Solution

Notes:

- problem
  - input
    - `array1`: int array
    - `array2`: int array
  - output
    - `res`: int array
  - `array1` and `array2`
    - can be of arrays of different lengths
    - guaranteed
      - to never be empty
      - to only hold ints
  - `res`
    - may have duplicate ints
    - each element is a unique product of some element in `array1` multiplied by an element in `array2`
    - sorted in ASC order
- algorithm
  - set `res` to empty array
  - loop thru each element `el1` in `array1`
    - loop thru each element `el2` in `array2`
      - push result of `el1` multiplied by `el2` to `res`
  - sort `res` in ASC order
  - return `res`

```ruby
def multiply_all_pairs(array1, array2)
  res = []
  for item1 in array1
    for item2 in array2
      res << item1 * item2
    end
  end
  res.sort
end

p multiply_all_pairs([2, 4], [4, 3, 1, 2]) == [2, 4, 4, 6, 8, 8, 12, 16]
```

### Official Solution

Solution 1:

```ruby
def multiply_all_pairs(array_1, array_2)
  products = []
  array_1.each do |item_1|
    array_2.each do |item_2|
      products << item_1 * item_2
    end
  end
  products.sort
end
```

Solution 2:

```ruby
def multiply_all_pairs(array_1, array_2)
  array_1.product(array_2).map { |num1, num2| num1 * num2 }.sort
end
```

## Ex10

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: string
  - `text`
    - guaranteed
      - to be non-empty
      - to have 2+ words
      - have words delimited by a space ` ` char
  - word: a substring in `text` of consecutive non-space chars
- data structures
  - array to hold substring words from `text`
- algorithm
  - set `words` to array of words created from `text`
  - return string element at second-last index (e.g. `-2`) of `words`

```ruby
def penultimate(text)
  words = text.split(' ')
  words[-2]
end

p penultimate('last word') == 'last'
p penultimate('Launch School is great!') == 'is'
```

### Official Solution

```ruby
def penultimate(words)
  words_array = words.split
  words_array[-2]
end
```

## Ex11

### My Solution

Notes:

- problem
  - input
    - `array`: array
  - output
    - none
  - side effects
    - print the occurences of each unique element in `array`
- data structures
  - hash that maps each unique element from `array` to its count
- algorithm
  - set `count` to empty hash that uses `0` as its default value for entries
  - loop thru each element `item` in `array`
    - if `item` is not in `count`,
      - add key-value entry (`item`, 0) to `count`
    - increment `item`'s associated value by 1 in `count`
  - loop thru each key `key` and value `val` in `count`,
    - print `{key} => {val}` to console
  - optional: return `nil`

```ruby
def count_occurrences(array)
  count = Hash.new(0)
  for item in array
    count[item] += 1
  end
  count.each do |key, val|
    puts "#{key} => #{val}"
  end
  nil
end

vehicles = [
  'car', 'car', 'truck', 'car', 'SUV', 'truck',
  'motorcycle', 'motorcycle', 'car', 'truck'
]

count_occurrences(vehicles)
```

### Official Solution

```ruby
def count_occurrences(array)
  occurrences = {}

  array.uniq.each do |element|
    occurrences[element] = array.count(element)
  end

  occurrences.each do |element, count|
    puts "#{element} => #{count}"
  end
end
```
