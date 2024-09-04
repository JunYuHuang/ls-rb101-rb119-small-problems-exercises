# Debugging Solutions

## Ex1 (Redo)

### My Solution

Local variable `counter` is to the integer `10` on line 5. On line 7, `times` is called on the integer `10` with a `do..end` block. The block outputs `counter`'s value and calls `decrease` with `counter`'s integer value. Method `decrease` reassigns its `counter` parameter to the inner method variable `counter`'s integer value minus 1. Integers are immutable in Ruby, which means the `decrease` call never alters the outer `counter`'s assigned integer value. Thus, the block outputs the unchanged value of `counter` -- `10` -- each time the block is called -- 10 times in total.

The program can be fixed by directly reassigning the outer `counter`'s value to its value minus 1 in the block. In this case, method `decrease`'s definition and call can be removed. Fixed code:

```ruby
counter = 10

10.times do
  puts counter
  counter -= 1
end

puts 'LAUNCH!'
```

### Official Solution

```ruby
def decrease(counter)
  counter - 1
end

counter = 10

10.times do
  puts counter
  counter = decrease(counter)
end

puts 'LAUNCH!'
```

Relevant to notice here is that the `counter` variable initialized on line 5 and the `counter` parameter of the `decrease` method are two independent variables. Most importantly, the latter lives only within the scope of the method. On line 2 of our original code snippet we reassign the method parameter to the result of its value minus 1. Recall that `counter -= 1` is shorthand for `counter = counter - 1`. This changes the value of the `counter` variable local to the method, but not the variable `counter` referenced on lines 5, 8 and 9. We also don't use the return value of the method anywhere, so our `counter` variable outside of the method continues to reference the integer `10`.

To address this, we can reassign the variable `counter` on line 9 to the return value of `decrease(counter)` each time we iterate through the block. For clarity, we also remove the reassignment in the `decrease` method, as it does not serve any purpose.

## Ex2 (Redo)

### My Solution

On line 7, method `shout_out_to` is called with the string `'you'` which binds to the method's `name` parameter. On line 2, `each` is called on the string array returned from calling `chars` on `name`. The `{..}` block passed to `each` calls `String#upcase!` on the current iterated string element in the array, which mutates its caller string. The string referenced by parameter `name` and the string array returned by the `chars` call are 2 different objects. The mutating `upcase!` call does not affect `name`'s string, thus line 4 outputs `'HEY you'`.

The problem can be fixed by chaining a `join` call on the `each` call after the passed block and reassigning `name` to the return value of the modified method chain on line 2, as shown below:

```ruby
def shout_out_to(name)
  name = name.chars.each { |c| c.upcase! }.join

  puts 'HEY ' + name
end

shout_out_to('you') # expected: 'HEY YOU'
```

### Official Solution

```ruby
def shout_out_to(name)
  name.upcase!
  puts 'HEY ' + name
end

shout_out_to('you')
```

The `String#chars` method returns an array of the characters in a string, so `name.chars` in our example returns `['y', 'o', 'u']`. Note that these character strings are new String objects, different from the `name` string, and it's those objects that we mutate on line 2. On line 4, `name` is therefore still `'you'`.

The solution is to upcase `name` directly, either as show in the solution above or using the non-destructive `String#upcase` method as follows:

```ruby
def shout_out_to(name)
  puts 'HEY ' + name.upcase
end
```

## Ex3 (Redo)

### My Solution

Method `valid_series?`'s last evaluated expression on line 5 always returns the boolean `true`. This expression reassigns the method variable `odd_count` to `true` since the ternary `if`'s test clause `3` is always truthy. Hence, it does not matter what integer is assigned to variable `odd_count` from the return value of the `count` call on line 4.

On line 12, `valid_series?` is called with a certain integer array. Since the array's sum is `47`, the call skips past the method's explicit return on line 2. On line 4, `odd_count` is assigned to `1`. However, since `odd_count` is always reassigned to `true` on line 5, that is what is returned by `valid_series?` and output on line 12.

Fix this problem by changing the reassignment operator `=` on line 5 to the equality comparison operator `==`. This will only make `valid_series?` return true if `odd_count` is equal to the integer `3`. Fixed code:

