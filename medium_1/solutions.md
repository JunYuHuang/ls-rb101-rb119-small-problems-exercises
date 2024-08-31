# Medium 1 Solutions

## Ex1 (Redo)

### My Solution

Notes:

- problem
  - input
    - `array`: array
  - output
    - `res`: array
  - `array`
    - cannot mutate it
    - assume to be non-empty
  - `res`
    - different array from `array`
    - first element is `array`'s last element
    - last element is `array`'s first element
  - cannot use built-in methods `Array#rotate` or `Array#rotate!`
- data structure
  - array to hold `res`
- algorithm
  - create first array that is subarray of `array` from index 1 to last index of `array`
  - create second array that only has the `array`'s first element
  - return third array that is the concatenation of the first and second arrays

```ruby
def rotate_array(array)
  array[1..-1] + [array[0]]
end

p rotate_array([7, 3, 5, 2, 9, 1]) == [3, 5, 2, 9, 1, 7]
p rotate_array(['a', 'b', 'c']) == ['b', 'c', 'a']
p rotate_array(['a']) == ['a']

x = [1, 2, 3, 4]
p rotate_array(x) == [2, 3, 4, 1]   # => true
p x == [1, 2, 3, 4]                 # => true
```

### Official Solution

```ruby
def rotate_array(array)
  array[1..-1] + [array[0]]
end
```

## Ex2 (Redo)

### My Solution

Notes:

- problem
  - input
    - `num`: integer
    - `n`: integer
  - output
    - `res`: integer
  - `num`
    - assume to be non-negative integer
  - `n`
    - integer > 0
    - refers to the last digits counting from the last digit in `num`
  - can reuse `rotate_array` method from `Ex1`
- data structures
  - strings
- algorithm
  - if `n` is 1,
    - return `num`
  - set `num_as_str` to `num` converted to a string
  - set `rotate_start_index` to (-1 \* `n`) + length of `nums_as_str`
  - set `left` to substring of `num_as_str` from index 0 to index `rotate_start_index` - 1
  - set `right` to substring of `num_as_str` from index `rotate_start_index` to last index of `num_as_str`
  - set `right` to return value of calling `rotate_array` with `right`
  - set `res` to string concatenation of `left` and `right`
  - return `res` converted to integer

```ruby
def rotate_rightmost_digits(num, n)
  num_as_str = num.to_s
  rotate_start_index = (-1 * n) + num_as_str.size
  left = num_as_str[0...rotate_start_index]
  right = num_as_str[rotate_start_index...num_as_str.size]
  right = rotate_array(right.split('')).join('')
  (left + right).to_i
end

def rotate_array(array)
  array[1..-1] + [array[0]]
end

p rotate_rightmost_digits(735291, 1) == 735291
p rotate_rightmost_digits(735291, 2) == 735219
p rotate_rightmost_digits(735291, 3) == 735912
p rotate_rightmost_digits(735291, 4) == 732915
p rotate_rightmost_digits(735291, 5) == 752913
p rotate_rightmost_digits(735291, 6) == 352917
```

### Official Solution

```ruby
def rotate_rightmost_digits(number, n)
  all_digits = number.to_s.chars
  all_digits[-n..-1] = rotate_array(all_digits[-n..-1])
  all_digits.join.to_i
end
```

## Ex3 (Redo)

### My Solution

Notes:

- problem
  - input
    - `num`: integer
  - output
    - `res`: integer
  - `num`
    - non-negative integer
  - `res`
    - `num` with its digits rotated
      - rotation starts with `num`'s first digit
      - rotation stops at `num`'s second last digit
      - every rotation rotates 1 less digits starting from the first digit of `num`
      - equals `num` if `num` has 1 digit only
  - can reuse `rotate_array` method from `Ex1`
  - can reuse `rotate_rightmost_digits` method from `Ex2`
- data structures
  - array
- algorithm
  - set `res` to `num`
  - set `index` to `num` as a string's length
  - while `index` >= 2,
    - set `res` to `rotate_rightmost_digits(res, index)`
    - decrement `index` by 1
  - return `res`

