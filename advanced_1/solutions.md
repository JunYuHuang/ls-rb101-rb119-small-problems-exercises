# Advanced 1 Solutions

## Ex1 (skipped)

### My Solution

todo

### Official Solution

todo

## Ex2 (Redo?)

### My Solution

Notes:

- problem
  - input:
    - `n`: positive odd integer greater than or equal to 7
  - output:
    - none
  - side effects
    - prints a 'star' in the console
  - `n`-sized star meaning
    - 4 lines that intersect at a middle point at the center of each line
    - occupies a square space with
      - `n` rows or chars wide
      - `n` cols or chars high
    - each line has `n` `*` chars
    - line distribution
      - 1 horizontal
      - 1 vertical
      - 1 positive diagonal
      - 1 negative diagonal
- examples
  - todo
- data structures
  - string to store each row printed
  - array to store each row
- algorithm
  - set `rows` to empty array
  - set `row` to string of `n` ` ` chars
  - push copy of `row` to `rows` `n` times
  - set horizontal line to all `*` chars
    - set `mid_row_index` to integer `n` int divided by 2 minus 1
    - set all chars in string `rows[mid_row_index]` to a `*` char
  - set vertical line to all `*` chars
    - set `mid_col_index` to (`n` int divided by 2)
    - set the char at index `mid_col_index` in each string in `rows` to a `*` char
  - set positive diagonal line to all `*` chars
    - set `row_index` to integer `n - 1` (last index in `rows`)
    - set `col_index` to integer `0` (first index in each string)
    - while `row_index` >= `0` and `col_index` < `n`,
      - set char in string `rows[row_index][col_index]` to `*` char
      - decrement `row_index`
      - increment `col_index`
  - set negative diagonal line to all `*` chars
    - set `row_index` to integer `0` (first index in `rows`)
    - set `col_index` to integer `0` (first index in each string)
    - while `row_index` < `n` and `col_index` < `n`,
      - set char in string `rows[row_index][col_index]` to `*` char
      - increment `row_index`
      - increment `col_index`
  - for each string `row` in `rows`,
    - print `row` to the console with a new line char

```ruby
def star(n)
  return if !n.is_a?(Integer) || n < 7 || n.even?

  # set empty string array of `n`-sized each of `n`-sized strings
  rows = []
  n.times { rows << (" " * n) }

  # set horizontal line
  mid_row_index = n / 2
  rows[mid_row_index] = "*" * n

  # set vertical line
  mid_col_index = n / 2
  for row in rows
    row[mid_col_index] = "*"
  end

  # set positive diagonal line
  row_index = n - 1
  col_index = 0
  while row_index >= 0 && col_index < n
    rows[row_index][col_index] = "*"
    row_index -= 1
    col_index += 1
  end

  # set negative diagonal line
  row_index = 0
  col_index = 0
  while row_index < n && col_index < n
    rows[row_index][col_index] = "*"
    row_index += 1
    col_index += 1
  end

  # print star
  puts
  for row in rows
    puts row
  end
  puts
end

star(7)

# *  *  *
#  * * *
#   ***
# *******
#   ***
#  * * *
# *  *  *

star(9)

# *   *   *
#  *  *  *
#   * * *
#    ***
# *********
#    ***
#   * * *
#  *  *  *
# *   *   *
```

### Official Solution

```ruby
def print_row(grid_size, distance_from_center)
  number_of_spaces = distance_from_center - 1
  spaces = ' ' * number_of_spaces
  output = Array.new(3, '*').join(spaces)
  puts output.center(grid_size)
end

def star(grid_size)
  max_distance = (grid_size - 1) / 2
  max_distance.downto(1) { |distance| print_row(grid_size, distance) }
  puts '*' * grid_size
  1.upto(max_distance) { |distance| print_row(grid_size, distance) }
end
```

## Ex3 (Skip)

### My Solution

todo

### Official Solution

todo

## Ex4 (Skip)

### My Solution

todo

### Official Solution

todo

## Ex5 (Skip)

### My Solution

todo

### Official Solution

todo

## Ex6 (Redo?)

### My Solution

The code can be fixed by setting the `elsif` clause's test to the expression `array.size > 1` as shown below:

```ruby
def my_method(array)
  if array.empty?
    []
  elsif array.size > 1
    array.map do |value|
      value * value
    end
  else
    [7 * array.first]
  end
end

p my_method([]) == []
p my_method([3]) == [21]
p my_method([3, 4]) == [9, 16]
p my_method([5, 6, 7]) == [25, 36, 49]
```

In the original code, method `my_method` returns the `elsif`'s test's last executed line if the `if`'s test expression is falsy and the `else` clause's body is never returned. In Ruby, the `elsif` clause is a like a method. Since there is no test expression on the same line as the `elsif`, the `elsif` uses its body defined from lines 5 - 7 as the test expression which means `elsif` has no explicit body to execute -- that is `elsif` returns `nil` if its test expression is truthy. Lines 5 - 7 is the return value of calling `#map` on method variable `array`'s referenced array with a `do..end` block, which is a new array based on `array`. Arrays in Ruby are truthy, so the `elsif` is executed and its body's expression -- `nil` is returned by `my_method` if the `if`'s test is falsy. Thus, calling `my_method` with any non-empty array always returns `nil`, as returned by the calls to `my_method` from lines 14 - 16.