```ruby
def valid_series?(nums)
  return false if nums.sum != 47

  odd_count = nums.count { |n| n.odd? }
  odd_count == 3 ? true : false
end

p valid_series?([5, 6, 2, 7, 3, 12, 4, 8])        # should return true
p valid_series?([1, 12, 2, 5, 16, 6])             # should return false
p valid_series?([28, 3, 4, 7, 9, 14])             # should return false
p valid_series?([20, 6, 9, 4, 2, 1, 2, 3])        # should return true
p valid_series?([10, 6, 19, 2, 6, 4])             # should return false
```

### Official Solution

```ruby
def valid_series?(nums)
  return false if nums.sum != 47

  odd_count = nums.count { |n| n.odd? }
  odd_count == 3 ? true : false
end
```

On line 5 of our original code, we mistakenly performed assignment rather than comparison. `==` performs equality comparison and returns a Boolean, while `=` is used for variable assignment and returns the assigned value. In this case the assigned value was 3, which will always be evaluated as a truthy condition in our ternary operator. The method therefore returned `true` for all series of numbers that sum to 47, irrespective of how many odd numbers it contains.

## Ex4 (Redo)

### My Solution

On line 7 of the original code, string concatenation is mistakenly done instead of array concatenation. Method `String#+` is called on the `words[i]`'s string with `reversed_words`' array. Since the `reversed_words` does not reference a string, the program stops and raises the error here. Fix the problem by wrapping `words[i]` as the single element of a a new array, which will make the expression on line 7 reassign local variable `reversed_words` to the new array returned from the array concatenation.

Fixed code:

```ruby
def reverse_sentence(sentence)
  words = sentence.split(' ')
  reversed_words = []

  i = 0
  while i < words.length
    reversed_words = [words[i]] + reversed_words
    i += 1
  end

  reversed_words.join(' ')
end

p reverse_sentence('how are you doing')
# expected output: 'doing you are how'
```

### Official Solution

```ruby
def reverse_sentence(sentence)
  words = sentence.split(' ')
  reversed_words = []

  i = 0
  while i < words.length
    reversed_words = [words[i]] + reversed_words
    i += 1
  end

  reversed_words.join(' ')
end
```

Both `String` and `Array` have a `+` method (`String#+` and `String#+`). The former concatenates the two strings, the latter concatenates two arrays. On line 7 we mixe these two types: `words[i]` is a string and `reversed_words` is an array. Recall that `words[i] + reversed_words` is syntactic sugar for `words[i].+(reversed_words)`; we invoked `String#+` with an array as argument, causing a `TypeError` to be raised.

In oreder to resolve this type mismatch, we can simply turn `words[i]` into a one-element array as shown in the solution. Alternatively, we could also use the `Array#unshift` method to prepend the String object onto the front of our array:

```ruby
def reverse_sentence(sentence)
  words = sentence.split(' ')
  reversed_words = []

  i = 0
  while i < words.length
    reversed_words.unshift(words[i])
    i += 1
  end

  reversed_words.join(' ')
end
```

## Ex5 (Redo)

### My Solution

On line 34 in the original code, the `sum` call on the array `remaining_cards` raises an error. `remaining_cards` still has the original unaffected elements that was not affected by the non-destructive `map` call on line 30.

This problem can be fixed by swapping out that `map` call with a call to the destructive `map!` call which will mutate the original array referenced by `remaining_cards`.

On line 3, local variable `deck` is set a hashmap of 4 entries, each of which whose value is a reference to the array referenced by local variable `cards` on line 1. Any mutating any of the values of the entries in `deck` affects all of them since all values reference the same array. The `cards` referenced on lines 22 and 23 in the `do..end` block passed to the `each` call references the original array set to `cards` on line 1. The block shuffles the same array 4 times and removes 4 elements from it via the destructive `shuffle!` and `pop` calls on lines 23 and 24. After the `each` call, `cards` points to an array of `13 - 4 => 9` elements. Assuming the first problem has been fixed, calling `reduce` call with a `do..end` block that spans lines 29 thru 35 computes the score of the same 9-sized array 4 times; thus the summed score of `9 * 4 => 36` cards is computed instead of the correct sum score of `13 * 4 - 4 => 52 - 4 => 48` cards.

