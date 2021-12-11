# Methods, Variables and Scope

## Methods

> **Methods** are a common structure. Define code abstraction, providing specific semantic and hiding implementation.
>
> * Parameterized Block code with a name: can be used with different parametric values per invocation.

{% tabs %}
{% tab title="Simple method" %}
```ruby
def double(x)
    return x*2
end    
```
{% endtab %}

{% tab title="Alias" %}
```ruby
def spit_it_out
    puts "arghhhh!!"
end

alias arg spit_it_out
```
{% endtab %}

{% tab title="Variable length arguments" %}
```ruby
def vl_method(first, *others)
    puts "First is: " + first.to_s
    puts "Others: " + others.to_s
end

vl_method(1,2)
vl_method(1,2,3,4,5,6)
```
{% endtab %}

{% tab title="VLA in method invocation" %}
```ruby
# Variable Length Arguments

def sum(a,b)
    a+b
end

>> sum *[1,2]
=> 3
>> sum *[10,20]
=> 30
```
{% endtab %}

{% tab title="Hash as ARG" %}
```ruby
def printPerson(hash)
    puts hash["name"]
    puts hash["age"]
end

printPerson({"name"=>"Jon", "age"=>25})

# Improved:

def printPerson(hash)
    name = hash[:name] || 'Unknown'
    name = hash[:age] || 'Unknown'
    name = hash[:gender] || 'Unknown'
    print "#{name} #{age} #{gender}"
end

>> printPerson name:"Jon", age:25
=> Jon 25 Unknown
```
{% endtab %}

{% tab title="yield" %}
```ruby
def method
    puts "uno"
    yield
    puts "dos"
    yield
end

>> method { puts "check!" }
uno
check!
dos
check!

# Another example
def double(x)
    yield 2*x
end

>> double(5) { |x| puts x }
=> 10
```
{% endtab %}

{% tab title="call" %}
```ruby
# Ruby allows you to pass a block as an argument.
# With this strategy, the block becomes an instance of the Proc class and you have
# to use call instead of yield to transfer the control to it.
#
# To specify that an argument will be a Proc object that encapsulates a block
# you must use ampersand (&) in method definition.

def square_cube(n,&p)
    for i in 1..0
        p.call(i**2)
        p.call(i**3)
    end
end

>> square_cube(5) { |x| print x, "\s" }
1 1 4 8 9 27 16 64 25 125

>> square_cube(5) { |x| print x-1, "\s" }
0 0 3 7 8 26 15 63 24 124

# Using Proc
>> square = Proc.new {|x| print x**2, "\s"}
>> (1..10).each(&square)
1 4 9 16 25 36 49 64 81 100

>> summ = Proc.new{|sum,x| sum+x**2}
>> (1..5).inject(0,&summ)
=> 55
```
{% endtab %}
{% endtabs %}

## Variables and Scope

> **Variable:** name for a mutable value
>
> * Ruby is a dynamically typed language, you can create a variable without specifying its type.
> * 4 types: **local**, **global**, **instance**, **class**.
> * Ruby allows the definitions of **constant** too.

{% tabs %}
{% tab title="local" %}
```ruby
# Local scope is the area where code that can use the binding 
#  between the name and the object ref value.
```

* If the outside scope has some binding for variables with the same name used in the block scope, they are hidden inside the block and reactivated outside.
* Note that statements like for, while, if, etc... do not define a new scope. Variables defined inside them are still accessible outside.

```ruby
for i in 1..0
    a = i + 1
end

a === 11 # True
```

* Methods does define its own scope and variables.
* Generally, the following control structures define a new scope:
  * `def ... end`
  * `class ... end`
  * `module ... end`
  * `loop {...}`
  * `proc {...}`
  * iterators/method blocks
  * the entire script
* You can also verify the scope of a variable using the `defined?` method.
{% endtab %}

{% tab title="global" %}
* Global variables begins with the **$** special character.
* Accessible anywhere in the program.
* Nor recommendable. Global vars can be changed anywhere in the code.
* Their overuse can make tracking and handling bugs difficult.

```ruby
$*        # array of command line arguments
$0        # name of the script being executed
$_        # last string reads by gets
```
{% endtab %}

{% tab title="instance /  class" %}
* Instance and class variables can be defined within a class definition.
* **Class variables** begin with `@@` **** and they are visible by all instances of a class.
* **Instance variables** begin with `@` and they are local to specific instances of a class.
{% endtab %}

{% tab title="constants" %}
* Constants begin with **uppercase letters**.&#x20;
* Ruby allows you to change uppercased variables but a _warning is raised_.&#x20;
* Constants belong to their namespace in their scope:

```ruby
A=100
module B
    A = 200
end

>> A         # 100
>> B:A       # 200
```

* Ruby has a lot of predefined constants:
  * `ARGV`: holds command line arguments.
  * `ENV`: holds info about the environment.
{% endtab %}
{% endtabs %}

## Some tricks

```ruby
# Multiple variable assignment
>> a,b,c = "a", "b", "c"

# Swap values
>> x = 10; y = 20
>> [x, y] = [y, x]

# Multi-variable assignment via a function
def ret_value
    return 1,2,3
end

one, two, three = ret_value
```