### Official Solution

The program canbe fixed by changing `my_method` to read:

```ruby
def my_method(array)
  if array.empty?
    []
  elsif array.size > 1
    array.map do |value|
      value * value
    end
  else
    [7 * array.first]
  end
end
```

This bug can be difficult to find since a first reading of the code is not likely to notice the fact that `elsif` is missing a conditional expression. If you don't notice that right away, you can spend a long time looking at this program trying to figure out why it isn't working. Even once you spot the problem, it can take a few minutes just to understand why the program runs at all; if you don't run the code yourself, you may not believe that it runs.

Anyway, that missing conditional expression on the `elsif` isn't really missing -- at least not as far as the Ruby parser is concerned. When Ruby gets to the end of the `elsif` line and doesn't find a conditional expression, it's smart enough to go looking for it on the next line. Lo and behold, there's the conditional expression.

What conditional expression, you ask? Why, the one that begins with `array.map do |value|`; that's right, that `map` call (including the associated block) is a conditional expression. And, it's value is truthy. Actually the value of a `map` is the Array returned by `map`, and an Array is always truthy. Thus, any time the array is non-empty, the `elsif` branch gets executed.

What `elsif` branch? IF the `map` call is the conditional expression, where's the code that gets executed in that branch? The answer is that there is no code, but a branch doesn't need to have any code. If there is no code in a branch, then it's equivalent to the expression `nil`. This is why `my_method` always returns `nil` instead of an Array when the input isn't empty.

Fixing this is easy once the problem has been identified. It's just a matter of figuring out what conditional expression should be passed to the `elsif`. From lookking at the inputs and expected outputs, we can see that any Array of size greater than 1 should take the `elsif` path, so the correct conditional is `array.size > 1`.

Actually, there are other possible conditions you can use on the `elsif` that would work as well. `array.size > 1` is merely one reasonable answer, and perhaps the most logical.

## Ex7

### My Solution

Notes:

- problem
  - inputs:
    - `array1`: sorted integer array
      - may be empty
    - `array2`: sorted integer array
      - may be empty
  - output
    - `res`: sorted array of integers from both `array1` and `array2`
      - may have duplicate integers
      - may be empty
  - rules
    - cannot sort either `array1` or `array2`
    - cannot sort `res`
    - cannot mutate either `array1` or `array2`
    - `array1` and `array2` can be of unequal length
- examples
  - skipped
- data structures
  - array to hold `res`
- algorithm
  - set `res` to empty array
  - set `index1` to integer 0 to iterate thru each element in `array1` at this index
  - set `index2` to integer 0 to iterate thru each element in `array2` at this index
  - loop while both `index1` and `index2` point to in-bound elements in both arrays
    - if `array1[index1]` < `array2[index2]`,
      - push `array1[index1]` to `res`
      - increment `index1` by 1
    - else if `array1[index1]` >= `array2[index2]`,
      - push `array2[index2]` to `res`
      - increment `index2` by 1
  - if `index1` is still pointing to a valid element in `array1`,
    - push the remaining elements in `array1` from `index1` to its last index to `res`
  - if `index2` is still pointing to a valid element in `array2`,
    - push the remaining elements in `array2` from `index2` to its last index to `res`
  - return `res`

```ruby
def merge(array1, array2)
  res = []
  index1 = 0
  index2 = 0
  array1_size = array1.size
  array2_size = array2.size

  while index1 < array1_size && index2 < array2_size
    if array1[index1] < array2[index2]
      res.push(array1[index1])
      index1 += 1
    else
      res.push(array2[index2])
      index2 += 1
    end
  end

  if index1 < array1_size
    res.concat(array1[index1..-1])
  end

  if index2 < array2_size
    res.concat(array2[index2..-1])
  end

  res
end

p merge([1, 5, 9], [2, 6, 8]) == [1, 2, 5, 6, 8, 9]
p merge([1, 1, 3], [2, 2]) == [1, 1, 2, 2, 3]
p merge([], [1, 4, 5]) == [1, 4, 5]
p merge([1, 4, 5], []) == [1, 4, 5]
```

### Official Solution

```ruby
def merge(array1, array2)
  index2 = 0
  result = []

  array1.each do |value|
    while index2 < array2.size && array2[index2] <= value
      result << array2[index2]
      index2 += 1
    end
    result << value
  end

  result.concat(array2[index2..-1])
end
```

This obvious solution is to walk through both arrays simultaneously, keeping track of where you are in each array with appropriate indexes. We'll modify this a tiny bit by using `Array#each` to iterate through the `array`, and use an index to track our location in the `array2`.

With each iteration of `array1`, we copy all elements from `array2` that are less than or equal to teh `array1` value, incrementing our index as needed. Note that we need to be careful to not try copying any values from `array2` that aren't there. After copying these elements, we then append the current value from `array1`, and start the next iteration.

When the loops are done, we may be left with 1 or more items in `array2` that were not include in the results. The last step is to make sure they are included.

We did not write this method all in one go; it took several debugging rounds to get just the right sequence of actions. It was not easy to get right.

## Ex8

### My Solution

todo

### Official Solution

todo

## Ex9 (Skipped)

### My Solution

todo

### Official Solution

todo

## Ex10

### My Solution

todo

### Official Solution

todo
