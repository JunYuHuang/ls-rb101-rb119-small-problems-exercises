# My Solution

```ruby
print "What is the bill? "
bill = gets.chomp.to_f

print "What is the tip percentage? "
tip_percent = gets.chomp.to_f

tip = bill * (tip_percent / 100)
total = bill + tip

puts "\nThe tip is $#{tip.round(1)}\nThe total is $#{total.round(1)}"
```
