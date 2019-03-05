---
layout: post
title:  "Applying rust macros"
categories: programming
---

Rust macros can be a little elusive mostly because of the lack of practical guides that I found on them. I had to look through the various Rust documents and read the [Little book on Rust macros][Little book on Rust macros] before I could finally start to contemplate writing actual code. Even as I wrote code, I faced a couple of problems in trying to understand the tiny language semantics because I didn't find a step-by-step explanation on how to use them. It seemed a bit like pushing an amateur into a swimming pool and expecting them to figure out how to swim.

But overall it felt like a satisfying experience once I got things working. So let's begin!

# Conventions

For now we will cover one of the macro types in Rust which are defined by `macro_rules`. The convention is to define macros as follows:

{% highlight rust %}
macro_rules! name_of_your_macro {
  ( /* rule 0 */ ) => { /* rule 0's definition */ };
  ( /* rule 1 */ ) => { /* rule 1's definition */ };
  // ...
  ( /* rule n */ ) => { /* rule n's definition */ }; // optional semi-colon at the end of the last line
}
{% endhighlight %}

Calling the macro might be a little familiar if you have written Rust code before, anyway here is a refresher on how the above macro could be called:

{% highlight rust %}
name_of_your_macro!(/* arguments */);
{% endhighlight %}

{% highlight rust %}

{% endhighlight %}

[Little book on Rust macros]: https://danielkeep.github.io/tlborm/book/README.html