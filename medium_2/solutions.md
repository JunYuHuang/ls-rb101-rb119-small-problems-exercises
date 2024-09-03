# Medium 2 Solutions

## Ex1 (Redo)

### My Solution

Notes

- problem
  - input
    - `file_name`: string
  - output
    - none
  - side effects
    - prints the longest sentence in `file_name`
    - prints the count of words in the longest sentence in `file_name`
  - `file_name`
    - name of text file in local directory to read
    - assume it exists
  - sentence meaning
    - series of space-delimited words that ends with a `.`, `!`, or `?` char
  - word meaning
    - consecutive series of chars that are NOT
      - space chars
      - `.`, `!`, or `?` chars
    - can end with a punctuation char e.g. `,` `:`, `;`, `.`, `!`?
    - includes double dashes i.e. `--`
- data structures
  - arrays
- algorithm
  - set `file_name` from first command-line argument
  - return if `file_name` is not a file that exists in the local directory
  - set `text` to the reading of all bytes in `file_name`
  - set `sentences` to a string array created from `text` using 3 delimiters `.`, `!`, and `?` chars
  - set `longest_sentence` to the element in `sentences` with the longest length of chars
  - set `word_count` to the length of the string array created from `longest_sentence` using the ` ` space char delimiter
  - print `longest_sentence` to the console
  - print `word_count` to the console

```ruby
file_name = ARGF.argv[0]
unless File.exist?(file_name)
  puts "Error: File '#{file_name}' does not exist"
  return
end

file = File.open(file_name)
text = file.read
file.close
sentences = text.split(/\.|\?|!/)
longest_sentence = sentences.max { |a, b| a.size <=> b.size }
longest_sentence_word_count = longest_sentence.split.size
puts ">> Longest sentence in '#{file_name}':"
puts longest_sentence
puts ">> Its word count: #{longest_sentence_word_count}"
```

### Official Solution

```ruby
text = File.read('sample_text.txt')
sentences = text.split(/\.|\?|!/)
largest_sentence = sentences.max_by { |sentence| sentence.split.size }
largest_sentence = largest_sentence.strip
number_of_words = largest_sentence.split.size

puts "#{largest_sentence}"
puts "Containing #{number_of_words} words"
```

## Ex2 (Redo)

### My Solution

Notes:

- problem
  - input
    - `word`: string
  - output
    - `res`: boolean
  - `word`
    - treat as case-insensitive
  - `res`
    - true if `word` can be spelled with the spelling block set
    - false otherwise
      - `word` has 13+ chars long
      - `word` has any duplicate letters
  - spelling block `block` meaning
    - a 3-char string of 2 uppercase letters delimited by a `:` colon
    - a `block` is used IFF
      - either of its chars is used
- data structures
  - hashmap
- algorithm
  - set `letter_pairs` to a hashmap that does 2-way mappings for each letter in the spelling block set
  - loop thru each char `letter` in the upcased version of `word`,
    - if `letter` is not a key in `letter_pairs`,
      - return false
    - set `paired_letter` to value of key `letter` in `letter_pairs`
    - delete entry with key `paired_letter` in `letter_pairs`
    - delete entry with key `letter` in `letter_pairs`
  - return true

```ruby
def block_word?(word)
  letter_pairs = {
    'B' => 'O', 'O' => 'B', 'X' => 'K', 'K' => 'X',
    'D' => 'Q', 'Q' => 'D', 'C' => 'P', 'P' => 'C',
    'N' => 'A', 'A' => 'N', 'G' => 'T', 'T' => 'G',
    'R' => 'E', 'E' => 'R', 'F' => 'S', 'S' => 'F',
    'J' => 'W', 'W' => 'J', 'H' => 'U', 'U' => 'H',
    'V' => 'I', 'I' => 'V', 'L' => 'Y', 'Y' => 'L',
    'Z' => 'M', 'M' => 'Z'
  }
  word.upcase.chars do |char|
    return false unless letter_pairs[char]
    paired_letter = letter_pairs[char]
    letter_pairs.delete(paired_letter)
    letter_pairs.delete(char)
  end
  true
end

p block_word?('BATCH') == true
p block_word?('BUTCH') == false
p block_word?('jest') == true
p block_word?('apples') == false
p block_word?('Baby') == false
```

### Official Solution