```ruby
def max_rotation(num)
  res = num
  index = num.to_s.size
  while index >= 2
    res = rotate_rightmost_digits(res, index)
    index -= 1
  end
  res
end

def rotate_rightmost_digits(number, n)
  all_digits = number.to_s.chars
  all_digits[-n..-1] = rotate_array(all_digits[-n..-1])
  all_digits.join.to_i
end

def rotate_array(array)
  array[1..-1] + [array[0]]
end

p max_rotation(735291) == 321579
p max_rotation(3) == 3
p max_rotation(35) == 53
p max_rotation(105) == 15 # the leading zero gets dropped
p max_rotation(8_703_529_146) == 7_321_609_845
```

### Official Solution

```ruby
def max_rotation(number)
  number_digits = number.to_s.size
  number_digits.downto(2) do |n|
    number = rotate_rightmost_digits(number, n)
  end
  number
end
```

## Ex4 (Redo)

### My Solution

Notes:

- problem
  - input
    - `n`: integer
  - output
    - `res`: int array
  - `n`
    - integer greater than 0
  - `res`
    - int array
    - elements are the integer numbered switches that are turned on after toggling the switches `n` times in a defined process
  - switches are 1-indexed
- examples
  - for `n` = 5
    - 0-indexed 0 1 2 3 4
    - round 0 [ F F F F F ]
    - round 1 [ T T T T T ]
    - round 2 [ T F T F T ]
    - round 3 [ T F F F T ]
    - round 4 [ T F F T T ]
    - round 5 [ T F F T F ]
- data structures
  - array to hold `res`
  - array to hold status of all light switches
- algorithm
  - set `switch_status` to `n`-sized array of all `false` booleans
  - loop for `round` from 1 to `n` inclusive
    - set `toggle_index` to `i` - 1
    - while `toggle_index` < `n`,
      - set `switch_status[toggle_index]` to itself negated
      - increment `toggle_index` by `round`
  - set `res` to empty array
  - for each element `el` with its index `i` in `switch_status`,
    - if `el` is true,
      - push `i` + 1 to `res`
  - return `res`

```ruby
def toggle_switches(n)
  switch_status = [false] * n
  for round in (1..n)
    toggle_index = round - 1
    while toggle_index < n
      switch_status[toggle_index] = !switch_status[toggle_index]
      toggle_index += round
    end
  end

  res = []
  switch_status.each_with_index do |is_on, index|
    res.push(index + 1) if is_on
  end
  res
end

p toggle_switches(1) == [1]
p toggle_switches(2) == [1]
p toggle_switches(3) == [1]
p toggle_switches(4) == [1, 4]
p toggle_switches(5) == [1, 4]
p toggle_switches(10) == [1, 4, 9]
```

### Official Solution

```ruby
# initialize the lights hash
def initialize_lights(number_of_lights)
  lights = Hash.new
  1.upto(number_of_lights) { |number| lights[number] = 'off' }
  lights
end

# return list of light numbers that are on
def on_lights(lights)
  lights.select { |_position, state| state == 'on' }.keys
end

# toggle every nth light in lights hash
def toggle_every_nth_light!(lights, nth)
  lights.each do |position, state|
    if position % nth == 0
      lights[position] = (state == 'off') ? 'on' : 'off'
    end
  end
end

# Run entire program for number of lights
def toggle_lights(number_of_lights)
  lights = initialize_lights(number_of_lights)
  1.upto(lights.size) do |iteration_number|
    toggle_every_nth_light!(lights, iteration_number)
  end

  on_lights(lights)
end

p toggle_lights(1000)
```

## Ex5 (Wrong & Redo)

### My Solution

Notes:

- problem
  - input
    - `n`: integer
  - output
    - none
  - side effects
    - prints out a 4-pointed diamond of `*` asterisk chars to the console
  - `n`
    - guaranteed to be a positive odd integer
  - diamond meaning
    - like a `n`-sized string array
    - every element in the array
      - is a string of up to `n` chars
      - has certain amount of ` ` space chars
      - has certain amount of `*` star chars
