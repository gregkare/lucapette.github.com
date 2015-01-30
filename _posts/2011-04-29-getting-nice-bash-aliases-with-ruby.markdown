---
title: Getting nice bash aliases with Ruby
description: Getting some bash cd aliases with Ruby
keywords: ruby, bash, alias, aliases
layout: articles
---
I like shortcuts a lot. I like aliases and I like all the stuff that makes me
more productive. Even if we are talking about a couple of characters to change
a directory in the terminal. So I am one of those people that stores all their
code in one directory, precisely in `$HOME/code`. And I found it a bit annoying
to type something like:

{% highlight bash %}
cd[ENTER]
cd c[TAB] --> cd code[ENTER]
cd my-awesome-project[ENTER]
{% endhighlight %}

when I wanted to reach a project of mine. It was annoying because I had to do
it many times in a day even to reach the same directory. So I though it would
be nice to get some dynamic aliases to reach my projects with something like
the following:

{% highlight bash %}
my-awe[TAB] --> my-awesome-project[ENTER]
{% endhighlight %}

It would have been a small boost in my productivity. So I tried to do it with
Bash scripting but, since I am not a bash
[hero](http://twitter.com/#!/wayneeseguin), I wasn’t successful. So I switched
to Ruby and I came up with the following script:

{% highlight ruby %}
#!/usr/bin/ruby 

Dir["#{File.expand_path("~/code")}/*"].each do |file|
  if File.directory?(file)
    cmd=File.basename(file)
    puts "alias #{cmd}=\"cd #{file}\"" if `which #{cmd}`.length == 0
  end
end
{% endhighlight %}

To my mind it is quite self-explanatory. And in my bashrc I put the following:

{% highlight bash %}
$HOME/.bash/code_dirs.rb > $HOME/.bash/codedirs
source $HOME/.bash/codedirs
{% endhighlight %}

This simply redirects the output of the above-mentioned Ruby script in a file
sourced in the current bash shell. It just works fine for me.
