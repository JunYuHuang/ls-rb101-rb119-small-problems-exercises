# My Solution

```ruby
def print_in_box(message)
  return unless message.is_a?(String)

  inner_length = message.size + 2
  5.times do |i|
    row = []

    if i == 0 || i == 4
      # build top and bot border rows
      row.push('+')
      inner_length.times { row.push('-') }
      row.push('+')
    elsif i == 1 || i == 3
      # build top and bot space rows
      row.push('|')
      inner_length.times { row.push(' ') }
      row.push('|')
    else
      # build message row
      row.push('|', ' ', message, ' ', '|')
    end

    # print the row
    puts(row.join)
  end
end

print_in_box('To boldly go where no one has gone before.')
=begin
Prints:

+--------------------------------------------+
|                                            |
| To boldly go where no one has gone before. |
|                                            |
+--------------------------------------------+
=end

print_in_box('')
=begin
Prints:

+--+
|  |
|  |
|  |
+--+
=end
```
