# My Solution

```ruby
require 'date'

print "What is your age? "
age = gets.chomp.to_i

print "At what age would you like to retire? "
retire_age = gets.chomp.to_i

work_years = retire_age - age
current_year = Date.today.year
retire_year = current_year + work_years
puts "\nIt's #{current_year}. You will retire in #{retire_year}."
puts "You have only #{work_years} years of work to go!"
```
