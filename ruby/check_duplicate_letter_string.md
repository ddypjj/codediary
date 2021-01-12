An isogram is a word that has no repeating letters, consecutive or non-consecutive.

Implement a function that determines whether a string that contains only letters is an isogram.

Assume the empty string is an isogram. Ignore letter case.

```ruby
def is_isogram(string)
  string.downcase.chars.uniq == string.downcase.chars
end
```

```ruby
string = 'helLo'
string.downcase.chars.uniq # => ["h", "e", "l", "o"]
string.downcase.chars # => ["h", "e", "l", "l", "o"]
string.downcase.chars.uniq == string.downcase.chars # => false
```