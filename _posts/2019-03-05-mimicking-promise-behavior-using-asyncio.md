---
layout: post
title:  "Mimicking promise behavior using asyncio"
categories: programming
---

Python is a fascinating language. I have learnt many a new things while working on it, and a few surprises have come along the way as well. `asyncio` was definitely one of them. Perhaps because I had not seen anything like this before in Python. I've used Javascript extensively and understand the behavior of JS promises well. So I had a natural tendency to make things work in a similar manner in Python as well. However, looking at the documentation did not paint a clear picture in my head. Nor did reading up a couple of articles on the web, so I was forced to do a little experimentation of my own and find my way around this.

Note: I am still not very comfortable using `asyncio` and frankly I have no idea when to use `await` because everytime I use it, the interpreter throws an error of some kind.

Well anyway, I'll show ya what I have learnt so far.

Let's say we have an ordinary python `async def` function that does nothing much but returns a string.

```python
async def return_hello():
    print('executes once')
    await asyncio.sleep(2)
    return 'hello_world'
```

Another thing I have found is, I need to always create an event loop to get things to work. So let's get to work!

```python
event_loop = asyncio.new_event_loop()
```

Now we need to set this event loop (no idea what this does, check the documentation I suppose).

```python
asyncio.set_event_loop(event_loop)
```

Now comes the fun part. We create a task that executes the coroutine `return_hello`.

**Important**: This task starts executing the coroutine the instant we create it. This means that anything we execute after creating it will run in a parallel fashion.

```python
task = asyncio.ensure_future(return_hello())

print('runs before anything else')
```

If we print something after the above line, it might get executed before the first line in the coroutine.

If we would like to obtain the result of our "promise", then all we got to say is:

```python
hello = asyncio.get_event_loop().run_until_complete(task)

print(hello)

assert hello == 'hello_world'
```

The above line blocks the thread till the response arrives. It can be in the same thread or any other thread as well.

Now let me explain how this is closely related to how promises behave. Promises essentially store their resolved or rejected result after resolution or rejection. That means that at any point in time, one may `.then` them and get that result. Similarly, using tasks we are able to achieve this same caching behavior.

It is helpful when there is certain data that is needed by different threads of execution and you do not want to fetch the data multiple times in those threads. You only want to fetch it once and make those threads hungrily wait for that data to finally resolve before they can consume it.

The full program is as follows:

```python
async def return_hello():
  print('executes once')
  await asyncio.sleep(2)
  return 'hello_world'

event_loop = asyncio.new_event_loop()
asyncio.set_event_loop(event_loop)

task = asyncio.ensure_future(return_hello())

print('runs before anything else')

hello = asyncio.get_event_loop().run_until_complete(task)

print(hello)

assert hello == 'hello_world'
```

The output might be:

```ShellSession
runs before anything else
executes once
hello_world
```