```ruby
BLOCKS = %w(BO XK DQ CP NA GT RE FS JW HU VI LY ZM).freeze

def block_word?(string)
  up_string = string.upcase
  BLOCKS.none? { |block| upcase-count(block) >= 2 }
end
```

## Ex3 (Sloppy & Redo)

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: hash
  - `text`
    - non-empty string
  - `res`
    - hash with 3 entries
      - each value of each entry is a float that represents the percentage of a char group / type
      - 3 char group types: lowercase, uppercase, neither
- data structures
  - hash
- algorithm
  - set `res` to has with 3 entries that each have a value of 0
  - loop thru each char `char` in `text`
    - increment the value of the key that `char` is classified under
  - loop thru each entry `key`, `value` in `res`,
    - set `percent` to `count`'s value divided by `text`'s length converted to a float multiplied by 100
    - set `res[key]` to `percent`'s value
  - return `res`

```ruby
def letter_percentages(text)
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
  char_count = text.size.to_f
  res.each do |key, value|
    res[key] = value / char_count * 100
  end
  res
end

p letter_percentages('abCdef 123') == { lowercase: 50.0, uppercase: 10.0, neither: 40.0 }
p letter_percentages('AbCd +Ef') == { lowercase: 37.5, uppercase: 37.5, neither: 25.0 }
p letter_percentages('123') == { lowercase: 0.0, uppercase: 0.0, neither: 100.0 }
```

### Official Solution

```ruby
def letter_percentages(string)
  counts = {}
  percentages = {}
  characters = string.chars
  length = string.length

  counts[:lowercase] = characters.count { |char| char =~ /[a-z]/ }
  counts[:uppercase] = characters.count { |char| char =~ /[A-Z]/ }
  counts[:neither] = characters.count{ |char| char =~ /[^A-Za-z]/ }

  calculate(percentages, counts, length)

  percentages
end

def calculate(percentages, counts, lengths)
  percentages[:lowercase] = (counts[:lowercase] / length.to_f) * 100
  percentages[:uppercase] = (counts[:uppercase] / length.to_f) * 100
  percentages[:neither] = (counts[:neither] / length.to_f) * 100
end
```

## Ex4 (Redo)

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: boolean
  - `text`
    - assumed non-empty string
    - may have left and right parentheses chars
  - `res`
    - true if `text` has balanced parentheses
    - false otherwise
  - `text` has balanced parentheses IFF:
    - same number of both left and right parentheses
      - either can be 0
    - first parentheses char in `text` must be an opening / left parentheses
    - every left parentheses char must be closed by a right parentheses char
    - every right parentheses char must be opened by a left parentheses char
- data structures
  - array / stack to track parentheses
- algorithm
  - set `left_par_count` to 0
  - set `left_par_stack` to an empty array
  - loop thru each char `char` in `text`,
    - if `char` is not a parenthese char,
      - go to next iteration
    - else if `char` is left parentheses
      - add 1 to `left_par_count`
      - push left parentheses char to `left_par_stack`
    - else
      - means `char` is right parentheses
      - if `left_par_stack` is empty,
        - return false
      - remove 1 from `left_par_count`
      - pop from `left_par_stack`
  - if `left_par_count` is 0 and `left_par_stack` is empty,
    - return true
  - else, return false

```ruby
def balanced?(text)
  left_par_count = 0
  left_par_stack = []
  text.each_char do |char|
    if char == '('
      left_par_count += 1
      left_par_stack.push(char)
    elsif char == ')'
      return false if left_par_stack.empty?
      left_par_count -= 1
      left_par_stack.pop()
    end
  end
  left_par_count == 0 && left_par_stack.empty?
end

p balanced?('What (is) this?') == true
p balanced?('What is) this?') == false
p balanced?('What (is this?') == false
p balanced?('((What) (is this))?') == true
p balanced?('((What)) (is this))?') == false
p balanced?('Hey!') == true
p balanced?(')Hey!(') == false
p balanced?('What ((is))) up(') == false
p balanced?('What ())(is() up') == false
```

### Official Solution

```ruby
def balanced?(string)
  parens = 0
  string.each_char do |char|
    parens += 1 if char == '('
    parens -= 1 if char == ')'
    break if parens < 0
  end

  parens.zero?
end
```

## Ex5 (Sloppy & Redo)

### My Solution

Notes:

