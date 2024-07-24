# My Solution

```ruby
print "What is your name? "
name = gets.chomp

msg = name[-1] == '!' ?
  "HELLO #{name[0...(name.size - 1)].upcase}. WHY ARE WE SCREAMING?" :
  "Hello #{name}."

puts msg
```
