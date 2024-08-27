# Easy 5 Solutions

## Ex1

### My Solution

```ruby
def ascii_value(str)
  return 0 if str.size == 0
  values = str.chars.map { |char| char.ord }
  values.reduce(:+)
end

p ascii_value('Four score') == 984
p ascii_value('Launch School') == 1251
p ascii_value('a') == 97
p ascii_value('') == 0
```

### Official Solution

```ruby
def ascii_value(string)
  sum = 0
  string.each_char { |char| sum += char.ord }
  sum
end
```

## Ex2 (Stuck and Redo)

### My Solution

Notes:

- problem
  - input:
    - `time_in_min`: integer
  - output:
    - `res`: string of `time_in_in` in 24 hour format
  - `time_in_min`:
    - any integer
  - `res`:
    - string in format `'hh:mm'` where
      - `hh`: represents 0-padded hours
      - `mm`: represents remaining 0-padded minutes
  - cannot use STL `Date` or `Time` classes
- data structures
  - none; just use integer variables
- algorithm
  - set `DAY_IN_MINUTES` to `60 * 24`
  - set `res` to empty string
  - set `is_pre_midnight` to `minutes < 0`
  - normalize `minutes` within a 24-hour timeframe
    - set `days` to `minutes` modulus by
  - set `hrs` to whole hours from `minutes` equal to truncated integer of `minutes` divided by 60
  - set `mins` to remaining how hours from `minutes`
    - equals `minutes` modulus by 60
  - if `hrs` < 10,
    - append `'0'`-prefixed `hrs` as string to `res`
  - else
    - append `hrs` as string to `res`
  - append `':'` char to `res`
  - if `mins` < 100,
    - append `'0'`-prefixed `mins` as string to `res`
  - else
    - append `mins` as string to `res`
  - return `res`

```ruby
def time_of_day(minutes)
  # TODO
end

p time_of_day(0) == "00:00"
p time_of_day(-3) == "23:57"
p time_of_day(35) == "00:35"
p time_of_day(-1437) == "00:03"
p time_of_day(3000) == "02:00"
p time_of_day(800) == "13:20"
p time_of_day(-4231) == "01:29"
```

### Official Solution

```ruby
MINUTES_PER_HOUR = 60
HOURS_PER_DAY = 24
MINUTES_PER_DAY = HOURS_PER_DAY * MINUTES_PER_HOUR

def normalize_minutes_to_0_through_1439(minutes)
  while minutes < 0
    minutes += MINUTES_PER_DAY
  end

  minutes % MINUTES_PER_DAY
end

def time_of_day(delta_minutes)
  delta_minutes = normalize_minutes_to_0_through_1439(delta_minutes)
  hours, minutes = delta_minutes.divmod(MINUTES_PER_HOUR)
  format('%02d:%02d', hours, minutes)
end
```

## Ex3 (Stuck & Redo)

### My Solution

Notes for method `after_midnight`:

- problem
  - input
    - `time`: string
  - output
    - `res`: integer
  - `time`
    - guaranteed to be valid in 24-hour `hh:mm` timestamp format
    - always 5 chars long
    - hour part may be `'24'`
  - `res`
    - int in range \[0, 1439] inclusive
  - count minutes after midnight
    - equals hours after midnight + minutes after midnight
    - `hours_in_mins` = hour part of `time` \* 60
    - `leftover_mins` = min part of `time`
    - `mins` = `hours_in_mins` + `leftover_mins`
    - a
- examples

Notes for method `before_midnight`:

- problem

```ruby
todo
```

### Official Solution

```ruby
HOURS_PER_DAY = 24
MINUTES_PER_HOUR = 60
MINUTES_PER_DAY = HOURS_PER_DAY * MINUTES_PER_HOUR

def after_midnight(time_str)
  hours, minutes = time_str.split(':').map(&:to_i)
  (hours * MINUTES_PER_HOUR + minutes) % MINUTES_PER_DAY
end

def before_midnight(time_str)
  delta_minutes = MINUTES_PER_DAY - after_midnight(time_str)
  delta_minutes = 0 if delta_minutes == MINUTES_PER_DAY
  delta_minutes
end

p after_midnight('00:00') == 0
p before_midnight('00:00') == 0
p after_midnight('12:34') == 754
p before_midnight('12:34') == 686
p after_midnight('24:00') == 0
p before_midnight('24:00') == 0
```