- examples
  - for `n` == 5
    - diamond
      ```
      1   *
      2  ***
      3 *****
      4  ***
      5   *
      ```
    - row 0: 2 left spaces, 1 stars
    - row 1: 1 left spaces, 3 stars
    - row 2: 0 left spaces, 5 stars
    - row 3: 1 left spaces, 3 stars
    - row 4: 2 left spaces, 1 stars
- data structures
  - string array to hold the 'diamond'
- algorithm
  - set `res` to `n`-sized array of all `*` chars
  - set `left_index` to 0
  - set `right_index` to `n` - 1
  - set `side_space_count` to (`n` - 1) / 2
  - set `star_count` to 1
  - while `left_index` <= `right_index`,
    - set `row` to empty string
    - append `side_space_count` amount of ` ` chars to `row`
    - append `star_count` amount of `*` chars to `row`
    - append `side_space_count` amount of ` ` chars to `row`
    - set `res[left_index]` to `row`
    - set `res[right_index]` to `row`
    - minus 1 from `side_space_count`
    - add 2 to `star_count`
    - add 1 to `left_index`
    - minus 1 from `right_index`
  - print each element of `res` on its own line

```ruby
def diamond(n)
  res = [''] * n
  side_space_count = (n - 1) / 2
  star_count = 1
  left_index = 0
  right_index = n - 1
  while left_index <= right_index
    row = ' ' * side_space_count + '*' * star_count
    row += ' ' * side_space_count
    res[left_index] = row
    res[right_index] = row
    side_space_count -= 1
    star_count += 2
    left_index += 1
    right_index -= 1
  end
  puts(res)
  puts
end

diamond(1)
diamond(3)
diamond(5)
diamond(7)
diamond(9)
```

### Official Solution

```ruby
def print_row(grid_size, distance_from_center)
  number_of_stars = grid_size - 2 * distance_from_center
  stars = '*' * number_of_stars
  puts stars.center(grid_size)
end

def diamond(grid_size)
  max_distance = (grid_size - 1) / 2
  max_distance.downto(0) { |distance| print_row(grid_size, distance) }
  1.upto(max_distance) { |distance| print_row(grid_size, distance) }
end
```

## Ex6

### My Solution

Notes:

- problem
  - input
    - `syntax`: string
  - output
    - none
  - side effects
    - stores internal state for
      - `register`: integer
      - `value_stack`: integer array
    - prints current value stored in register when running command `'PRINT'`
  - `syntax`
    - guaranteed to be a valid and correct program written in the custom `minilang` language
  - `minilang` language specs
    - values are only integers
    - commands
      - are strings in ALL_CAPS casing OR a numerical number that represents an integer as a string
      - are executed in order of appearance in `syntax`
      - cannot take arguments
      - can only manipulate values in `register` or `value_stack`
      - are delimited by a single space char
    - keeps running program until all commands have been run
  - divide `a` into `b` means `b` / `a`
- data structures
  - arrays / stacks to store the values and commands in the `minilang` syntax
- algorithm for `minilang(syntax)`
  - set `register` to 0
  - set `value_stack` to empty array
  - set `commands` to a string array created from splitting `syntax` with the space char delimiter
  - loop thru each string `command` in `commands`,
    - if `command` is an integer,
      - set `register` to `command` converted to an integer
    - else,
      - process the command as needed
        - mutate / reassign `register` and/or `value_stack` as needed
- algorithm for `integer?(string)`
  - if `string` converted to an int converted to a string equals `string`,
    - return true
  - else, return false