The second problem can be fixed by setting each value of the 4 entries in the `deck` hash on line 3 to a complete and separate copy of `cards`' array. In this case, appending a call to `Array#[]` with the range `0..-1` to each `cards` reference from lines 3 thru 6 does the job.

Fixed code:

```ruby
cards = [2, 3, 4, 5, 6, 7, 8, 9, 10, :jack, :queen, :king, :ace]

deck = { :hearts   => cards[0..-1],
         :diamonds => cards[0..-1],
         :clubs    => cards[0..-1],
         :spades   => cards[0..-1] }

def score(card)
  case card
  when :ace   then 11
  when :king  then 10
  when :queen then 10
  when :jack  then 10
  else card
  end
end

# Pick one random card per suit

player_cards = []
deck.keys.each do |suit|
  cards = deck[suit]
  cards.shuffle!
  player_cards << cards.pop
end

# Determine the score of the remaining cards in the deck

sum = deck.reduce(0) do |sum, (_, remaining_cards)|
  remaining_cards.map! do |card|
    score(card)
  end

  sum += remaining_cards.sum
end

puts sum
```

### Official Solution

```ruby
cards = [2, 3, 4, 5, 6, 7, 8, 9, 10, :jack, :queen, :king, :ace]

deck = { :hearts   => cards.clone,
         :diamonds => cards.clone,
         :clubs    => cards.clone,
         :spades   => cards.clone }

def score(card)
  case card
  when :ace   then 11
  when :king  then 10
  when :queen then 10
  when :jack  then 10
  else card
  end
end

# Pick one random card per suit

player_cards = []
deck.keys.each do |suit|
  cards = deck[suit]
  cards.shuffle!
  player_cards << cards.pop
end

# Determine the score of the remaining cards in the deck

sum = deck.reduce(0) do |sum, (_, remaining_cards)|
  scores = remaining_cards.map do |card|
    score(card)
  end

  sum += scores.sum
end

puts sum
```

The error message looks something like this:

```
example.rb:34:in `+': :jack can't be coerced into Integer (TypeError)
```

This means that when suming up the card scores with `remaining_cards.sum` on line 34, the `remaining_cards` array still contains `:jack` instead of its score `10`. Your error message may vary slightly, nameing another face card instead.

This is because we use `Array#map` on lines 30-32 to map the cards to their scores. `map` returns a new array with the scores, but we never use that new array. On line 34, `remaining_cards` still references the original array containing both integers representing numbered cards, and symbols representing face cards.

The solution is to assign the new array returned by `map` to a variable (`scores`) and invoke `sum` on that new array of integers. Now our program successfully returns a sum. Let's check out whether the sum is correct.

In order to check the final sum, let's add a test case: The sum of the remaining cards in the deck should be the total sum of the deck when it's complete minus the sum of `player_cards`.

```ruby
total_sum = 4 * [2, 3, 4, 5, 6, 7, 8, 9, 10, 10, 10, 10, 11].sum
player_sum = player_cards.map { |card| score(card) }.sum

puts(sum == total_sum - player_sum) #=> false
```

Let's inspect what is happening by firing up `pry` with two break points: one before we collect the player's cards, and one at the end of each iteration.

```ruby
require 'pry'

# Code ommited

# Pick one random card per suit

binding.pry

player_cards = []
deck.keys.each do |suit|
  cards = deck[suit]
  cards.shuffle!
  player_cards << cards.pop

  binding.pry
end
```

The initial card deck is as expected:

```
pry(main)> deck
=> { :hearts   => [2, 3, 4, 5, 6, 7, 8, 9, 10, :jack, :queen, :king, :ace],
     :diamonds => [2, 3, 4, 5, 6, 7, 8, 9, 10, :jack, :queen, :king, :ace],
     :clubs    => [2, 3, 4, 5, 6, 7, 8, 9, 10, :jack, :queen, :king, :ace],
     :spades   => [2, 3, 4, 5, 6, 7, 8, 9, 10, :jack, :queen, :king, :ace]}
```

After the first iteration, you see something like the following (the details will differe for you):

