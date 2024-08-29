# Easy 8 Solutions

## Ex1

### My Solution

Notes:

- problem
  - input
    - `numbers`: int array
  - output
    - `res`: int
  - `numbers`
    - guaranteed
      - to be non-empty
      - only has ints
  - `res`
    - sum of sums of all subarrays that start from `numbers`' first element and end from the first element in `numbers` to the last element in `numbers`
- data structures
  - none
- algorithm
  - set `subtotal` to 0
  - set `res` to 0
  - loop thru each element `num` in `numbers`,
    - add `num` to `subtotal`
    - add `subtotal` to `res`
  - return `res`

```ruby
def sum_of_sums(numbers)
  subtotal = 0
  res = 0
  numbers.each do |num|
    subtotal += num
    res += subtotal
  end
  res
end

p sum_of_sums([3, 5, 2]) == (3) + (3 + 5) + (3 + 5 + 2) # -> (21)
p sum_of_sums([1, 5, 7, 3]) == (1) + (1 + 5) + (1 + 5 + 7) + (1 + 5 + 7 + 3) # -> (36)
p sum_of_sums([4]) == 4
p sum_of_sums([1, 2, 3, 4, 5]) == 35
```

### Official Solution

Solution 1:

```ruby
def sum_of_sums(numbers)
  sum_total = 0
  accumulator = 0

  numbers.each do |num|
    accumulator += num
    sum_total += accumulator
  end

  sum_total
end
```

Solution 2:

```ruby
def sum_of_sums(numbers)
  sum_total = 0
  1.upto(numbers.size) do |count|
    sum_total += numbers.slice(0, count).reduce(:+)
  end
  sum_total
end
```

## Ex2 (Redo)

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: string array
  - `text`
    - assumed to be non-empty
  - `res`
    - sorted in ASC order of shortest to longest strings
- data structures
  - array to hold `res`
- algorithm
  - set `res` to empty array
  - loop for `i` from 0 to length of `text` minus 1 inclusive
    - get substring of `text` that starts at index 0 and ends at (i.e. includes) index `i`
    - push this substring to `res`
  - return `res`

```ruby
def leading_substrings(text)
  res = []
  for i in (0..text.size - 1)
    res << text[0..i]
  end
  res
end

p leading_substrings('abc') == ['a', 'ab', 'abc']
p leading_substrings('a') == ['a']
p leading_substrings('xyzzy') == ['x', 'xy', 'xyz', 'xyzz', 'xyzzy']
```

### Official Solution

```ruby
def leading_substrings(string)
  result = []
  0.upto(string.size - 1) do |index|
    result << string[0..index]
  end
  result
end
```

## Ex3

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: string array
  - `text`
    - assumed to be non-empty
  - `res`
    - array of all substrings of `text`
    - in order of where each substring beings
      - starts with substrings from `text`'s first char
      - ends with substrings from `text`'s last char
  - can use previous exercise (`Ex4`)'s `leading_substrings` method
- data structures
  - array to hold `res`
- algorithm
  - set `res` to empty array
  - loop thru every index `i` in `text` from its first to last index
    - set `sub` to substring of `text` that
      - starts at index `i`
      - ends at `text`'s last index
    - call `leading_substrings` with `sub`
    - store result of call in `subs`
    - push all elements in `subs` to `res`
  - return `res`

```ruby
def substrings(text)
  res = []
  for i in (0...text.size)
    subs = leading_substrings(text[i..-1])
    res.concat(subs)
  end
  res
end

def leading_substrings(text)
  res = []
  for i in (0..text.size - 1)
    res << text[0..i]
  end
  res
end

p substrings('abcde') == [
  'a', 'ab', 'abc', 'abcd', 'abcde',
  'b', 'bc', 'bcd', 'bcde',
  'c', 'cd', 'cde',
  'd', 'de',
  'e'
]
```

### Official Solution

```ruby
def substrings(string)
  results = []
  (0...string.size).each do |start_index|
    this_substring = string[start_index..-1]
    results.concat(leading_substrings(this_substring))
  end
  results
end
```

## Ex4

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: string array
  - `text`
    - assumed to be a non-empty string
  - `res`
    - should include duplicate palindromes
    - elements in order of appearance
  - palindrome meaning
    - a string that must be 2+ chars long
  - can reuse `substrings` method from `Ex3`
  - can reuse `leading_substrings` method from `Ex2`
- algorithm for `palindromes(text)`
  - set `res` to return value of calling `substrings(text)`
  - return and select string elements `string` in `res` that
    - are of length 2 or greater AND
    - is a palindrome (i.e. `palindrome?(string)` returns true)
- algorithm for `palindrome?(string)`
  - if `string` is same as its reversed string,
    - return true
  - else return false

