# My Solution

```ruby
def palindromic_number?(num)
  num.to_s == num.to_s.dup.reverse
end

palindromic_number?(34543) == true
palindromic_number?(123210) == false
palindromic_number?(22) == true
palindromic_number?(5) == true
```