```
pry(main)> player_cards
=> [2]

pry(main)> deck
=> {:hearts   => [8, :ace, :king, :queen, 10, 5, 9, 7, :jack, 4, 6, 3],
    :diamonds => [8, :ace, :king, :queen, 10, 5, 9, 7, :jack, 4, 6, 3],
    :clubs    => [8, :ace, :king, :queen, 10, 5, 9, 7, :jack, 4, 6, 3],
    :spades   => [8, :ace, :king, :queen, 10, 5, 9, 7, :jack, 4, 6, 3]}
```

The important thing to notice here is that although we intended to apply `shuffle!` and `pop` to only onf othe arrays (returned by `deck[suit]`), these method invocations impact all four arrays; they are all shuffle in the exact same way and they all miss the player cad we picked (`2` in this case).

This is because on lines 3-6 we assigned the same array object to each suit. The value associated with each key in our `deck` hash is thus the same object. When we mutate this object using `shuffle!` and `pop`, each value in our `deck` is afected by that mutation.

The most straightforward solution is to clone the `cards` array when adding it to the deck on lines 3-6.

## Ex6 (Redo)

### My Solution

The `SystemStackError` error means the Ruby interpreter ran out of (stack) memory when trying to run the original code. Method `move` is a recursive method which means it calls itself. However, `move` has no base case or exit condition that stops it from calling itself. Thus, any call to `move` results in `move` calling itself continuously until the Ruby interpreter runs out of memory space.

The error can be fixed by adding a base case to `move` that causes it to explictly return early if some condition has been met. In this case, `move` should stop when its parameter `n`'s value reaches `0` to get the desired result.

Fixed code:

```ruby
def move(n, from_array, to_array)
  return if n <= 0
  to_array << from_array.shift
  move(n - 1, from_array, to_array)
end

# Example

todo = ['study', 'walk the dog', 'coffee with Tom']
done = ['apply sunscreen', 'go to the beach']

move(2, todo, done)

p todo # should be: ['coffee with Tom']
p done # should be: ['apply sunscreen', 'go to the beach', 'study', 'walk the dog']
```

### Official Solution

```ruby
def move(n, from_array, to_array)
  return if n == 0
  to_array << from_array.shift
  move(n - 1, from_array, to_array)
end
```

We want to stop as soon as we have moved `n` elements from one array to the other, i.e. when `n == 0`. The code we have added on line 2 of the solution tells Ruby to `return` from the method when that condition is met.

## Ex7 (Redo)

### My Solution

On line 18, `neutralize` is called with a string which binds to the method's parameter `sentence`. On line 2, `words` is assigned the array of strings `['These', 'dull', 'boring', 'cards', 'are', 'part', 'of', 'a', 'chaotic', 'board', 'game.']` returned from the `split` call on `sentence`'s string. `each` is called on `words` with a `do..end` block that removes in-place the current iterated string `word` in `words`. When an element is removed in-place from `words`, all elements after the removed element are shifted left. This means the next block call by `each` binds the `word` block parameter to the element after the element that originally followed the removed element; the effect is that the next immediate element is skipped by the loop created by the `each` call. In this case, since `'dull'` is removed in-place, its next element `'boring'` is skipped over in the next iteration for `'cards'`, which means `'boring'` cannot be removed from `words`.

To fix the problem, reassign `words` to a new array of filtered strings instead of mutating the array while looping over it. In this case, we can reassign to `words` to a new array where each element makes the negated return value of calling `negative?` return true The following code is one way to address this problem:

```ruby
def neutralize(sentence)
  words = sentence.split(' ')
  words = words.filter do |word|
    !negative?(word)
  end

  words.join(' ')
end

def negative?(word)
  [ 'dull',
    'boring',
    'annoying',
    'chaotic'
  ].include?(word)
end

puts neutralize('These dull boring cards are part of a chaotic board game.')
# Expected: These cards are part of a board game.
# Actual: These boring cards are part of a board game.
```

### Official Solution

```ruby
def neutralize(sentence)
  words = sentence.split(' ')
  words.reject! { |word| negative?(word) }
  words.join(' ')
end

def negative?(word)
  ['dull', 'boring', 'annoying', 'chaotic'].include?(word)
end

puts neutralize('These boring cards are part of a chaotic board game.')
#=> These cards are part of a board game.
```

While iterating over an array, avoid mutating it from within the block. Instead you can use the Array methods `select` or `reject / reject!`.

## Ex8 (Redo)

### My Solution

