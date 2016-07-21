---
layout: post
title: Ruby Frequency Sorter
---
I was recently tasked with writing a small function as a technical exercise for a possible internship.  On it's surface it was pretty simple, but it made me learn a lot of new things.

The exercise was to:
***Write a simple function that takes an input file of sentences (single column, one per line) and produces a new file with the ordered frequency counts of these sentences.  For instance, a couple of example sentences are: "I like dogs" and "I like cats".***

***Analyze your program and give a detailed running time of it.***

I started by identifying two main steps:

1. I would need a function that would seed my file, since I knew I wanted to have several thousand lines and I definitely didn't want to write them myself.
2. I would need the main function that did the sorting once I had my seeded text file.

Initially I thought of the pseudo code for the first function.

{% highlight ruby %}
#Create an array and fill it with example sentences.

#Take each sentence and multiply it a random number of times.

#Write the sentences to a new text file.
{% endhighlight %}

Again, simple. Let's start with creating the array with the sample sentences.

{% highlight ruby %}
#!/usr/bin/env ruby
#Create an array and fill it with example sentences.
sentences = ["I like cats", "I like dogs", "I like turtles", "I like frogs", "I like parrots"]

#Take each sentence and multiply it a random number of times.

#Write the sentences to a new text file.
{% endhighlight %}

Next, we'll loop over each item and create a random number of sentences.

{% highlight ruby %}
#!/usr/bin/env ruby
#Create an array and fill it with example sentences.
sentences = ["I like cats", "I like dogs", "I like turtles", "I like frogs", "I like parrots"]

#Take each sentence and multiply it a random number of times.
sentences.each do |sentence|
  output = (sentence + "\n") * ((rand(9) + 1) * 1_000)
end
#Write the sentences to a new text file.
{% endhighlight %}
Here we're taking the `sentences` array and looping through its contents with the `each` method, giving each sentence the name `sentence` and passing it into a block.  Inside the block we take each `sentence` and add the new line character `\n` to it, so each sentence is on a separate line.

Ruby lets us then take the sentence and multiply it to get copies of it.  We need each sentence to have a different output amount, so we're creating a random number.  Passing in 9 to `rand` gives us a random integer 0 to 9.  Since this means we could get 0 and then not have any copies of a sentence, we're adding 1 to it, giving us a possible random number 1 to 10, which we then multiply by 1,000.

Lastly, we need to write the sentences to a new text file.

{% highlight ruby %}
#!/usr/bin/env ruby
#Create an array and fill it with example sentences.
sentences = ["I like cats", "I like dogs", "I like turtles", "I like frogs", "I like parrots"]

#Take each sentence and multiply it a random number of times.
sentences.each do |sentence|
  output = (sentence + "\n") * ((rand(9) + 1) * 1_000)
#Write the sentences to a new text file.
  File.open('seed.txt', 'a').print output
end
{% endhighlight %}

With `File.open` we pass in the name of the file and give it a mode.  Mode `a` means write-only and it will append the data at the end of the file.  This way we don't have to worry about each previous group of sentences being overwritten each time our block loops.  Also, with this mode, if a file doesn't already exist with the name we pass in, it will create it for us.

So there we have it.  Step one is done, we have a file with a random number of repeated sentences that we can now pass into the second part of our function to be sorted.


On to step two.  Let's start with some pseudo code again.

{% highlight ruby %}
#Create an empty array that we can pass our sentences into.

#Open the file and put each sentence into the array.

#Go through the array and count how many times each sentence occurs and sort them accordingly.

#Analyze the speed of the function
{% endhighlight %}

Let's just jump right in to this.

{% highlight ruby %}
#!/usr/bin/env ruby
#Create an empty array that we can pass our sentences into.
result = []

#Open the file and put each sentence into the array.
result = File.read('seed.txt').split("\n")

#Go through the array and count how many times each sentence occurs and sort them accordingly.

#Analyze the speed of the function
{% endhighlight %}

We first have the empty array `result` which we fill by calling `File.read` and giving it the file name, then using `split` to look for each new line character to divide the sentences.

Now we can sort the array.

{% highlight ruby %}
#!/usr/bin/env ruby
#Create an empty array that we can pass our sentences into.
result = []

#Open the file and put each sentence into the array.
result = File.read('seed.txt').split("\n")

#Go through the array and count how many times each sentence occurs and sort them accordingly.
p result.each_with_object(Hash.new(0)) { |key, value| value[key] += 1 }.sort_by{ |key, value| value }
#Analyze the speed of the function
{% endhighlight %}

This line is a little bit more complicated, so let's break it down.  `p` will print out to the console our final hash, once everything is sorted.  I went with `p` instead of `print` or `puts` because it doesn't call `to_s` and I want the data displayed inside the final hash.  However, the other two options would have worked just as well.

Next, we're calling `each_with_object` and passing in `Hash.new(0)`.  `each_with_object` is for creating a collection out of a collection - in this case, we're taking our array or sentences and then making a hash that will contain how often each sentence appears.  Then we have the block that says we have a `key`, the sentence, and the `value`, which will be our count.  It says that for each time it has the same key, the value will increase by one.  Lastly, we call `sort_by`, passing in a block that identifies the `key` and `value` from the hash and says to order by the value.

Great! Now we just need to add a couple lines to analyze the code.

{% highlight ruby %}
#!/usr/bin/env ruby
require 'benchmark'

speedTest = Benchmark.measure do |bm|
#Create an empty array that we can pass our sentences into.
  result = []

#Open the file and put each sentence into the array.
  result = File.read('seed.txt').split("\n")

#Go through the array and count how many times each sentence occurs and sort them accordingly.
  p result.each_with_object(Hash.new(0)) { |key, value| value[key] += 1 }.sort_by{ |key, value| value }
end
#Analyze the speed of the function

puts speedTest
{% endhighlight %}

Seeding the text file gave me 27,000 lines of code, which I then ran the sort function on 10 times for the following averages:

{% highlight ruby %}
  user: 0.013000 | system: 0.000000 | total: 0.013000 | real: 0.0138918
{% endhighlight %}

This function was a great exercise to learn a couple new things like using `each_with_object` and opening and reading files.  Overall, it was a lot of fun to work on, and hopefully my explanation of the code makes sense and can help you in your development, too.
