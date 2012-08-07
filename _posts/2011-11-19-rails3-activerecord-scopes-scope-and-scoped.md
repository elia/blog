Assume you have the following AR scope:

```ruby
class Hat < ActiveRecord::Base
  def self.except_colors *colors
    where('color NOT IN ?', colors)
  end
end
```

It surely works… unless you pass no colors

```ruby
Hat.except_colors
```

So you'll tend to do something like:

```ruby
def Hat.except_colors *colors
  colors.any? ? where('color NOT IN ?', colors) : self
end
```

and will work until you'll chain it to another scope and pass no arguments, like:

```ruby
Hat.cowboy.except_colors
```

## Solution

Since seems that each scope is evaluated by the class itself and not by the scope chain, what you really wanted is to use scoped:

```ruby
def self.except_colors *colors
  # self = Hat
  colors.any? ? where('color NOT IN ?', colors) : scoped
end
```