The problem is that the local variable `password` in the outermost scope is not directly accessible in either method `set_password` or `verify_password` due to local variable scoping rules with respect to method definitions. `password` on line 6 refers to the initialisation and assignment of a method local variable in `set_password`, while `password` on line 14 refers to an undefined method local variable in `verify_password`. Thus, line 14 stops the program and raises an error since.

The problem can be fixed by reassigning `password` to the return value of calling `set_password` on line 22 and modifying `verify_password` to be able to access the outer `password`. Add the parameter `password` to `verify_password` and pass `password` as an argument to the `verify_password` call on line 25. Optionally, line 6 in `set_password` can be removed. The program runs without error and behaves as expected after the fixes.

Fixed code:

```ruby
password = nil

def set_password
  puts 'What would you like your password to be?'
  new_password = gets.chomp
end

def verify_password(password)
  puts '** Login **'
  print 'Password: '
  input = gets.chomp

  if input == password
    puts 'Welcome to the inside!'
  else
    puts 'Authentication failed.'
  end
end

if !password
  password = set_password
end

verify_password(password)
```

### Official Solution

```ruby
password = nil

def set_password
  puts 'What would you like your password to be?'
  new_password = gets.chomp
  new_password
end

def verify_password(password)
  puts '** Login **'
  print 'Enter your password: '
  input = gets.chomp

  if input == password
    puts 'Welcome to the inside!'
  else
    puts 'Authentication failed.'
  end
end

if !password
  password = set_password
end

verify_password(password)
```

When we initially run the code, the following exception is raised:

```
example.rb:14:in `verify_password': undefined local variable or method `password' for main:Object (NameError)
```

This tells us that within the `verify_password` method, Ruby cannot access the variable `password` that we initialized on line 1. Recall that methods create their own scope, and that within a method we do not have access to local variables in outer scopes.

We can address this error by passing `password` as an argument to the `verify_password` method.

Next, we find that the `set_password` method is not updating our password as expected. The reason is that the variable `password`, to which we assign the value referenced by `new_password` on line 6, is local only to the `set_password` method, and this variable assignment does not impact the local variable `password` initialized on line 1.

We can solve this issue by re-assigning `password` to the return value of `set_password` on line 22. We can then also remove the unnecessary variable assignment on line 6 within `set_password`.

Of course in reality you should never show passwords as plain text or store them unencrypted. You will elarn more about passwords in course 170.

## Ex9 (Redo)

### My Solution

The problem with the original code is two-fold. First, if the correct number is inputted, the game will still continue until the correct number is inputted for another `max_attempts` times where `max_attemps` is a parameter of `guess_number` that references an integer. Second, if the wrong number is inputted, method `guess_number` calls itself which spawns a new game with 1 attempt and with a possibly different winning number. Depending on chance, the game may continue forever as every wrong guess spawns a new game when `guess_number` calls itself on line 28.

The problem can be fixed. First, `guess_number`'s loop should exit if the correctly guessed number referenced by `guess` is input on the line after line 22. Second, the recursive call to `guess_number` in `guess_number` on line 28 should be removed.

Fixed code:

```ruby
def valid_integer?(string)
  string.to_i.to_s == string
end

def guess_number(max_number, max_attempts)
  winning_number = (1..max_number).to_a.sample
  attempts = 0

  loop do
    attempts += 1
    break if attempts > max_attempts

    input = nil
    until valid_integer?(input)
      print 'Make a guess: '
      input = gets.chomp
    end

    guess = input.to_i

    if guess == winning_number
      puts 'Yes! You win.'
      break
    else
      puts 'Nope. Too small.' if guess < winning_number
      puts 'Nope. Too big.'   if guess > winning_number
    end
  end
end

guess_number(10, 3)
```

### Official Solution

```ruby
def valid_integer?(string)
  string.to_i.to_s == string
end

def guess_number(max_number, max_attempts)
  winning_number = (1..max_number).to_a.sample
  attempts = 0

  loop do
    attempts += 1
    break if attempts > max_attempts

    input = nil
    until valid_integer?(input)
      print 'Make a guess: '
      input = gets.chomp
    end

    guess = input.to_i

    if guess == winning_number
      puts 'Yes! You win.'
      break
    else
      puts 'Nope. Too small.' if guess < winning_number
      puts 'Nope. Too big.'   if guess > winning_number

      # simply re-enter the loop for the next attempt
    end
  end
