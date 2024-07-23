# My Solution

```ruby
a = %w(a b c d e)

puts a.fetch(7)
# Raises an `IndexError` because index 7 is out-of-bounds for `a`
# Prints `IndexError: index 7 outside of array bounds: -5...5

puts a.fetch(7, 'beats me')
# Prints `beats me`

puts a.fetch(7) { |index| index**2 }
# Prints `49`
```