```ruby
def palindromes(text)
  substrings(text).select do |string|
    string.size > 1 && palindrome?(string)
  end
end

def palindrome?(string)
  string == string.reverse
end

def substrings(text)
  res = []
  for i in (0...text.size)
    subs = leading_substrings(text[i..-1])
    res.concat(subs)
  end
  res
end

def leading_substrings(text)
  res = []
  for i in (0..text.size - 1)
    res << text[0..i]
  end
  res
end

palindromes('abcd') == []
palindromes('madam') == ['madam', 'ada']
palindromes('hello-madam-did-madam-goodbye') == [
  'll', '-madam-', '-madam-did-madam-', 'madam', 'madam-did-madam', 'ada',
  'adam-did-mada', 'dam-did-mad', 'am-did-ma', 'm-did-m', '-did-', 'did',
  '-madam-', 'madam', 'ada', 'oo'
]
palindromes('knitting cassettes') == [
  'nittin', 'itti', 'tt', 'ss', 'settes', 'ette', 'tt'
]
```

### Official Solution

```ruby
def palindromes(string)
  all_substrings = substrings(string)
  results = []
  all_substrings.each do |substring|
    results << substring if palindrome?(substring)
  end
  results
end

def palindrome?(string)
  string == string.reverse && string.size > 1
end
```

## Ex5 (Wrong & Redo)

### My Solution

Notes:

- problem
  - input
    - `first`: integer
    - `second`: integer
  - output
    - none
  - side effects
    - print out the array `res` to the console
    - `res`
      - an array of all numbers from `first` to `second` but
        - numbers divisible by 3 and 5 are replaced by the string `'FizzBuzz'`
        - numbers divisible by 3 and not by 5 are replaced by the string `'Fizz'`
        - numbers divisible by 5 and not by 3 are replaced by the string `'Buzz'`
- data structures
  - array to hold elements to print
- algorithm
  - set `res` to empty array
  - loop for `i` from `first` to `second` inclusive,
    - if `i` is divisible by both 3 and 5
      - push `'FizzBuzz'` to `res`
    - else if `i` is divisible by 3
      - push `'Fizz'` to `res`
    - else if `i` is divisible by 5
      - push `'Buzz'` to `res`
    - else
      - push `i` to `res`
  - print `res` as a string joined with substring `', '` to the console

```ruby
def fizzbuzz(first, second)
  res = []
  for i in (first..second)
    if i % 3 == 0 && i % 5 == 0
      res.push('FizzBuzz')
    elsif i % 3 == 0
      res.push('Fizz')
    elsif i % 5 == 0
      res.push('Buzz')
    else
      res.push(i)
    end
  end
  puts res
end

fizzbuzz(1, 15) # -> 1, 2, Fizz, 4, Buzz, Fizz, 7, 8, Fizz, Buzz, 11, Fizz, 13, 14, FizzBuzz
```

### Official Solution

```ruby
def fizzbuzz(starting_number, ending_number)
  result = []
  starting_number.upto(ending_number) do |number|
    result << fizzbuzz_value(number)
  end
  puts result.join(', ')
end

def fizzbuzz_value(number)
  case
  when number % 3 == 0 && number % 5 == 0
    'FizzBuzz'
  when number % 5 == 0
    'Buzz'
  when number % 5 == 0
    'Fizz'
  else
    number
  end
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
  - `text` and `res` point to different strings
  - `text`
    - may be empty
  - `res`
    - twice the length of `text`
    - has two of each char in `text` in order
- data structures
  - string to hold `res`
- algorithm
  - set `res` to empty string
  - loop for each char `char` in `text`
    - append `res` with new substring created from `char` concatenated with itself
  - return `res`

```ruby
def repeater(text)
  res = ''
  text.each_char do |char|
    res.concat(char + char)
  end
  res
end

p repeater('Hello') == "HHeelllloo"
p repeater("Good job!") == "GGoooodd  jjoobb!!"
p repeater('') == ''
```

### Official Solution

```ruby
def repeater(string)
  result = ''
  string.each_char do |char|
    result << char << char
  end
  result
end
```

## Ex7

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: string
  - same as `Ex6` but
    - only double consonant chars
      - append consonant chars to `res` 2 times
    - consonant char meaning
      - any lower or uppercase English letter not in the string `aeiou`
    - non-consonant chars are appended to `res` once
- data structures
  - string to hold `res`
- algorithm for `double_consonants(text)`
  - set `res` to empty string
  - loop thru each char `char` in `text`,
    - if `char` is a consonant,
      - append `res` with new string from `char` concatenated with itself
    - else
      - append `res` with `char`
  - return `res`
- algorithm for `consonant?(char)`
  - return false if `char` is not a lowercase char
  - return false if `char` is not an uppercase char
  - if `char` is a vowel,
    - return true
  - else, return false

```ruby
def double_consonants(text)
  res = ''
  text.each_char do |char|
    if consonant?(char)
      res << (char + char)
    else
      res << char
    end
  end
  res
end

def consonant?(char)
  return false unless char =~ /[a-z]/i
  !!(char =~ /[^aeiou]/i)
end

p double_consonants('String') == "SSttrrinngg"
p double_consonants("Hello-World!") == "HHellllo-WWorrlldd!"
p double_consonants("July 4th") == "JJullyy 4tthh"
p double_consonants('') == ""
```

### Official Solution

```ruby
CONSONANTS = %w(b c d f g h j k l m n p q r s t v w x y z)

