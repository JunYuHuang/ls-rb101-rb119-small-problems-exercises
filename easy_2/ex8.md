# My Solution

```ruby
int = 0
loop do
  puts ">> Please enter an integer greater than 0:"
  int = gets.chomp.to_i

  break if int > 0
  puts ">> #{int} is not an integer greater than 0. Please try again."
end

option = ""
loop do
  puts ">> Enter 's' to compute the sum, 'p' to compute the product."
  option = gets.chomp

  break if option == 's' || option == 'p'
  puts ">> '#{option}' is not a valid option. Please try again."
end

if option == 's'
  sum = (1..int).sum
  puts "The sum of the integers between 1 and #{int} is #{sum}."
else
  product = (1..int).inject(:*)
  puts "The product of the integers between 1 and #{int} is #{product}."
end
```
