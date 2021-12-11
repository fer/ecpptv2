# Control Structures

## Conditional structures

{% tabs %}
{% tab title="Comp ops" %}
| Operator | Description                  |
| :------: | ---------------------------- |
|   `==`   | Equality Operator            |
|  `.eql?` | Equality Operator (OO Style) |
|   `!=`   | Inequality operator          |
|    `<`   | Less than                    |
|    `>`   | Greater than                 |
|   `<=`   | Less than or equal to        |
|   `>=`   | Greater than or equal to     |
{% endtab %}

{% tab title="if" %}
```bash
>> x = 6
?> if x > 3 then
?>   puts "greater!!"
>> end
greater!!

# else
>> x = 2
?> if x > 3 then
?>   puts "greater!!"
?> else
?>   puts "lower!!"
>> end
lower!!

>> puts "greater!!" if x > 1
greater!!
```
{% endtab %}

{% tab title="unless" %}
```ruby
# Opposite of 'if' statement
>> x = 5
=> unless x == 10 then print "#{x}" end
=> print "#{x}" unless x == 10
```
{% endtab %}

{% tab title="case" %}
```ruby
=> x = 5
>> case x
 | when 1 print "one"
 | when 2 print "two" 
 | when 3 print "three"
 | else print "no idea"
 | end
 
# Assign the return value to a variable
>> id = 2000
>> name = case
 | when id == 1111 then "Mark"
 | when id == 1112 then "Bob"
 | when id >= 2000 then "Other"
>> id
=> 2000
>> name
=> Other 
```
{% endtab %}

{% tab title="Comparisons in Ranges" %}
```ruby
(1..10) === 4 # Use triple equal sign
```
{% endtab %}

{% tab title="Ternary Operators" %}
```ruby
name = "Bob"
name == "Bob" ? "Hi Bob" : "Who are you?"
```
{% endtab %}
{% endtabs %}

## Loops

{% tabs %}
{% tab title="while" %}
```ruby
#  Repeats a block of code until the test expression is evaluated to false

>> i = 0 
?> while i < 5 do
?>   print i = i + 1,"\s"
>> end
1 2 3 4 5
=> nil

>> i = 0
>> print i = i + 1,"\s"  while i < 5
1 2 3 4 5
```
{% endtab %}

{% tab title="until" %}
```ruby
# Repeats a block of code until the test expression is evaluated to true
>> i = 5
?> until i == 0
?>   print i-=1,"\s"
>> end
4 3 2 1 0 => nil

>> i = 5
>> print i-=1,"\s" until i == 0
4 3 2 1 0 => nil

```
{% endtab %}

{% tab title="for" %}
```ruby
# iterates through the elements of an enumerable object

?> for i in [10,20,4] do
?>     print i,"\s"
>> end
10 20 4 => [10, 20, 4]

```
{% endtab %}

{% tab title="for with collections" %}
```ruby
>> hash = {:name => "fer", :gender => "maple" }
?> for key, value in hash
?>     print "#{key}:#{value}\n"
>> end
name:fer
gender:maple
```
{% endtab %}

{% tab title="for with ranges" %}
```ruby
?> for i in 1..10
?>     print "#{i}\s"
>> end
1 2 3 4 5 6 7 8 9 10 => 1..10
```
{% endtab %}
{% endtabs %}

## Iterators & Enumerators

{% tabs %}
{% tab title="each" %}
```ruby
>> (1..5).each {|i| print "#{i}\s" }
1 2 3 4 5 => 1..5

# Blocks!
?> ('a'..'h').each do |c|
?>   print "#{c}\n"
>> end
a
b
c
d
e
f
g
h
=> "a".."h"
```
{% endtab %}

{% tab title="times, upto, downto" %}
```ruby
>> 5.downto(1) { |i| print "#{i}\s" }
5 4 3 2 1 => 5

>> 1.upto(5) { |i| print "#{i}\s" }
1 2 3 4 5 => 1

>> 5.times { |i| print "#{i}\s" }
0 1 2 3 4 => 5
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="map" %}
```ruby
>> array = [1, 2, 3, 4, 5]
>> array.map { |x| x**2 }
=> [1, 4, 9, 16, 25]
>> array.map! { |x| x**2 }
=> [1, 4, 9, 16, 25]
>> array
=> [1, 4, 9, 16, 25]
```
{% endtab %}

{% tab title="select" %}
```ruby
>> array
=> [1, 4, 9, 16, 25]
>> array.select { |x| x > 10 }
=> [16, 25]
```
{% endtab %}

{% tab title="reject" %}
```ruby
# oposite of select

>> array
=> [1, 4, 9, 16, 25]
>> array.reject { |x| x > 10 }
=> [1, 4, 9]
```
{% endtab %}

{% tab title="inject/accumulator" %}
```ruby
>> array = [1, 2, 3, 4, 5]
>> array.inject { |sum, x| sum + x}
=> 15
>> array.inject(100) { |sum, x| sum + x}
=> 115
```
{% endtab %}
{% endtabs %}

> **Enumerator**: object whose purpose is to enumerate another enumerable object. This means that enumerators are _enumerable_ too.
>
> If you have a method that uses an enumerable object, you may not want to pass the enumerable collection object because it is mutable and the method may modify it.
>
> So you can pass an enumerator created with **to\_enum** and nothing will happen to the original enumerable collection

{% tabs %}
{% tab title="Ruby" %}
```ruby
>> a = [1,2,3,4]
>> a.each 
=> #<Enumerator: ...>
>> a.map 
=> #<Enumerator: ...>
>> a.select 
=> #<Enumerator: ...>
```
{% endtab %}

{% tab title="External Iterators" %}
```ruby
>> a = [1, 2, 3, 4]
=> enum = a.to_enum
>> enum.next
=> 1
>> enum.next
=> 2
```
{% endtab %}
{% endtabs %}

## Altering structured control flow (break/next)

{% tabs %}
{% tab title="Break" %}
```ruby
>> for i in 1..10
 |    print i,"\s"
 |    break if i == 5
 | end
=> 1 2 3 4 5 => nil

>> (1..10).each do |i|
 |    print i,"\s"
 |    break if i == 5
 | end
=> 1 2 3 4 5 => nil
```
{% endtab %}

{% tab title="Next" %}
```ruby
# The next statement ends the current iteration and jumps to the next one.
>> for i in 1..10
 |    print i,"\s"
 |    next if i == 5
 | end
=> 1 2 3 4 6 7 8 9 10 => nil
```
{% endtab %}

{% tab title="Redo" %}
```ruby
# Redo restarts the current iteration from th efirst instruction in the body
# of the loop or iteration
>> sum = 0 
=> 0
>> for in i in 1..3
 |    sum +=1
 |    redo if sum == 1
 | end
=> 1..3
>> sum
=> 7
>> 1+2+3
=> 6
```
{% endtab %}
{% endtabs %}

## BEGIN/END

These are two reserved words in Ruby.

{% tabs %}
{% tab title="BEGIN/END" %}
```ruby
# BEGIN allows the execution of Ruby code at the beginning of a Ruby program
# END allow sthe execution if Ruby code at the end of a Ruby program

BEGIN {
  puts "Beginning"
}

END {
  puts "Ending"
}

puts "Normal control flow"

=>
Beginning
Normal control flow
Ending
```
{% endtab %}

{% tab title="Ordering of Begin/end" %}
```ruby
BEGIN {puts 1}
BEGIN {puts 2}
BEGIN {puts 3}
END {puts 4}
END {puts 5}
END {puts 6}

=>
1
2
3
6
5
4
```
{% endtab %}
{% endtabs %}
