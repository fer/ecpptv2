---
description: Numbers, Strings, Arrays, Ranges & Hashes.
---

# Data Types

## Numbers

> Keep in mind that _every value_ in Ruby is an object, including the operators `+`, `-`, `/`, used in operations are happening between objects.

{% tabs %}
{% tab title="Integer" %}
```
>> 2**5
=> 32
>> 10/2
=> 5
>> 4.odd?
=> false
>> 10.next
=> 11
>> 10.pred
=> 9
>> 25.to_s
=> "25"
>> 65.chr
=> "A"
>> -5.abs
=> 5
```
{% endtab %}

{% tab title="Float" %}
```
>> 2.0**3
=> 8.0
>> 2.51.round
=> 3
>> 2.51.ceil
=> 3
>> 2.51.floor
=> 2
```
{% endtab %}

{% tab title="Numeric" %}
**Integer** & **Float** _extend_ `Numeric` wich in turn extends **Object** and so on up to the **Basic Object**.
{% endtab %}

{% tab title="Anticipation" %}
{% code title="ip_upto.rb" %}
```bash
# $ ruby ip_upto.rb 192.168.1 10 20
# 192.168.1.10
# 191.168.1.11
# ....
# 192.168.1.20

(ARGV[1]..ARGV[2]).each {|i| print ARGV[0],".",i,"\n"}
```
{% endcode %}
{% endtab %}

{% tab title="Comments" %}
```ruby
=begin
 Multiline comment
 with multiple lines :)
=end

# this is a comment too.
```
{% endtab %}
{% endtabs %}

## **Strings**

> Another built-in Ruby class. Create strings with single and double quotes.

{% tabs %}
{% tab title="Escape Sequences" %}
```ruby
# Single quotes only support two escape sequences:
>> 'It\'s funny'
=> "It's funny"
>> '\\backslash!'
=> \backslash!
```
{% endtab %}

{% tab title="Double Quotes" %}
Double quotes support escape sequences. Some of the are:

| Syntax | Escape sequence description |
| ------ | --------------------------- |
| `\"`   | Double quote                |
| `\r`   | Carriage return             |
| `\s`   | Space                       |
| `\t`   | Tab                         |
| `\n`   | New Line                    |
| `\\`   | Single backslash            |
{% endtab %}

{% tab title="Alternative quotes" %}
You can add your custom string delimiter after the first `%q`character to instruct Ruby where the quoted string begins:&#x20;

```bash
>> print %q!my string! 
=> my string=> nil
```

{% hint style="info" %}
`%q` works as delimited _single quote_.

`%Q` works as delimited _double quote_.
{% endhint %}

You can make full use of:

* Brackets
* Braces
* Parenthesis
* <> signs

```bash
>> print %q[my string]
my string=> nil
>> print %q<my string>
my string=> nil
>> print %q{my string>}
my string>=> nil
>> print %q(my string)
my string=> nil

```
{% endtab %}

{% tab title="Operations" %}
```bash
# check if string is empty
str.empty?

# clear string
str.clear

# Length
str.length

str.size
str.start_with? "start"
str.end_with? "end"

# ... and many more string methods... 
# Remember: strings are objects in Ruby
```
{% endtab %}

{% tab title="heredoc" %}
```ruby
str = <<END
This is a multiline
string... Enjoy!
END
```
{% endtab %}

{% tab title="String Arithmetics" %}
```ruby
# + notation
"this" + " is " + "a string"

# juxtaposition
"this" " is " "a string too" 

# << notation
"this" << " is" << " a string"

# OO Notation
"this".concat(" is ").concat("a string")

# Do not alter a string
st = "my string"
st.freeze

# Check if string is frozen
st.frozen?

# [index] method
st = "My String"
st["My"]             # => "My"
st[1..6]             # => "y stri"
st[0]                # => "M"

# 'sub' replaces first ocurrence
st = st * 2         # => "My stringMy string"
st.sub('i','1')     # => "My str1ngMy string"

# 'gsub' replaces all ocurrences
st.gsub('g','8')     # => "My strin8My strin8"

# Note: modify the original string with:
# => st.gsub!('g','8')
# => st.sub!('i','1')

# Insert string
st.insert(st.size, " FIN")
```
{% endtab %}

{% tab title="Interpolation" %}
```ruby
>> st = "1234567890"
>> "'st' string has #{st.length} chars"
=> "'st' string has 10 chars"
```
{% endtab %}

{% tab title="More useful methods" %}
```ruby
>> "asdf".upcase
=> "ASDF"
>> "asdf".downcase
=> "asdf"
>> "asdf".capitalize
=> "Asdf"
>> "asdf".reverse
=> "fdsa"
>> "asdf".chop
=> "asd"
```
{% endtab %}
{% endtabs %}

## **Array**

> An Array is an Object containing other Objects (including other Arrays) accessible through an Index.

{% tabs %}
{% tab title="Creation" %}
```bash
=> arr = Array.new(2)
=> arr = [] 
=> arr = ['uno', 'dos']
=> arr << 'tres'         # ['uno', 'dos', 'tres']

```
{% endtab %}