## Ex4

### My Solution

Notes:

- problem
  - input:
    - `words`: a non-empty string
  - output:
    - `res`: a non-empty string
  - rules
    - `words`
      - uses space ` ` chars to delimit its words
      - will have 1+ words
      - only has word chars and spaces
      - word char = any non-space char?
    - `res`
      - has same words as in `words` but changed
      - for each word `w` in `words`,
        - swap its first (`w[0]`) and last (`w[-1]`) char
        - guaranteed to be non-empty
- examples
  - todo
- data structures
  - todo
- algorithm
  - set `words_arr` to string array of substrings of `words` using ` ` char as delimiter
  - for each string word `w` in `words_arr`,
    - set `temp` to `w[0]`
    - set `w[0]` to `w[-1]`
    - set `w[-1]` to `temp`
  - `res`: new string from joining `words_arr` with ` ` space char
  - return `res`

```ruby
def swap(words)
  words_arr = words.split(' ')
  words_arr.each do |word|
    temp = word[0]
    word[0] = word[-1]
    word[-1] = temp
  end
  words_arr.join(' ')
end

p swap('Oh what a wonderful day it is') == 'hO thaw a londerfuw yad ti si'
p swap('Abcde') == 'ebcdA'
p swap('a') == 'a'
```

### Official Solution

```ruby
def swap_first_last_characters(word)
  word[0], word[-1] = word[-1], word[0]
  word
end

def swap(words)
  results = words.split.map do |word|
    swap_first_last_characters(word)
  end
  result.join(' ')
end
```

## Ex5 (Redo)

### My Solution

Notes:

- problem
  - input:
    - `text`: a string
  - output:
    - `res`: a string
  - rules
    - `text`
      - of chars
        - from `a-z`, space char, non-alphabetic chars
      - guaranteed to be string
      - guaranteed to be non-empty?
    - `res`: a string
      - of chars from `a-z` and space chars only
      - no consecutive space chars
    - how to know if current char `c` in `text` is part of a consecutive non-alpha char string or not?
- examples
  - todo
- data structures
  - hash or set or string to track all alphabet chars
- algorithm
  - set `res` to empty string
  - set `last_char` to empty string
  - set `alpha_chars` to string `'abcdefghijklmnopqrstuvwxyz'`
  - set `i` to `0`
  - set `text_size` to count of chars in `text
  - while `i` < `text_size`
    - if `c` is in `alphachars`,
      - if `last_char` is not an alpha char,
        - append a space char ` ` to `res`
      - append `c` to `res`
    - `last_char` = `text[i]`
    - increment `i` by 1
  - if `last_char` is not an alpha char,
    - append a space char ` ` to `res`
  - return `res`

```ruby
def cleanup(text)
  res = ''
  last_char = 'a'
  text.each_char do |char|
    if alpha?(char)
      res << ' ' unless alpha?(last_char)
      res << char
    end
    last_char = char
  end
  res << ' ' unless alpha?(last_char)
  res
end

def alpha?(char)
  'a' <= char && char <= 'z'
end

p cleanup("---what's my +*& line?") == ' what s my line '
```

### Official Solution

## Ex6

Solution 1:

```ruby
ALPHABET = ('a'..'z').to_a

def cleanup(text)
  clean_chars = []

  text.chars.each do |char|
    if ALPHABET.include?(char)
      clean_chars << char
    else
      clean_chars << ' ' unless clean_chars.last == ' '
    end
  end

  clean_chars.join
end
```

Solution 2:

````ruby
def cleanup(text)
  text.gsub(/[^a-z]/, ' ').squeeze(' ')
end
```

### My Solution

Notes:

