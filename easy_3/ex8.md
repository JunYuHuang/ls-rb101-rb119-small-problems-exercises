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

def real_palindrome?(str)
  palindrome?(str.downcase.gsub(/[^a-z1-3]/, ''))
end

real_palindrome?('madam') == true
real_palindrome?('Madam') == true           # (case does not matter)
real_palindrome?("Madam, I'm Adam") == true # (only alphanumerics matter)
real_palindrome?('356653') == true
real_palindrome?('356a653') == true
real_palindrome?('123ab321') == false
```