- problem
  - input
    - `side1`: int or float
    - `side2`: int or float
    - `side3`: int or float
  - output
    - `res`: symbol
  - `side`, `side2`, `side3`
    - any int or float
    - can be negative, zero, or positive
- data structures
  - array to hold all 3 sides
- algorithm for `triangle(side1, side2, side3)`
  - set `sides` to array that holds all ints or floats from `side`, `side2`, `side3`
  - if `valid_triangle(sides)` returns false,
    - return `:invalid`
  - if `side1` == `side2` and `side2` == `side3` and `side1` == `side3`
    - return `:equilateral`
  - if `side1` != `side2` and `side2` != `side3` and `side1` != `side3`
    - return `:scalene`
  - return `:isoceles`
- algorithm for `valid_triangle?(sides)`
  - if any elements `side` in `sides` is less than or 0,
    - return false
  - sort `sides` in ASC order
  - if `sides[0]` + `sides[1]` > `sides[2]`,
    - return true,
  - else, return false

```ruby
def triangle(side1, side2, side3)
  sides = [side1, side2, side3]
  return :invalid unless valid_triangle?(sides)
  if side1 == side2 && side2 == side3 && side1 == side3
    :equilateral
  elsif side1 != side2 && side2 != side3 && side1 != side3
    :scalene
  else
    :isosceles
  end
end

def valid_triangle?(sides)
  return false if sides.any? { |side| side <= 0 }
  sides = sides.sort
  sides[0] + sides[1] > sides[2]
end

p triangle(3, 3, 3) == :equilateral
p triangle(3, 3, 1.5) == :isosceles
p triangle(3, 4, 5) == :scalene
p triangle(0, 3, 3) == :invalid
p triangle(3, 1, 1) == :invalid
```

### Official Solution

```ruby
def triangle(side1, side2, side3)
  sides = [side1, side2, side3]
  largest_side = sides.max

  case
  when 2 * largest_side >= sides.reduce(:+), sides.include?(0)
    :invalid
  when side1 == side2 && side2 == side3
    :equilateral
  when side1 == side2 || side1 == side3 || side2 == side3
    :isosceles
  else
    :scalene
  end
end
```

## Ex6

### My Solution

Notes:

- problem
  - input
    - `degree1`: int
    - `degree2`: int
    - `degree3`: int
  - output
    - `res`: symbol
  - `degree1`, `degree2`, `degree3`
    - negative, zero, or positive integer
    - represents an angle in degrees
  - `res`
    - 1 or 4 symbols that classifies the triangle represented by the 3 given degrees
- data structures
  - array to hold all degrees
- algorithm
  - if sum of all degrees is not 180,
    - return `:invalid`
  - set `degrees` to array that holds all degrees
  - loop thru each element `degree` in `degrees`,
    - return `:invalid` if `degree` < 1
    - return `:right` if `degree` is 90
    - return `:obtuse` if `degree` > 90
  - return `:acute`

```ruby
def triangle(degree1, degree2, degree3)
  return :invalid if degree1 + degree2 + degree3 != 180
  degrees = [degree1, degree2, degree3]
  for degree in degrees
    return :invalid if degree < 1
    return :right if degree == 90
    return :obtuse if degree > 90
  end
  :acute
end

p triangle(60, 70, 50) == :acute
p triangle(30, 90, 60) == :right
p triangle(120, 50, 10) == :obtuse
p triangle(0, 90, 90) == :invalid
p triangle(50, 50, 50) == :invalid
```

### Official Solution

```ruby
def triangle(angle1, angle2, angle3)
  angles = [angle1, angle2, angle3]

  case
  when angles.reduce(:+) != 180, angles.include?(0)
    :invalid
  when angles.include?(90)
    :right
  when angles.all? { |angle| angle < 90 }
    :acute
  else
    :obtuse
  end
end
```

## Ex7 (Redo)

### My Solution

Notes:

- problem
  - input
    - `year`: integer
  - output
    - `res`: integer
  - `year`
    - non-negative integer > 1752
    - represents a year after 1752
  - `res`
    - non-negative integer > -1
    - count of fridays in `year` that are on the 13th of a month
- data structures
  - `Date` class
- algorithm
  - set `res` to 0
  - loop for `month` from 1 to 12 inclusive
    - set `date` to new date at
      - month `month`, day 13, year `year`
    - if `date` is a Friday,
      - add 1 to `res`
  - return `res`