{% tab title="Accessing" %}
```bash
>> arr = ['uno', 'dos', 'tres']
>> arr[0]                 # => 'uno'
>> arr[-1]                # => 'tres'
>> arr[-2]                # => 'dos'
>> arr.first              # => 'uno'
>> arr.last               # => 'tres'
>> arr[1..2]              # => 'tres' 
```
{% endtab %}

{% tab title="Insert" %}
```ruby
>> arr.insert(0, 'cero')
=> ["cero", "uno", "dos", "tres"]
>> arr.insert(4,"cuatro", "cinco")
=> ["cero", "uno", "dos", "tres", "cuatro", "cinco"]
>> arr << "seis"
=> ["cero", "uno", "dos", "tres", "cuatro", "cinco", "seis"]
```
{% endtab %}

{% tab title="Deletion" %}
```ruby
>> arr
=> ["cero", "uno", "dos", "tres", "cuatro", "cinco", "seis"]
>> arr.delete("cero")
=ruby> "cero"
>> arr
=> ["uno", "dos", "tres", "cuatro", "cinco", "seis"]
>> arr.delete_at(0)
=> "uno"
>> arr
=> ["dos", "tres", "cuatro", "cinco", "seis"]
```
{% endtab %}

{% tab title="Operations" %}
```ruby
>> arr = [1,2,3]
>> arr2 = [3,4,5]
>> arr + arr2            # => [1, 2, 3, 3, 4, 5]
>> arr.concat(arr2)      # => [1, 2, 3, 3, 4, 5]

# Union (|): concatenates two arrays, removing dupes
# Intersection (&): common elements to both arrays
# Difference (-): 1st array without the element contained into the second array

>> arr                     # => [1, 2, 3]
>> arr2                    # => [3, 4, 5]
>> arr | arr2              # => [1, 2, 3, 4, 5]
>> arr & arr2              # => [3]
>> arr - arr2              # => [1, 2]
```
{% endtab %}

{% tab title="Stack" %}
```ruby
>> arr            # => [1, 2, 3]
>> arr.push(4)    # => [1, 2, 3, 4]
>> arr.pop()      # => 4
>> arr            # => [1, 2, 3]
```
{% endtab %}

{% tab title="More methods" %}
```ruby
>> arr = [10,1,1,1,2,3]
>> arr.reverse            # => [3, 2, 1, 1, 1, 10]
>> arr.uniq               # => [10, 1, 2, 3]
>> arr.sort               # => [1, 1, 1, 2, 3, 10]
>> arr.uniq               # => [10, 1, 2, 3]
>> arr.max                # => 10
>> arr.min                # => 1

# Persist changes
>> arr.uniq!.sort!        # => [1, 2, 3, 10]
```
{% endtab %}

{% tab title="Arrays and Strings" %}
```ruby
>> arr = [ 'hi', 'world', '!'] 
>> arr.join(' ')        # => "hi world !"

# Reverse operation
>> "hi world !".split(' ') # => ["hi", "world", "!"]
```
{% endtab %}
{% endtabs %}

## **Ranges and Hash**

> **Ranges:** allows data to be represented in the form of a range, consisting in a _start value_ and and _end value_.

{% tabs %}
{% tab title="Ranges" %}
```ruby
>> ('a'..'f').to_a
=> ["a", "b", "c", "d", "e", "f"]

# 3 dots notation excludes last element
>> ('a'...'f').to_a
=> ["a", "b", "c", "d", "e"]

>> ("hi 1".."hi 4").to_a
=> ["hi 1", "hi 2", "hi 3", "hi 4"]

>> (1.1..4.4).step.to_a
=> [1.1, 2.1, 3.1, 4.1]
```
{% endtab %}

{% tab title="Methods" %}
```ruby
>> (1..10).begin        # => 1
>> (1..10).end          # => 10
>> (1..10).max          # => 10
>> (1..10).min          # => 1

# Contains
>> (1..10) === 4        # => true
>> (1..10) === 11       # => false
```
{% endtab %}
{% endtabs %}

> **Hashes:** similar to Arrays but acting as _dictionaries_. This is, they can use an index to reference an object.
>
> This **index can be also an object instead of an integer**. Arrays use integers as index.

{% tabs %}
{% tab title="Hashes" %}
```ruby
>> hash = { "a" => "Hi", "b" => "There"}
>> hash             # => {"a"=>"Hi", "b"=>"There"}
>> hash['a']        # => "Hi"
```
{% endtab %}

{% tab title="Hash keys are Symbols" %}
```ruby
>> website = {:url => 'https://ferx.gitbook.io/wiki/', :title => 'Wiki'}
>> website[:url]
=> "https://ferx.gitbook.io/wiki/"

# Same can be achieved without ':'
>> website = {url: 'https://ferx.gitbook.io/wiki/', title: 'Wiki'}
>> website[:url]
=> "https://ferx.gitbook.io/wiki/"

```
{% endtab %}
{% endtabs %}