```ruby
def minilang(syntax)
  register = 0
  value_stack = []
  commands = syntax.split
  puts ">> Program start"
  for command in commands
    if integer?(command)
      register = command.to_i
    else
      case command
      when 'PUSH'
        value_stack.push(register)
      when 'ADD'
        register = register + value_stack.pop
      when 'SUB'
        register = register - value_stack.pop
      when 'MULT'
        register = register * value_stack.pop
      when 'DIV'
        register = register / value_stack.pop
      when 'MOD'
        register = register % value_stack.pop
      when 'POP'
        register = value_stack.pop
      when 'PRINT'
        puts register
      else
        puts 'Syntax error!'
      end
    end
  end
  puts ">> Program end\n"
end

def integer?(string)
  string == string.to_i.to_s
end

minilang('PRINT')
# 0

minilang('5 PUSH 3 MULT PRINT')
# 15

minilang('5 PRINT PUSH 3 PRINT ADD PRINT')
# 5
# 3
# 8

minilang('5 PUSH POP PRINT')
# 5

minilang('3 PUSH 4 PUSH 5 PUSH PRINT ADD PRINT POP PRINT ADD PRINT')
# 5
# 10
# 4
# 7

minilang('3 PUSH PUSH 7 DIV MULT PRINT ')
# 6

minilang('4 PUSH PUSH 7 MOD MULT PRINT ')
# 12

minilang('-3 PUSH 5 SUB PRINT')
# 8

minilang('6 PUSH')
# (nothing printed; no PRINT commands)
```

### Official Solution

```ruby
def minilang(program)
  stack = []
  register = 0
  program.split.each do |token|
    case token
    when 'ADD'    then register += stack.pop
    when 'DIV'    then register /= stack.pop
    when 'MULT'   then register *= stack.pop
    when 'MOD'    then register %= stack.pop
    when 'SUB'    then register -= stack.pop
    when 'PUSH'   then stack.push(register)
    when 'POP'    then register = stack.pop
    when 'PRINT'  then puts register
    else               register = token.to_i
    end
  end
end
```

## Ex7 (Wrong & Redo)

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: string
  - `text`
    - assume to be non-empty string
    - has 0+ words
  - word meaning
    - any substring in `text` that is a consecutive series of non-space chars
  - `res`
    - separate string from `text`
    - converts English words for the 10 numerical digits to their numerical string form
- data structures
  - array to hold words from `text`
- algorithm
  - set `to_digit` to a hash that maps string words for `'zero'` to `'nine'` to their arabic number digit string equivalent
  - set `words` to string array that splits `text` by the space char delimiter
  - loop every string element `word` in `words`,
    - if `word` is in `to_digit`,
      - replace `word` with its associated value in `to_digit`
  - return `words` joined as a string via the space char

```ruby
def word_to_digit(text)
  to_digit = {
    'zero' => '0', 'one' => '1', 'two' => '2', 'three' => '3',
    'four' => '4', 'five' => '5', 'six' => '6', 'seven' => '7',
    'eight' => '8', 'nine' => '9'
  }
  text_copy = text.dup
  to_digit.each do |key, value|
    text_copy.gsub!(key, value) if text_copy.include?(key)
  end
  text_copy
end

p word_to_digit('Please call me at five five five one two three four. Thanks.') == 'Please call me at 5 5 5 1 2 3 4. Thanks.'
p word_to_digit('freight') == 'freight'
```

### Official Solution

```ruby
DIGIT_HASH = {
  'zero' => '0', 'one' => '1', 'two' => '2', 'three' => '3', 'four' => '4',
  'five' => '5', 'six' => '6', 'seven' => '7', 'eight' => '8', 'nine' => '9'
}.freeze

def word_to_digit(words)
  DIGIT_HASH.keys.each do |word|
    words.gsub!(\/b#{word}\b/, DIGIT_HASH[word])
  end
  words
end
```

## Ex8

### My Solution

Notes:

- problem
  - input
    - `nth`: integer
  - output
    - `res`: integer
  - `nth`
    - integer > 0
  - `res`
    - the `nth` Fibonacci number
  - Fibonacci sequence
    - 1, 1, 2, 3, ...
    - first two numbers are always 1
    - every number `n` after the first two numbers is the sum of the previous two numbers
- data structures
  - none
- algorithm
  - return 1 if `nth` < 3
  - return `fibonacci(nth - 1)` + `fibonacci(nth - 2)`

