# WIP - Classes, Modules and Exceptions

## Class Principles

> **Classes** define what an object will look like.

{% tabs %}
{% tab title="class" %}
```ruby
# Must start with a capital letter, so Ruby silently creates the constant 
# to refer to the class. Capital letter is required.
class MyClass 
    def hello
        print "Hello"
    end
end

>> myObj = MyClass.new
>> myObj.hello
```
{% endtab %}

{% tab title="Instance Variables" %}
```ruby
class Myclass
    # Constructor
    def initialize(a)
        @a = a
    end
    
    # Setter
    def a=(value)
        @a = value
    end 

    # Getter
    def a
        @a
    end
end

>> obj1 = MyClass.new(20)
>> obj2 = MyClass.new(3)
>> obj1.a = 400
```
{% endtab %}

{% tab title="Instance Vars 2" %}
```ruby
class WrongClass
    @a = 4000
    
    def a 
        @a
    end
    
    def a=(val)
        @a = val
    end
end

>> wobj = WrongClass.new
>> wobj.a                     # nil
>> wobj.instance_variables    # [ ]
>> wobj.a = 100
>> wobj.instance_variables    # [:@a]
>> wobj.a                     # 100

```
{% endtab %}

{% tab title="Metaprogramming" %}
```ruby
# Getter/Setter via metaprogramming
# Metaprogramming allows to write, manipulate and
# generate programs at runtime.

class QuickClass
    attr_accessor :x, :y
    # attr_accessor defines a getter and setter 
end

>> obj = QuickClass.new
>> obj.x, obj.y = 100, 300

# with attr_reader, a getter is defined

class QuickClass
    attr_reader :x, :y
    
    def initialize(x, y)
        @x, @y = x, y
    end
end

>> obj.x = 123 # !!!! Error

# Attr 
# if used alone, it defines a getter, while 
# with 'True' it defines a setter too.

class QuickClass
    attr :x, true
    attr :y
    
    def initialize(x,y)
        @x, @y = x,y
    end
end

>> my_class = QuickClass.new(10,20)
>> obj.x = 100
>> obj.y = 200    # error
```
{% endtab %}

{% tab title="Class Methods" %}
```ruby
# use self for declaring own class methods
# these methods won't be visible per class instance

class C1
    def self.say
        print "hello!"
    end
end

>> C1.say
=> "hello!"
>> obj = C1.new
>> obj.say => # !!!! error
```
{% endtab %}
{% endtabs %}

Exercise:

```ruby
class ClassObj
  # class object @a constructor
  @a = 100
  
  # Instance object getter/setter for @a
  attr_accessor :a
 
  # class object setter for @a
  def self.a=(val)
   @a = val
  end
  
  # class object getter for @a  
  def self.a 
   @a 
  end
  
  # instance object @a constructor
  def initialize(a)
   @a = a 
  end
end
```

> Class methods may be defined in a few other ways
>
> * Using the class name instead of `self` keyword
> * Using the `<<` notation: this is useful when you work with classes that have been already defined.&#x20;

```ruby
class Operation
 
 def Operation.add(a,b)
   a + b
 end
 
 class << Operation
   def mul(a,b)
     a * b
   end
 end
end

>> load classMethods.rb"
>> Operation.add 10,30 # => 30
>> Operation.mul 10,20 # => 200
```

A class variable must start with special characters @@ and it is shared among all class instances:

### Method Visibility

### Subclassing and Inheritance

### Modules

### Exceptions

### Conclusion

