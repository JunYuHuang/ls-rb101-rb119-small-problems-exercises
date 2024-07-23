# My Solution

```ruby
def calculate_bonus(salary, is_bonus)
  return 0 if salary < 0 || !salary.is_a?(Integer)
  return 0 unless is_bonus

  salary / 2
end

puts calculate_bonus(2800, true) == 1400
puts calculate_bonus(1000, false) == 0
puts calculate_bonus(50000, true) == 25000
```
