# My Solution

```ruby
def triangle(length)
  return if length < 1 || !length.is_a?(Integer)

  length.times do |i|
    spaces = length - (i + 1)
    stars = i + 1
    puts "#{' ' * spaces}#{'*' * stars}"
  end
end

triangle(5)
=begin
Prints:

    *
   **
  ***
 ****
*****
=end

triangle(9)
=begin
Prints:

        *
       **
      ***
     ****
    *****
   ******
  *******
 ********
*********
=end
```
