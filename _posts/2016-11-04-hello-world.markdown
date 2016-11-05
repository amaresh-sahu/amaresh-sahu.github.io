---
layout: post
title:  "'hello, world!' (and where it's from)"
date:   2016-11-04 08:00:00
---
When learning a new programming language, the first program written generally outputs the phrase ['hello, world!'](http://en.wikipedia.org/wiki/%22Hello,_world!%22_program){:target="_blank"} (or some variation of it) to the screen. While incredibly simple, such a program ensures everything "under the hood" (the compiler and environments) is working as well. As this is my first post on this website, I too wanted to start off with the phrase -- but then I started wondering where the phrase came from.

As it turns out, `hello, world` was the output of the [first program](http://www.extremetech.com/computing/102835-dennis-ritchie-creator-of-c-bids-goodbye-world){:target="_blank"} in the book *[The C Programming Language](http://www.amazon.com/Programming-Language-Brian-W-Kernighan/dp/0131101633/){:target="_blank"}* (1978). The program is reproduced, below. In this example, `\n` is a new line character, meaning any further output would start on the next line.

{% highlight c linenos %}
main()
{
	printf("hello, world\n");
}
{% endhighlight %}


The book in question was written by Dennis Ritchie, creator of C, and Brian Kernighan while they were both working at the fabled AT&T Bell Labs. It is incredibly popular, having sold millions of copies and been translated into dozens of languages. Furthermore, C has been widely adopted and has influenced nearly all languages in use today. It is no wonder 'hello, world!' has made its way to computer screens all around the world.

While we now know how the phrase was popularized, this is not the beginning of the story. Kernighan wrote an internal memorandum while at Bell Labs, called [Programming in C: A Tutorial (1974)](https://www.bell-labs.com/usr/dmr/www/ctut.pdf){:target="_blank"}. In it was almost identical code:

{% highlight c linenos %}

main(){
	printf("hello, world");
}

{% endhighlight %}

Although Kernighan had nothing to do with creating C, he wrote a tutorial on it because he had also written a tutorial to the programming language B (the precursor to C) in 1972. In [that tutorial](https://www.bell-labs.com/usr/dmr/www/btut.pdf){:target="_blank"}, he included the following program:

{% highlight c linenos %}

main(){
	extrn a,b,c;
	putchar(a); putchar(b); putchar(c); putchar('!*n');
	}

a 'hell';
b 'o, w';
c 'orld';

{% endhighlight %}

This example illustrates the use of external variables, necessary since all character constants are limited to four ASCII characters. The code snippet above will piece together the variables a, b, and c to print `hello, world` We again see the new line character, now expressed as `*n`.

It is [further claimed](http://stackoverflow.com/questions/602237/where-does-hello-world-come-from){:target="_blank"} that 'hello, world' originated earlier, in [the manual to BCPL](http://www.fh-jena.de/~kleine/history/languages/Richards-BCPL-ReferenceManual.pdf){:target="_blank"} (a precursor to the B programming language). While this manual, and the language itself, was written by Kernighan and Martin Richards of Cambridge, I have yet to see such a program in it. For the time being, 1972 is the date to beat.

So now you know. The next time you see someone learning a new programming language or you see 'hello, world!' printed on a t-shirt, feel free to spread the word.

And of course, as this is my first post:

> hello, world!

