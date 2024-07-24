# My Solution

```ruby
print "Please write word or multiple words: "
text = gets.chomp

count = text.gsub(' ', '').size
puts "There are #{count} characters in \"#{text}\"."
```
