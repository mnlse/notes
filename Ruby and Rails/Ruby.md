Author: Matthew Nielsen
License: [Creative Commons](https://creativecommons.org/licenses/by/4.0/)
# The Ruby language
## Intro
Interactive shell: `irb`
File extension: `.rb`
## Basic
Commenting is done via the hash (`#`) symbol.
~~~
# This is a comment
~~~

Print to the screen using `puts` and `print`.
~~~
puts "abc"
print "abcdefghijklmnoprstuvwxyz"
~~~
`puts` (put string) adds a newline at the end. `print` does not.

---


The **if** statement:
~~~
if condition
  body
elsif condition2
  body2
else
  body3
end
~~~

### Loops
For loops are aliases for the `.each` method.

~~~
someHash = {
  "key" => "value",
  "key2" => "value2"
}

someHash.each { |k, v| puts k }
                 ^^^^
                 k = key, v = value
~~~

Infinite loops:
~~~
loop do
  input = gets.chomp
  if input == "x"
    break
  end
end
~~~

---
Methods are similar to JavaScript and Python:
~~~
"Hello says world".split(" ").reverse.join(" ")
=> world says Hello
~~~

To get a listing of each method of an object, use the `.methods` method.
~~~
[1, 2, 3].methods
"abc".methods
~~~

### Functions
~~~
def fnc
  ...
end
~~~

## Data types
Class hierarchy of Ruby:
![text](https://i.stack.imgur.com/1taqB.png)
<sup>(right click -> view image)</sup>

Data types:
~~~
String      =>      "abc".class
Fixnum      =>      1.class
Integer     =>      1.class.superclass
Numeric     =>      1.class.superclass.superclass
Float       =>      1.337.class
Numeric     =>      1.337.class.superclass
NilClass    =>      nil.class
Hash        =>      {}.class
Symbol      =>      :hello.class
Array       =>      [].class
Range       =>      (0..13).class
~~~

## Classes
~~~
class Person
  def initialize(name)
    @name = name
  end
end

class Teenager < Person
  def speak
    puts "#{@name}: What is this Ubuntu?"
  end
end

class Adult < Person
  def speak
    puts "#{@name}: What I did on New Year's Eve? Just recompiling Gentoo."
  end
end

jack_inst = Teenager.new("jack")
jack_inst.speak

tom_inst = Adult.new("tom")
tom_inst.speak
~~~