```ruby
require 'date'

def friday_13th(year)
  res = 0
  for month in (1..12)
    date = Date.new(year, month, 13)
    res += 1 if date.friday?
  end
  res
end

p friday_13th(2015) == 3
p friday_13th(1986) == 1
p friday_13th(2019) == 2
```

### Official Solution

```ruby
require 'date'

def friday_13th(year)
  unlucky_count = 0
  thirteenth = Date.civil(year, 1, 13)
  12.times do
    unlucky_count += 1 if thirteenth.friday?
    thirteenth = thirteenth.next_month
  end
  unlucky_count
end
```

## Ex8 (Misunderstood & Redo)

### My Solution

Notes:

- problem
  - input
    - `number`: integer
  - output
    - `res`: integer or string
  - `number`
    - assume to be positive integer
  - `res`
    - if `res` is less than `9_876_543_210`,
      - it is an integer in the range:
        - \[`number` + 1, `9_876_543_210`] exclusive
      - the next featured number in the range
    - else,
      - it is the string `There is no possible number that fulfills those requirements`
  - `n` is a featured number IFF:
    - it is odd
    - it is evenly divisible by 7
    - each of its digits is unique
    - it can be have at most 10 digits
- data structures
  - hash to count unique digits in a featured number
- algorithm
  - set `MAX_NUMBER` to 9_876_543_210
  - algorithm for `featured(number)`
    - set `res` to next number greater than `number` that is divisible by 7
      - equal to `number` int divided by 7 times 7 + 7
    - while `res` < `MAX_NUMBER`,
      - if `number` < number and `is_featured?(res)`
        - return `res`
      - add 7 to `res`
    - return error string
  - algorithm for `is_featured?(number)`
    - return false if `number` is even
    - return false if `number` is not divisible by 7
    - return false if `number` >= `MAX_NUMBER`
    - return false if `unique_digits?(number)` returns false
    - return true
  - algorithm for `unique_digits?(number)`
    - return true if `number` is 0
    - set `seen` to empty hash
    - while `number` > 0,
      - set `last_digit` to `number` % 10
      - if `last_digit` is a key in `seen`,
        - return false
      - add entry `(last_digit, true)` to `seen`
      - set `number` to `number` / 10
    - return true

```ruby
MAX_NUMBER = 9_876_543_210

def featured(number)
  res = (number / 7) * 7 + 7
  while res < MAX_NUMBER
    return res if number < res && is_featured?(res)
    res += 7
  end
  'There is no possible number that fulfills those requirements'
end

def is_featured?(number)
  return false if number % 2 == 0
  return false if number % 7 != 0
  return false if number >= MAX_NUMBER
  unique_digits?(number)
end

def unique_digits?(number)
  return true if number == 0
  seen = {}
  while number > 0
    last_digit = number % 10
    return false if seen[last_digit]
    seen[last_digit] = true
    number /= 10
  end
  true
end

p featured(12) == 21
p featured(20) == 21
p featured(21) == 35
p featured(997) == 1029
p featured(1029) == 1043
p featured(999_999) == 1_023_547
p featured(999_999_987) == 1_023_456_987

p featured(9_999_999_999) # -> There is no possible number that fulfills those requirements
```

### Official Solution

```ruby
def featured(number)
  number += 1
  number += 1 until number.odd? && number % 7 == 0

  loop do
    number_chars = number.to_s.split('')
    return number if number_chars.uniq == number_chars
    number += 14
    break if number >= 9_876_543_210
  end

  'There is no possible number that fulfills those requirements.'
end
```

## Ex9

### My Solution

Notes:

- problem
  - input
    - `list`: array
  - output
    - none
  - side effects
    - sorts `list` in ascending order
  - `list`
    - array of 2+ elements
    - each element is of the same type e.g. all ints, all strings, etc.
  - mutates `list` to sort it in place in ASC order
  - must use bubble sort algorithm for sorting `list`
- data structures
  - none
- algorithm
  - set `n` to `list`'s length
  - loop
    - set `is_swapped` to `false`
    - loop for `i` from 1 to `n` exclusive,
      - if `list[i - 1]` > `list[i]`,
        - set `temp` to `list[i - 1]`
        - set `list[i - 1]` to `list[i]`
        - set `list[i]` to `temp`
        - set `is_swapped` to `true`
      - set `n` to `n` - 1
    - break if `is_swapped` is `false`
  - optional: return `nil`

