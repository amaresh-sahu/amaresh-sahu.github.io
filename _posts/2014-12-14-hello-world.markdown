---
layout: post
title:  "'hello, world!' (and where it came from)"
date:   2014-12-14 14:27:01
categories: cs
---

Any time you learn a new programming language, the first program you write will output the phrase ['hello, world!'](http://en.wikipedia.org/wiki/%22Hello,_world!%22_program) (or some variation of it) to the screen. This is one of the simplest programs you can write, and getting it to work means that everything going on 'under the hood' (compiler & environments) is also working. But this is true for any program that outputs text. I wanted to find out what was so special about the phrase 'hello, world!', where it came from, and why it is ubiquitous in the world of computer science.

It turns out that `hello, world` was the output of the [first program](http://www.extremetech.com/computing/102835-dennis-ritchie-creator-of-c-bids-goodbye-world) in the book *[The C Programming Language](http://www.amazon.com/Programming-Language-Brian-W-Kernighan/dp/0131101633/)* (1978). In this example, the `\n` is a new line character, meaning that any further output would start on the next line.

{% highlight c linenos %}

main()
{
	printf("hello, world\n");
}

{% endhighlight %}


This book was written by Dennis Ritchie, creator of C, and Brian Kernighan while they were both working at Bell Labs. It is incredibly popular, having sold millions of copies and been translated into dozens of languages. C has been widely adopted and has influenced nearly all languages in use today. Because of the popularity of C and this book, it is no wonder 'hello, world!' has made its way to computer screens all around the world.

While this explains how the phrase was popularized, it is not the beginning of the story. Kernighan wrote an internal memorandum while at Bell Labs, called [Programming in C: A Tutorial (1974)](http://cm.bell-labs.com/cm/cs/who/dmr/ctut.pdf). In it was almost identical code:

{% highlight c linenos %}

main(){
	printf("hello, world");
}

{% endhighlight %}

Although Kernighan had nothing to do with creating C, he wrote a tutorial on it because he had also written a tutorial to the programming language B (the precursor to C) in 1972. In [that tutorial](http://cm.bell-labs.com/cm/cs/who/dmr/scbref.pdf), he included the following:

{% highlight c linenos %}

main(){
	extrn a,b,c;
	putchar(a); putchar(b); putchar(c); putchar('!*n');
	}

a 'hell';
b 'o, w';
c 'orld';

{% endhighlight %}

This example is used to illustrate external variables, necessary since all character constants are limited to four ASCII characters. This code will piece together the variables a, b, and c to print `hello, world` We again see the new line character, this time expressed as `*n`.

It is [further claimed](http://stackoverflow.com/questions/602237/where-does-hello-world-come-from) that 'hello, world' originated earlier, in [the manual to BCPL](http://www.fh-jena.de/~kleine/history/languages/Richards-BCPL-ReferenceManual.pdf) (a precursor to B). While this manual (and the language itself) was written by Kernighan and Martin Richards of Cambridge, I have yet to see such a program in it. The good news is that Kernighan is currently a professor at Princeton, so I'll try to bump into him when I'm next on campus and figure out where the phrase truly originated. For the time being, however, 1972 is the date to beat.

So now you know. The next time you see someone learning a new programming language or you see 'hello, world!' printed on a t-shirt, feel free to spread the word.

And of course, as this is my first post:

> hello, world!
