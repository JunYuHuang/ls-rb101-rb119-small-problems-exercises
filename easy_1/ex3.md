# My Solution

```ruby
def stringy(length)
  return '' if length < 1 || !length.is_a?(Integer)

  res = ''
  length.times do |i|
    char = i % 2 == 0 ? '1' : '0'
    res.concat(char)
  end

  res
end

puts stringy(6) == '101010'
puts stringy(9) == '101010101'
puts stringy(4) == '1010'
puts stringy(7) == '1010101'
```
