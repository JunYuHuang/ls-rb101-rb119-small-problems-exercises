# My Solution

```ruby
nums = []
last_num = -1

def numbered(num)
  return "1st" if num == 1
  return "2nd" if num == 2
  return "3rd" if num == 3
  "#{num}th"
end

for i in 1..6
  puts ">> Enter the #{numbered(i)} number:"
  if i == 6
    last_num = gets.chomp.to_i
  else
    nums << gets.chomp.to_i
  end
end

if nums.include?(last_num)
  puts "The number #{last_num} appears in #{nums}."
else
  puts "The number #{last_num} does not appear in #{nums}."
end
```