- problem:
  - input
    - `text`: potentially empty string
  - output
    - `res`: potentially empty hashmap
  - rules
    - `text`
      - like an array of words
      - word = any char string with no space chars
        - can include alpha chars, punctuation symbols, numbers, etc.
- algorithm
  - set `text_arr` to string array made from `text` by space delimiter
  - set `res` to empty hashmap
  - loop thru each el string `word` in `text_arr`
    - set `key` equal to int length of `word`
    - if `key` is not in a key `res`,
      - add it with value `0`
    - increment value of `key` in `res` by 1
  - return `res`

```ruby
def word_sizes(text)
  res = {}
  text.split(' ').each do |word|
    key = word.size
    res[key] = res.fetch(key, 0) + 1
  end
  res
end

p word_sizes('Four score and seven.') == { 3 => 1, 4 => 1, 5 => 1, 6 => 1 }
p word_sizes('Hey diddle diddle, the cat and the fiddle!') == { 3 => 5, 6 => 1, 7 => 2 }
p word_sizes("What's up doc?") == { 6 => 1, 2 => 1, 4 => 1 }
p word_sizes('') == {}
````

### Official Solution

```ruby
def word_sizes(words_string)
  counts = Hash.new(0)
  words_string.split.each do |word|
    counts[words.size] += 1
  end
  counts
end
```

## Ex7

### My Solution

Notes:

- problem:
  - input
    - `text`: potentially empty string
  - output
    - `res`: potentially empty hashmap
  - rules
    - `text`
      - like an array of words
      - word = any char string with no space chars
        - includes alpha chars (lower AND uppercase)
        - include punctuation symbols
        - if ends with non
- algorithm
  - set `text_arr` to string array made from `text` by space delimiter
  - set `res` to empty hashmap
  - loop thru each el string `word` in `text_arr`
    - set `key` to return of calling `count_alphas` with `word`
    - if `key` is not in a key `res`,
      - add it with value `0`
    - increment value of `key` in `res` by 1
  - return `res`
  - method `count_alphas(word)`
    - set `res` to `0`
    - loop thru each char `c` in `word`
      - if `c` is an alpha char,
        - increment `res` by 1
    - return `res`

```ruby
def word_sizes(text)
  res = {}
  text.split(' ').each do |word|
    key = count_alphas(word)
    res[key] = res.fetch(key, 0) + 1
  end
  res
end

def count_alphas(word)
  res = 0
  word.each_char do |c|
    res += 1 if ('a' <= c && c <= 'z') || ('A' <= c && c <= 'Z')
  end
  res
end

p word_sizes('Four score and seven.') == { 3 => 1, 4 => 1, 5 => 2 }
p word_sizes('Hey diddle diddle, the cat and the fiddle!') == { 3 => 5, 6 => 3 }
p word_sizes("What's up doc?") == { 5 => 1, 2 => 1, 3 => 1 }
p word_sizes('') == {}
```

### Official Solution

```ruby
def word_sizes(words_string)
  counts = Hash.new(0)
  words_string.split.each do |word|
    clean_word = word.delete('^A-Za-z')
    counts[clean_word.size] += 1
  end
  counts
end
```

## Ex8

### My Solution

Notes:

- problem
  - input
    - `arr`: int array of numbers
  - output
    - `res`: int array sorted based on their English words in ASC order
  - rules
    - `arr`
      - of values from range \[0, 19] inclusive only
- algorithm
  - set `INT_WORDS` array that has english words for integers 0 to 19 in ASC order
  - set `res` to new array created from calling `sort` with a certain block
  - block returns value of calling `<=>` on parameter `a` with parameter `b`

```ruby
def alphabetic_number_sort(arr)
  int_words = %w[zero, one, two, three, four, five, six, seven, eight, nine, ten, eleven, twelve, thirteen, fourteen, fifteen, sixteen, seventeen, eighteen, nineteen]
  arr.sort { |a, b| int_words[a] <=> int_words[b] }
end

