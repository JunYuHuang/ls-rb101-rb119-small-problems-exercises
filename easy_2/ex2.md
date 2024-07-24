# My Solution

```ruby
def sq_meter_to_sq_feet(sq_meters)
  sq_meters * 10.7639
end

puts "Enter the length of the room in meters"
length = gets.chomp.to_f

puts "Enter the width of the room in meters:"
width = gets.chomp.to_f

sq_meters = length * width
sq_feet = sq_meter_to_sq_feet(sq_meters)
puts "The area of the room is #{sq_meters} square meters (#{sq_feet.round(2)} square feet)."
```
