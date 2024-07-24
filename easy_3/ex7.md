# My Solution

```ruby
def palindrome?(str)
  return false if !str.is_a?(String) || str.size < 1

  l = 0
  r = str.size - 1
  while l <= r
    return false if str[l] != str[r]
    l += 1
    r -= 1
  end

  true
end

palindrome?('madam') == true
palindrome?('Madam') == false          # (case matters)
palindrome?("madam i'm adam") == false # (all characters matter)
palindrome?('356653') == true
```
