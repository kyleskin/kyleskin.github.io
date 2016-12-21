---
layout: post
title: Advent of Code - Rotating Columns
---
A friend of mine from DaVinci Coders, Laurie Linz, pointed me towards a great set of coding challenges with a fun Christmas theme at [Advent of Code](http://adventofcode.com).  They've been a great way to practice tackling new problems and have led me to learn a few new techniques and methods in Ruby.  

The challenge for Day 8 is:

***Day 8: Two-Factor Authentication***

***You come across a door implementing what you can only assume is an implementation of two-factor authentication...***

***To get past the door, you first swipe a keycard (no problem; there was one on a nearby desk). Then, it displays a code on a little screen, and you type that code on a keypad. Then, presumably, the door unlocks.***

***Unfortunately, the screen has been smashed. After a few minutes, you've taken everything apart and figured out how it works. Now you just have to work out what the screen would have displayed.***

***The magnetic strip on the card you swiped encodes a series of instructions for the screen; these instructions are your puzzle input. The screen is 50 pixels wide and 6 pixels tall, all of which start off, and is capable of three somewhat peculiar operations:***

***rect AxB turns on all of the pixels in a rectangle at the top-left of the screen which is A wide and B tall.
rotate row y=A by B shifts all of the pixels in row A (0 is the top row) right by B pixels. Pixels that would fall off the right end appear at the left end of the row.
rotate column x=A by B shifts all of the pixels in column A (0 is the left column) down by B pixels. Pixels that would fall off the bottom appear at the top of the column.***

***There seems to be an intermediate check of the voltage used by the display: after you swipe your card, if the screen did work, how many pixels should be lit?***

This challenge may seem complicated at first, but it can be broken down into several smaller problems.  First, we need to create a display screen.  Second, we have to read in and parse a series of instructions.  Third, we need to either draw a rectangle, shift the pixels in a row, or shift the pixels in a column.  Lastly, we will count how many pixels are lit.

Let's start with some psuedo code!

{% highlight ruby %}
#create display(width, height)

#read in instructions

#parse instructions

#draw rectangle(width, height)

#shift row(row number, number of spots to shift)

#shift column(column number, number of spots to shift)

#count pixels
{% endhighlight %}

To create the display, we'll use a two-dimensional array, which is an array with additional arrays nested within it.

{% highlight ruby %}
#create display(width, height)
@display = Array.new(6, [])
@display.map! {|array| array = Array.new(50, '.')}
{% endhighlight %}

The first line creates an array with 6 more arrays inside of it (`[[], [], [], [], [], []]`).  We can think of this as being our display with six rows of pixels, but only one column.  In the second line, we run `map!` on our display, which takes each of the six subarrays and creates 50 arrays within it, filling each array with `'.'` which we'll use to signify a pixel that is turned off.

We now have a display that is 50x6 and has every pixel turned off.  In order to access each element within `@display` we first use the row value followed by the column value.  So, the top left pixel would be `@display[0][0]`.  The pixel in the 3rd row from the top and five rows in from the left would be `@display[2][4]`.

Now that we have the display, let's move on to reading in the instructions.

{% highlight ruby %}
#read in instructions
File.open('lib/day_eight/day_eight.input') do |file|
  file.each_line do |line|
  end
end
{% endhighlight %}

Here we're using Ruby's `File` class and methods to open the instructions (I have them in a file called `day_eight.input`) and then taking that file and calling the `each_line` method to read every individual line.  That was pretty easy, so now we'll parse each line.

Because there are only three types of instruction - draw rectangle, shift row, shift column - this isn't too complicated either.  

{% highlight ruby %}
#read in instructions
File.open('lib/day_eight/day_eight.input') do |file|
  file.each_line do |line|
    #parse instructions
    case line
    when /rect (\d+)x(\d+)/
      draw_rect($1.to_i, $2.to_i)
    when /rotate row y=(\d+) by (\d+)/
      rotate_row($1.to_i, $2.to_i)
    when /rotate column x=(\d+) by (\d+)/
      rotate_column($1.to_i, $2.to_i)
    end
  end
end
{% endhighlight %}

I'm using using Ruby's `case` statement operator with each `line` passed to it.  Our `when` clauses then use regular expressions (or regexes) to parse each statement.  Again, since each instruction is uniform it's very easy to parse these lines.

For example, to draw a rectangle we know the line will start with "rect" followed by the width, an "x" and then the height.  The regex is then looking for that exact pattern.  The `(\d+)` means we're creating a match group that will contain one or more digits.  These match groups are inputs that can be accessed with `$1` and `$2`.  So, we're then calling the method `draw_rect` (which doesn't exist yet) and passing in `$1` and `$2` as integers.  We're then doing the exact same thing with the methods `rotate_row` and `rotate_column`.  Often times with a `case` statement, you would provide an `else` clause in case the option isn't met with any of the `when` clauses.  However, since we know exactly what the instructions look like, that isn't necessary here.

Now we can create the methods that we called when parsing the instructions, starting with `draw_rect`.

{% highlight ruby %}
#draw rectangle(width, height)
def draw_rect(x_dimension, y_dimension)
  y_dimension.times do |y|
    x_dimension.times do |x|
      @display[y][x] = '#'
    end
  end
end
{% endhighlight %}

First we define the method and specify that it will take the arguments `x_dimension` and `y_dimension` (these were `$1` and `$2` from when we called the method).  When working with 2D arrays, we first access the row and then the column within that row, like we said before.  For example, if we're drawing a rectangle that's 3x2, we'll access the first two rows and the first three columns in each row.  So, if we plug those values into the code, we'll start with a `y` value of 0 and an `x` value of 0.  At position `@display[0][0]` we fill in a `#` which we'll use to denote a pixel that is on.  Then we still have a `y` value of 0 but now we have an `x` value of 1.  So `@display[0][1]` gets a `#`, too.  We then move on to `@display[0][2]` and fill it in with a `#` one last time.  We've now done our `x_dimension` loop 3 times, which is the value of `x_dimension` and so the `y_dimension` loop increments up to 1 and `x_dimension` resets to 0.  Pixels are now added to `@display[1][0]`, `@display[1][1]` and `@display[1][2]`.  The `y_dimension` loop has now run 2 times (the value of `y_dimension`) and we have our rectangle completed.

Next, let's shift the values in a row.

{% highlight ruby %}
#shift row(row number, number of spots to shift)
def rotate_row(row, distance)
  @display[row].rotate!(-distance)
end
{% endhighlight %}

Ruby has an array method called `rotate!` (which is why I've called the method `rotate_row`) that will do exactly what we need it to.  We simply pass in which row we're modifying and how many spots everything needs to shift over.  However, `rotate!` shifts everything from right to left and we need to go from left to right, which is why we are making the `distance` value negative.

The next method, `rotate_column` is a little bit trickier and is what really made me want to write about this challenge.  Because the subarrays of `@display` represent rows and not columns, it's not as simple as just calling `rotate!` on the column we need to change.

{% highlight ruby %}
#shift column(column number, number of spots to shift)
def rotate_column(column, distance)
  temp = @display.map { |row| row[column] }.rotate!(-distance)
  @display.each_with_index {|row, index| row[column] = temp[index]}
end
{% endhighlight %}

The first step to this method is taking `@display` and calling `map` on it, which will access each row.  Within each row, we access the value at `column`.  So, if we're rotating the third column, we are going through and accessing first `@display[0][2]` and then `@display[1][2]`, `@display[2][2]` and so on through all the rows.  The great thing about `map` is that it returns those values in an array so we can then call `rotate!` on it, just like we did for `rotate_row`.  This rotated array is then stored in the variable `temp`.

Now we have to take the values that are in `temp` and put them into `@display`.  For this, we use `each_with_index`.  This will return each row and give us the index of that row.  We can then use that index to access the appropriate value inside `temp`, which is put into the correct column position.

And that's it for the tricky stuff - it's all very basic from here.

{% highlight ruby %}
#count pixels
@display.each {|row| puts row.join("") }
pixel_count = 0
@display.each { |row| pixel_count += row.count('#') }
p pixel_count
{% endhighlight %}

It wasn't necessary for this challenge, but I did choose to output the results of the display, which is the first line.  Then we set a variable `pixel_count` to 0, and go through each row and count every `#`, adding it to `pixel_count` and end with outputting the total.

Just for fun, I did try a couple different methods to solve this challenge and ran speed tests on them and found that what I've outlined here was my best solution.  Running the code 1,000 times gave the following result:

{% highlight ruby %}
  user: 0.830000 | system: 0.070000 | total: 0.900000 | real: 0.934151
{% endhighlight %}

There are some ways to further refactor the code (I didn't focus on OOP, for example) but I feel this is a good solution that leaves the code simple and easy to read.  This was a fun challenge and great exercise in breaking down a problem into more manageable pieces and forced me to come up with a solution to a problem I hadn't encountered before, and I really recommend signing up for [Advent of Code](http://adventofcode.com).  The developers there have created some fantastic puzzles and deserve all the support they can get.

***Full solution below:***

{% highlight ruby %}
#!usr/bin/env ruby
@display = Array.new(6, [])
@display.map! {|array| array = Array.new(50, '.')}

def draw_rect(x_dimension, y_dimension)
  y_dimension.times do |y|
    x_dimension.times do |x|
      @display[y][x] = '#'
    end
  end
end

def rotate_row(row, distance)
  @display[row].rotate!(-distance)
end

def rotate_column(column, distance)
  temp = @display.map { |row| row[column] }.rotate!(-distance)
  @display.each_with_index {|row, index| row[column] = temp[index]}
end

File.open('lib/day_eight/day_eight.input') do |file|
  file.each_line do |line|
    case line
    when /rect (\d+)x(\d+)/
      draw_rect($1.to_i, $2.to_i)
    when /rotate row y=(\d+) by (\d+)/
      rotate_row($1.to_i, $2.to_i)
    when /rotate column x=(\d+) by (\d+)/
      rotate_column($1.to_i, $2.to_i)
    end
  end
  @display.each {|row| puts row.join("") }
  pixel_count = 0
  @display.each { |row| pixel_count += row.count('#') }
  p pixel_count
end
{% endhighlight %}
