# My Solution

```ruby
def reversed_number(num)
  return -1 if num < 1 || !num.is_a?(Integer)

  num_copy = num.dup
  res = 0
  while num_copy != 0
    last_digit = num_copy % 10
    res = res * 10 + last_digit
    num_copy /= 10
  end

  res
end

reversed_number(12345) == 54321
reversed_number(12213) == 31221
reversed_number(456) == 654
reversed_number(12000) == 21 # No leading zeros in return value!
reversed_number(12003) == 30021
reversed_number(1) == 1
```
