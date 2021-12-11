# Input/Output

## File Stream

Read/Write from/to an opened stream.&#x20;

To read a file we can use `open` with the correct modifier and then read its contents with a proper method:

* `r` : read only stream
* `r+` : read and write stream&#x20;

If `open` is followed by a block, the file object is passed to the block and the stream is automatically closed at block termination. Inside the block, you have a file object, and you can use different methods as `read`, `each`, `readline` and `gets` among more. Thee methods can be used with many optional arguments in order to perform position dependent and length dependent reading.

### Read

```ruby
File.open("multi_line.txt", "r") do |file|
    contents = file.read
    puts contents
end

# read can be also used without opening the file
content = File.read("multi_line.txt")

# readline is similar to read but it can be used to obtain an array 
# containing the lines of the file:
File.readlines("multi_line.txt")

# each can also be used to read a file line by line
file = File.new("multi_line.txt", "r")

count = 0
file.each do |line|
   puts "n:#{count} #{line}"
   counts+=1
end
```

### Write

```ruby
```

## Working with Nmap files

