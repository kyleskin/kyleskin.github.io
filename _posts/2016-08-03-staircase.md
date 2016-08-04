---
layout: post
title: Staircase
---

I was on HackerRank today and saw a pretty simple challenge called "StairCase."

***Write a function "StairCase" which takes an integer N, the height of the staircase as its argument, and prints a staircase as shown in the example.***

{% highlight ruby %}
Sample Input
N = 6
{% endhighlight %}

{% highlight ruby %}
Sample Output

      #
     ##
    ###
   ####
  #####
 ######  
{% endhighlight %}

I solved it pretty quickly in Ruby, but thought it would be a good chance to practice a little JavaScript, specifically how to repeat a string.  Like with my FizzBuzz post, I'll cover how I solved it in Ruby and then in JavaScript.

**Ruby**

My first step was to break down exactly what is happening on each line.  It might be easy to think of it as just an increasing number of hash signs, but it's important to not forget about the blank spaces that are present, as well.  So, each line consists of a total number of characters equal to N.  Assuming N is equal to 6, the first line has 5 spaces and then 1 hash, the second line has 4 spaces and then 2 hashes, and etc.  This means, each line has blank spaces equal to N minus the line number, and hashes equal to the line number.

The first thing that came to mind was using a while loop to keep track of which line we're on and allow us to make that simple calculation of N - line number.  I also thought of it as building each line from the individual components of spaces and hashes.  And that was pretty much all there is to it.

{% highlight ruby %}
def staircase(n)
  counter = 1
  while counter <= n
    string = ''
    string += ' ' * (n - counter)
    string += '#' * counter
    counter += 1
    puts string
  end
end

staircase(6)
{% endhighlight %}

To break this down, we're first defining the method `staircase` and saying it will accept an argument `n`.  Inside the method, we set a counter to 1 and then begin our while loop, looping through as long as `counter` is less than or equal to `n`.  Next, inside of the loop, we first declare our variable `string` and set it to be completely blank.  Next, we're going to set `string` equal to itself plus the blank spaces.  Ruby lets us multiply strings, so we can simply say `' '` times `(n - counter)`.  Then, it's on to building the hashes; again since we can multiply strings, this is as easy as setting `string` equal to itself (our series of blank spaces, this time) plus `'#'` times `counter`.  Lastly, we output `string` and end the while loop and then the method.  We then call our method `staircase` and pass it the number 6.

**JavaScript**

The solution I came up with for JavaScript followed the same steps as Ruby, with just a couple of differences.  Let's look at the code, and then we'll dissect it.

{% highlight js %}
function staircase(n){
  for ( counter = 1; counter <= n; counter ++ ){
    string = '';
    string += ' '.repeat(n - counter);
    string += '#'.repeat(counter);
    console.log(string);
  }
}

staircase(6);
{% endhighlight %}


Again, we start by defining our function `staircase` and saying it will accept the argument `n`.  Then we begin a for loop, setting `counter` to 1, specifying that it will continue as long as `counter` is less than or equal to `n`, and increasing `counter` by 1 each iteration.  Inside the loop, we define `string` as an empty string and then start to build its components.  JavaScript doesn't allow for mathematical operators on strings, like Ruby does, so instead we use the `repeat` method, using `n - counter` for our blank spaces and `counter` for the hashes.  Finally we output the result of `string` to the console.  We close the for loop and the function, and then call `staircase` and pass it 6.

Like I said at the beginning, this isn't a particularly challenging problem, but before I attempted it in JavaScript, I wasn't sure how to handle building the lines without being able to multiple a string, so it was a good opportunity for me to learn about the `repeat` method.  
