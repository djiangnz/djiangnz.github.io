---
layout: post
title: Ruby Cheatsheet
description:
summary:
tags: cheatsheet ruby
---

[ruby online](https://replit.com/languages/ruby)

> By convention, use snake_case for variable names.

# Falsey

- `false`
- `nil`

# Data Types

```ruby
t = 42 # Integer
t = "Sammy" # String
t = :sammy # Symbol, Symbols are immutable
t = true # boolean
t # nil
```

## Identifying Data Types

```ruby
42.class # Integer
(42.2).class # Float
["Sammy", "Shark"].class # Array
true.class # TrueClass
nil.class # NilClass

42.kind_of?(Integer) # true

if data.kind_of? String
  data = data.to_f
end

if data.is_a? String
  data = data.to_f
end
```

# String interpolation

```ruby
"I can #{placeholder} when using double quoted strings"

'hello ' + 'world'  # "hello world"
'hello ' + 3        # TypeError: can't convert Fixnum into String
'hello ' + 3.to_s   # "hello 3"
"hello #{3}"        # "hello 3"
'hello ' * 3        # "hello hello hello "
'hello' << ' world' # "hello world"
```

# Arrays

```ruby
# Defining
array = [1, 2, 3, 4, 5] # [1, 2, 3, 4, 5]
array = %w[foo bar baz] # ["foo", "bar", "baz"]

#Working with arrays
array[0] / array.first # 1

array[-1] / array.last # 5

array[2, 3] # [3, 4, 5]

array[1..3] # [2, 3, 4]

array[12] # nil

# There is also the shorthand block syntax. It's most useful when you need
# to call a simple method on all array items.
upcased = ['Watch', 'these', 'words', 'get', 'upcased'].map(&:upcase)
sum = [1, 2, 3, 4, 5].reduce(&:+)
```

# Hashes

```ruby
# Defining
hash = { 'color' => 'green', 'number' => 5 }
hash.keys #=> ['color', 'number']

# Hashes can be quickly looked up by key.
hash['color'] #=> "green"
hash['number'] #=> 5

# Asking a hash for a key that doesn't exist returns nil.
hash['nothing here'] #=> nil

# When using symbols for keys in a hash, you can use an alternate syntax.

hash = { :defcon => 3, :action => true }
hash.keys #=> [:defcon, :action]

hash = { defcon: 3, action: true }
hash.keys #=> [:defcon, :action]

# Check existence of keys and values in hash
hash.key?(:defcon) #=> true
hash.value?(3) #=> true
```

# Loops

```ruby
(1..5).each { |counter| puts "iteration #{counter}" }

(1..5).each do |counter|
  puts "iteration #{counter}"
end

for counter in 1..5
  puts "iteration #{counter}"
end

array.each do |element|
  puts "#{element} is part of the array"
end

hash.each do |key, value|
  puts "#{key} is #{value}"
end

array.each_with_index do |element, index|
  puts "#{element} is number #{index} in the array"
end

counter = 1
while counter <= 5 do
  puts "iteration #{counter}"
  counter += 1
end
```

# Cases

```ruby
grade = 82
case grade
when 90..100
  puts 'Hooray!'
when 80...90
  puts 'OK job'
else
  puts 'You failed!'
end
```

# Exception handling

```ruby
begin
  # Code here that might raise an exception
  raise NoMemoryError, 'You ran out of memory.'
rescue NoMemoryError => exception_variable
  puts 'NoMemoryError was raised', exception_variable
rescue RuntimeError => other_exception_variable
  puts 'RuntimeError was raised now'
else
  puts 'This runs if no exceptions were thrown at all'
ensure
  puts 'This code always runs no matter what'
end
```

# Methods

```ruby
def double(x)
x \* 2
end

# Blocks can be converted into a 'proc' object
def guests(&block)
  block.class #=> Proc
  block.call(4)
end

guests { |n| "You have #{n} guests." }
```

# yield

```ruby
def surround
  puts '{'
  yield
  puts '}'
end

surround { puts 'hello world' }
```

# Classes

## Classes

```ruby
class Human

  # A class variable.
  @@species = 'H. sapiens'

  # Basic initializer
  def initialize(name, age = 0)
    @name = name
    @age = age
  end

  # Basic setter method
  def name=(name)
    @name = name
  end

  # Basic getter method
  def name
    @name
  end

  # The above functionality can be encapsulated using the attr_accessor method as follows.
  attr_accessor :name

  # Getter/setter methods can also be created individually like this.
  attr_reader :name
  attr_writer :name

  # A class method
  def self.say(msg)
    puts msg
  end
end
```

## Variables

```ruby
# Variables that start with $ have global scope.
$var = "I'm a global var"
defined? $var #=> "global-variable"

# Variables that start with @ have instance scope.
@var = "I'm an instance var"
defined? @var #=> "instance-variable"

# Variables that start with @@ have class scope.
@@var = "I'm a class var"
defined? @@var #=> "class variable"

# Variables that start with a capital letter are constants.
Var = "I'm a constant"
defined? Var #=> "constant"
```

## Subclasses

```ruby
class Worker < Human
end
```

## Modules

```ruby
module ModuleExample
  def foo
    'foo'
  end
end

# Including modules binds their methods to the class instances.
class Person
  include ModuleExample
end
Person.new.foo #=> "foo"

# Extending modules binds their methods to the class itself.
class Book
  extend ModuleExample
end
Book.foo #=> "foo"

# Callbacks are executed when including and extending a module
module ConcernExample
  def self.included(base)
    base.extend(ClassMethods)
    base.send(:include, InstanceMethods)
  end

  module ClassMethods
    def bar
      'bar'
    end
  end

  module InstanceMethods
    def qux
      'qux'
    end
  end
end

class Something
  include ConcernExample
end

Something.bar     #=> "bar"
Something.qux     #=> NoMethodError: undefined method `qux'
Something.new.bar #=> NoMethodError: undefined method `bar'
Something.new.qux #=> "qux"
```

# Syntactic Sugar

```ruby
10 * 5 is 10.* 5
100.methods.include?(:/) #=> true

array[0] is array.[] 0
```

# Links

- [learnxinyminutes](https://learnxinyminutes.com/docs/ruby/)