```ruby
def bubble_sort!(list)
  n = list.size
  loop do
    is_swapped = false
    for i in 1...n
      if list[i - 1] > list[i]
        temp = list[i - 1]
        list[i - 1] = list[i]
        list[i] = temp
        is_swapped = true
      end
    end
    n -= 1
    break unless is_swapped
  end
end

array = [5, 3]
p bubble_sort!(array)
array == [3, 5]

array = [6, 2, 7, 1, 4]
p bubble_sort!(array)
array == [1, 2, 4, 6, 7]

array = %w(Sue Pete Alice Tyler Rachel Kim Bonnie)
p bubble_sort!(array)
array == %w(Alice Bonnie Kim Pete Rachel Sue Tyler)
```

### Official Solution

```ruby
def bubble_sort!(array)
  loop do
    swapped = false
    1.upto(array.size - 1) do |index|
      next if array[index - 1] <= array[index]
      array[index - 1], array[index] = array[index], array[index - 1]
      swapped = true
    end

    break unless swapped
  end
end
```

## Ex10

### My Solution

Notes:

- problem
  - input
    - `n`: integer
  - output
    - `res`: integer
  - `n`
    - assumed integer > 0
  - `res`
    - sum of all positive integers from 1 to `n` minus sum of all squares of all positive integers from 1 to `n`
- data structures
  - none
- algorithm
  - set `integers_sum` to 0
  - set `integer_squares_sum` to 0
  - loop for `i` from 1 to `n` inclusive,
    - add `i` to `integers_sum`
    - add `i`^2 to `integer_squares_sum`
  - return `integers_sum`^2 - `integer_squares_sum`

```ruby
def sum_square_difference(n)
  integers_sum = 0
  integer_squares_sum = 0
  for i in 1..n
    integers_sum += i
    integer_squares_sum += i ** 2
  end
  integers_sum ** 2 - integer_squares_sum
end

p sum_square_difference(3) == 22
   # -> (1 + 2 + 3)**2 - (1**2 + 2**2 + 3**2)
p sum_square_difference(10) == 2640
p sum_square_difference(1) == 0
p sum_square_difference(100) == 25164150
```

### Official Solution

```ruby
def sum_square_difference(n)
  sum = 0
  sum_of_squares = 0

  1.upto(n) do |value|
    sum += value
    sum_of_squares += value**2
  end

  sum**2 - sum_of_squares
end
```

## Ex11 (Sloppy & Redo)

### My Solution

Notes:

- problem
  - input
    - `number`: integer
  - output
    - `res`: boolean
  - `number`
    - any integer
  - `res`
    - true if `number` is a prime number, else false
  - a number `n` is prime IFF:
    - `n` > 1
    - `n` is evenly divisible by 1
    - `n` is evenly divisible by `n`
    - `n` is not evenly divisible by any integer in the range
      - \[2, `n` - 1] inclusive
  - cannot use built-in `Prime` class
- data structures
  - none
- algorithm
  - return false if `number` <= 1
  - loop for `i` from 2 to `number` - 1 inclusive,
    - if `number` is evenly divisible by `i`,
      - return false
  - return true

```ruby
def is_prime(number)
  return false if number <= 1
  for i in 2..number - 1
    return false if number % i == 0
  end
  true
end

puts(is_prime(1) == false)              # true
puts(is_prime(2) == true)               # true
puts(is_prime(3) == true)               # true
puts(is_prime(4) == false)              # true
puts(is_prime(5) == true)               # true
puts(is_prime(6) == false)              # true
puts(is_prime(7) == true)               # true
puts(is_prime(8) == false)              # true
puts(is_prime(9) == false)              # true
puts(is_prime(10) == false)             # true
puts(is_prime(23) == true)              # true
puts(is_prime(24) == false)             # true
puts(is_prime(997) == true)             # true
puts(is_prime(998) == false)            # true
puts(is_prime(3_297_061) == true)       # true
puts(is_prime(23_297_061) == false)     # true
```

### Official Solution

Simplest Solution:

```ruby
def is_prime(number)
  return false if number == 1

  (2..(number - 1)).each do |divisor|
    if number % divisor == 0
      return false
    end
  end

  return true
end
```

Faster Solution:

```ruby
def is_prime(number)
  return false if number == 1

  (2..Mathsqrt(number)).each do |divisor|
    if number % divisor == 0
      return false
    end
  end

  return true
end
```