```ruby
def fibonacci(nth)
  return 1 if nth < 3
  fibonacci(nth - 1) + fibonacci(nth - 2)
end

p fibonacci(1) == 1
p fibonacci(2) == 1
p fibonacci(3) == 2
p fibonacci(4) == 3
p fibonacci(5) == 5
p fibonacci(12) == 144
p fibonacci(20) == 6765
```

### Official Solution

Solution 1:

```ruby
def fibonacci(nth)
  return 1 if nth <= 2
  fibonacci(nth - 1) + fibonacci(nth - 2)
end
```

Solution 2 (tail recursion):

```ruby
def fibonacci_tail(nth)
  if nth == 1
    acc1
  elsif nth == 2
    acc2
  else
    fibonacci_tail(nth - 1, acc2, acc2 + acc1)
  end
end

def fibonacci(nth)
  fibonacci_tail(nth, 1, 1)
end
```

## Ex9

### My Solution

Notes:

- problem
  - input
    - `nth`: integer
  - output
    - `res`: integer
  - `nth`
    - integer > 0
  - `res`
    - the `nth` Fibonacci number
  - Fibonacci sequence
    - 1, 1, 2, 3, ...
    - first two numbers are always 1
    - every number `n` after the first two numbers is the sum of the previous two numbers
- data structures
  - none
- algorithm
  - return 1 if `nth` < 3
  - set `first` to 1
  - set `second` to 1
  - set `res` to -1
  - loop for `i` from 3 to `nth` inclusive,
    - set `res` to sum of `first` and `second`
    - set `second` to `first`'s value
    - set `first` to `res`' value
  - return `res`

```ruby
def fibonacci(nth)
  return 1 if nth < 3
  first = 1
  second = 1
  res = -1
  for i in (3..nth)
    res = first + second
    second = first
    first = res
  end
  res
end

p fibonacci(20) == 6765
p fibonacci(100) == 354224848179261915075
p fibonacci(100_001) # => 4202692702.....8285979669707537501
```

### Official Solution

```ruby
def fibonacci(nth)
  first, last = [1, 1]
  3.upto(nth) do
    first, last = [last, first + last]
  end

  last
end
```

## Ex10

### My Solution

Notes:

- problem
  - input
    - `nth`: integer
  - output
    - `res`: integer
  - `nth`
    - integer > 0
  - `res`
    - the `nth` Fibonacci number's last digit
  - Fibonacci sequence
    - 1, 1, 2, 3, ...
    - first two numbers are always 1
    - every number `n` after the first two numbers is the sum of the previous two numbers
- data structures
  - none
- algorithm
  - return 1 if `nth` < 3
  - set `first` to 1
  - set `second` to 1
  - loop for `_i` from 3 to `nth` inclusive,
    - set `temp` to `first`'s value
    - set `first` to `second`' value
    - set `second` to sum of `temp` and `first`
  - return `second` modded by 10

```ruby
def fibonacci_last(nth)
  return 1 if nth < 3
  first = 1
  second = 1
  for _i in (3..nth)
    temp = first
    first = second
    second = temp + first
  end
  second % 10
end

p fibonacci_last(15)        # -> 0  (the 15th p fibonacci number is 610)
p fibonacci_last(20)        # -> 5 (the 20th p fibonacci number is 6765)
p fibonacci_last(100)       # -> 5 (the 100th p fibonacci number is 354224848179261915075)
p fibonacci_last(100_001)   # -> 1 (this is a 20899 digit number)
p fibonacci_last(1_000_007) # -> 3 (this is a 208989 digit number)
p fibonacci_last(123456789) # -> 4
```

### Official Solution

Part 1:

```ruby
def fibonacci_last(nth)
  fibonacci(nth).to_s[-1].to_i
end
```

Part 2:

```ruby
def fibonacci_last(nth)
  last_2 = [1, 1]
  3.upto(nth) do
    last_2 = [last_2.last, (last_2.first + last_2.last) % 10]
  end

  last_2.last
end
```