p alphabetic_number_sort((0..19).to_a) == [
  8, 18, 11, 15, 5, 4, 14, 9, 19, 1, 7, 17,
  6, 16, 10, 13, 3, 12, 2, 0
]
```

### Official Solution

```ruby
NUMBER_WORDS = %w(zero one two three four
                  five six seven eight nine
                  ten eleven twelve thirteen fourteen
                  fifteen sixteen seventeen eighteen nineteen)

def alphabetic_number_sort(numbers)
  numbers.sort_by { |number| NUMBER_WORDS[number] }
end
```

## Ex9

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: string
  - `text`
    - possibly empty string
  - `res`
    - `text` with all consecutive duplicat chars removed
  - cannot use built-in `String#squeeze` or `String#squeeze!` methods
- data structures
  - array for holding WIP string
- algorithm
  - set `res` to empty array
  - set `last_char` to empty string
  - loop thru each char `c` in `text
    - if `last_char` is unlike `c`,
      - push `c` to `res`
  - return value of converting `res` to string with empty char join substring

```ruby
def crunch(text)
  last_char = ''
  res = []
  text.chars.each do |char|
    res << char if last_char != char
    last_char = char
  end
  res.join
end

p crunch('ddaaiillyy ddoouubbllee') == 'daily double'
p crunch('4444abcabccba') == '4abcabcba'
p crunch('ggggggggggggggg') == 'g'
p crunch('a') == 'a'
p crunch('') == ''
```

### Official Solution

```ruby
def crunch(text)
  index = 0
  crunch_text = ''
  while index <= text.length - 1
    crunch_text << text[index] unless text[index] == text[index + 1]
    index += 1
  end
  crunch_text
end
```

## Ex10

### My Solution

Returned string is a different object.

Method `spin_me` is called with the string `'hello world'` on line 7, which binds the string to `spin_me`'s parameter `str`. Method `each` is called on the return value of calling `split` on `str`. A `do..end` block is passed to the `each` call. The block calls `reverse!` on its parameter `word`, which represents each string element in the array returned by `split`. Since `split` returns a new array, the `reverse!` call mutates the elements in the new array which does not affect the original string referenced by `str`.

### Official Solution

The method will return a different object.

Since we are using mutating method `String#reverse!` inside of the `do..end` block, and we are also calling `each` method on the resulting array, which also returns the original array.

However, as soon as we have converted string into an array by calling `split` method on it, it is no longer possible for us to get back the original object again. Even just doing `str.split.join(" ")` returns a different object since you are splitting the string into an array and then joining that array back into a new string, with the same sequence of characters but still, a different object.

Let's also break down what happens inside of the `spin_me` method. `str.split` converts the string into array `['hello', 'world']`. When we call `each` method on this array and reverse each word inside of the array, our original array gets mutated and now it's values are `['olleh', 'dlrow']`.

So we have mutated the array that we got by splitting the string, but, when we join this array back into a string, a completely new string is returned.

You can check this by calling `Object#object_id` method.

## Ex11

### My Solution

Notes:

- problem
  - input:
    - `num`: int
  - output:
    - `res`: int array
  - `num`
    - guaranteed to be an int > 0
  - `res`
    - int array
    - has all digits of `num` in same displayed order
- data structures
  - array for storing result in `res`
- algorithm
  - set `res` to empty array
  - while `num` > 0
    - set `last_digit` to result of `num` mod 10
    - insert `last_digit` to front of `res`
    - reassign `num` to itself divided by 10
  - return `res`

```ruby
def digit_list(num)
  res = []
  while num > 0
    last_digit = num % 10
    res.unshift(last_digit)
    num /= 10
  end
  res
end

puts digit_list(12345) == [1, 2, 3, 4, 5]     # => true
puts digit_list(7) == [7]                     # => true
puts digit_list(375290) == [3, 7, 5, 2, 9, 0] # => true
puts digit_list(444) == [4, 4, 4]             # => true
```

### Official Solution

Brute force:

```ruby
def digit_list(number)
  digits = []
  loop do
    number, remainder = number.divmod(10)
    digits.unshift(remainder)
    break if number == 0
  end
  digits
end
```

Idiomatic Ruby:

```ruby
def digit_list(number)
  number.to_s.chars.map(&:to_i)
end
```