def double_consonants(string)
  result = ''
  string.each_char do |char|
    result << char
    result << char if CONSONANTS.include?(char.downcase)
  end
  result
end
```

## Ex8 (Misunderstood & Redo)

### My Solution

Notes:

- problem
  - input
    - `text`: string
  - output
    - `res`: string
  - word meaning
    - any substring in `text` that is a consecutive series of non-space chars
  - `text`
    - may be empty
    - of 0+ words delimited by the ` ` space char
  - `res`
    - is a new string that has all words in `text` in reverse order
      - first word in `text` is last word in `res`
    - is an empty string if `text` consists of only ` ` space chars
- data structures
  - array to hold construction of `res`
- algorithm for `reverse_sentence(text)`
  - set `words` to array created from `text` using the ` ` space char as a delimiter
  - if `words` is empty,
    - return an empty string `''`
  - set `reversed_words` to an empty string
  - loop thru each item `word` in `words` backwards from the last to first element
    - append `word` to `reversed_words`
  - return `reversed_words` as a string joining all its elements with the space char

```ruby
def reverse_sentence(text)
  words = text.split(' ')
  return '' if words.size == 0

  reversed_words = []
  words.each do |word|
    reversed_words.unshift(word)
  end
  reversed_words.join(' ')
end

puts reverse_sentence('Hello World') == 'World Hello'
puts reverse_sentence('Reverse these words') == 'words these Reverse'
puts reverse_sentence('') == ''
puts reverse_sentence('    ') == '' # Any number of spaces results in ''
```

### Official Solution

```ruby
def reverse_sentence(string)
  string.split.reverse.join(' ')
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
  - word meaning
    - any substring in `text` that is a consecutive series of non-space chars
  - `text`
    - guaranteed to be non-empty
    - of 1+ words delimited by the ` ` space char
  - `res`
    - is a new string that has all words in `text` in reverse order
      - first word in `text` is last word in `res`
    - words are joined by a space chars
- data structures
  - array to hold construction of `res`
- algorithm
  - create a string array from splitting `text` using the ` ` space char delimiter
  - transform the string array to a new array where each element in the new array is
    - the old array's element if the old array element's length is less than 5 chars long
    - the old array's element reversed if the old array element's length is 5 or longer
  - create a string that joins all elements in the new transformed array using the ` ` space char as a delimiter
  - return this new string

```ruby
def reverse_words(text)
  text.split.map do |word|
    word.size >= 5 ? word.reverse : word
  end.join(' ')
end

puts reverse_words('Professional')          # => lanoisseforP
puts reverse_words('Walk around the block') # => Walk dnuora the kcolb
puts reverse_words('Launch School')         # => hcnuaL loohcS
```

### Official Solution

```ruby
def reverse_words(string)
  words = []

  string.split.each do |word|
    word.reverse! if words.size >= 5
    words << word
  end

  words.join(' ')
end
```

At the top of ouir method, we create an empty array named `words` that will hold each modified word of the result: these words will be reversed if long, or as-is if they are short. We use `#each` to iterate over `string`, but first, we need to separate each word in `string` using `#split`, which returns an array containing the separated words. For each word, we count the number of characters it contains using `#size`. If it contains five or more characters, we use the destructive method `#reverse!` to reverse the word. We mutate `word` so that we can add it to `words` by invoking `words << word`.

After iterating over `string` and evaluating each word, `words` will contain all of the words, with longer words reversed. Finally, we can invoke `words.join(' ' )` to return the desired string.

## Ex10

### My Solution

Notes:

- problem
  - input
    - `nums`: integer array
  - output
    - `res`: integer
  - average meaning
    - sum of all numbers given divided by how many numbers were given
  - `nums`
    - guaranteed to
      - be non-empty
      - only have positive ints
  - `res`
    - must be integer
    - average of ints in `nums`
    - use integer division to truncate non-integer quotients when calculating average
- data structures
  - none
- algorithm
  - set `res` to 0
  - loop thru each element `num` in `nums`,
    - set `res` to sum of itself and `num`
  - return the quotient of doing int division on `res` by length of `nums`

```ruby
def average(nums)
  res = 0
  for num in nums
    res += num
  end
  res / nums.size
end

puts average([1, 6]) == 3 # integer division: (1 + 6) / 2 -> 3
puts average([1, 5, 87, 45, 8, 8]) == 25
puts average([9, 47, 23, 95, 16, 52]) == 40
```

### Official Solution

```ruby
def average(numbers)
  sum = numbers.reduce { |sum, number| sum + number }
  sum / numbers.count
end
```

Two things need to be done to find the average. First, add every number together. Second, divide the sum by the number of elements. We accomplish the first part by using `Enumerable#reduce` (also known as `#inject`), which combines all elements of the given array by applying a binary operation. This operation is specified by a block or symbol. We used a block in our solution, but we could have just as easily used a symbol, like this:

```ruby
numbers.reduce(:+)
```

Once we have the sum, all that's left is to divide it by the number of elements. To do that, we use `#count` to count the number of elements in `numbers`. Then, we divide `sum` by the number of elements and return the quotient.
