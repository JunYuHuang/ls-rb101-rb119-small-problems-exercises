# My Solution

```ruby
def center_of(str)
  return '' if !str.is_a?(String) || str.size < 1

  str_size = str.size
  mid_pos = str_size / 2
  str_size.odd? ? str[mid_pos] : str[(mid_pos - 1)..mid_pos]
end

center_of('I love ruby') == 'e'
center_of('Launch School') == ' '
center_of('Launch') == 'un'
center_of('Launchschool') == 'hs'
center_of('x') == 'x'
```