end

guess_number(10, 3)
```

In order to stop looping when the use has guessed correctly, we add a `break` statement on line 23.

The unexpected behavior exhibited when a user guesses wrongly is due to the invocation of `guess_number` on line 28 of the original code. As seen in the example debugging output provided, doing so determines a new `winning_number` and resets `attempts` to `0` each time we re-enter the method.

It's not necessary to invoke `guess_number` from within the loop. We can rely on our loop to continue prompting the user until they have either guessed correctly or run out of guesses. Therefore we can simply remove the invocation of `guess_number` from line 28.

## Ex10 (Redo)

### My Solution

The problem is that the argument passed to the `Math::log` call on line 26 is always an integer. In Ruby, if both divisor and dividend values are integers in a `/` division operation, this results in integer division; this means the quotient will always be a truncated integer and not include the decimal part of the division. Thus, division operations like `2 / 3` that result in decimal quotients less than `1` will truncate and return `0`.

To fix this problem, make either the divisor or quotient of a `/` division operation a float to make the quotient return a float. In this case, chaining a method call to `Integer#to_f` on the `length` call on line 23 will force the division operation on line 26 to return a float instead of an integer, thereby resolving the problem.

Fixed code:

```ruby
# Omitted code...

def idf(term, documents)
  number_of_documents = documents.length.to_f
  number_of_documents_with_term = documents.count { |d| tf(term, d) > 0 }

  Math.log(number_of_documents / number_of_documents_with_term)
end

# Omitted code...
```

### Official Solution

```ruby
def idf(term, documents)
  number_of_documents = documents.length
  number_of_documents_with_term = documents.count { |d| tf(term, d) > 0 }

  Math.log(number_of_documents.fdiv(number_of_documents_with_term))
end
```

In the definition of our original `idf` method, we perform division on two values which will both be integers. The division will therefore be performed as integer division, meaning it will produce an integer value, namely the integer part of the result without the remainder (for example, `1/2` will evaluate to `0` instead of `0.5`).

Let's take a closer look at our first test case. The term `"cat"` occurs in 2 or 3 documents, which returns an `idf` value of `Math.log(3/2) => Math.log(1) => 0`.

In order to return a float value, we must perform float division. We can do this by either converting one or both values to a floating point number using `Integer#to_f`, or by using the `Numeric#fdiv` method as seen in the above solution.

The `idf` value of `"cat"` is then correctly calculated as `Math.log(3.fdiv(2)) => Math.lof(1.5) => 0.4054651081081644`. The `tfidf` method multiplies the returned `idf` by the `tf` value for `document1` (which is 3, because `"cat"` occurs three times in that document) and returns the expected output of ~1.2 for the first example.

## Ex11 (Stuck & Redo)

### My Solution

Due to order precedence with regards to passing arguments to method calls, the `do..end` block is passed to the `p` call rather than the `sort` call. Thus, the `sort` call does not use the block when creating and returning a new sorted array based on `arr`'s array.

To fix this problem, call `p` with explicit parentheses to wrap around the `sort` call with its block. This passes the block to the `sort` call which returns the desired sorted array output by `p`.

Fixed code:

```ruby
arr = ["9", "8", "7", "10", "11"]
p(
  arr.sort do |x, y|
    y.to_i <=> x.to_i
  end
)

# => ["11", "10", "7", "8", "9"]
```

### Official Solution

Solution 1:

```ruby
p(
  arr.sort do |x, y|
    y.to_i <=> x.to_i
  end
)
```

Solution 2:

```ruby
p arr.sort { |x, y| y.to_i <=> x.to_i }
```

The main reason why the output was unexpected is because when you use `do...end` block, `p` and `arr.sort` bind more tightly due to Ruby's precedence, so you are passing the block to the return value of `p arr.sort`. It's as though you wrote:

```ruby
(p arr.sort) do |x, y|
  y.to_i <=> x.to_i
end
```

In Josh's case, the block is ignored since the return value of `p arr.sort` is not a method call.

One way to get around this precedence issue is to use parentheses as in Solution 1. You can also use braces instead of `do...end` as in Solution 2.
