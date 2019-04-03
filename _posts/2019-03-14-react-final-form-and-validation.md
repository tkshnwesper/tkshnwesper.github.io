---
layout: post
title:  "React Final Form and Validation"
tags: [react, react-final-form]
categories: [programming]
---

# An example

Here's an example simulating the condition:

<iframe src="https://codesandbox.io/embed/624rzzxqxn?autoresize=1&codemirror=1&fontsize=14&hidenavigation=1" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

[![Edit 🏁 React Final Form - Validation errors example](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/624rzzxqxn?autoresize=1&codemirror=1&fontsize=14&hidenavigation=1)

# The context

This post is about how you could sometimes get unexpected error messages while using `react-final-form`. It is especially difficult to find the source of such errors due to the library's complexity. However, after studying the behavior a little bit, it becomes a little easier to understand what's going on.

It would also help you in designing your applications in a way that you do not encounter this problem.

# The problem

We have `initialValues` that have been assigned to the `react-final-form`'s `Form` component as:

```js
{
  bio: {
    first: "Jane",
    last: "Doe",
    age: 23
  }
}
```

So there's basically just one element at the root level which is `bio`.

`bio` itself is an object that has data within the `first`, `last` and `age` keys.

The manner in which the initial values populate the input fields as the page loads is different in `first` and `age` or even `last` and `age` for that matter. `first` and `last` are populated in the same way.

## Understanding the difference

Here's the `Field` definition for the first name.

```jsx
<Field name="bio.first" validate={required}>
  {({ input, meta }) => (
    <div>
      <label>First Name</label>
      <input {...input} type="text" placeholder="First Name" />
    </div>
  )}
</Field>
```

As you can see, the above `Field` has subscribed to `bio.first`'s value. Which means that, the value that this field is going to get is the `string` **Jane**.

Now let's look at how the age is rendered. We'll ignore the last name since it has been rendered in the same way as the first name (as mentioned before).

```jsx
<Field name="bio" validate={requiredAge}>
  {({ input, meta }) => (
    <div>
      <label>Age</label>
      <input
        onChange={(...props) =>
          input.onChange({
            ...input.value,
            age: props[0].target.value
              ? props[0].target.value
              : undefined
          })
        }
        value={input.value.age}
        type="text"
        placeholder="Age"
      />
    </div>
  )}
</Field>
```

Now you see something different, am I right? Unlike the first or the last names, the age's input field subscribes to `bio` itself! Whoa! Why would someone do this in the first place? Could have just subscribed to `bio.age` while we were at it.

Such questions are out of scope of this post, and moreover this is just an example :D

The point is that the age's `Field` gets something like the following as its value:

```js
{
  first: "Jane",
  last: "Doe",
  age: 23
}
```

It gets access to data that it doesn't have to know even. But that's something to discuss for another day. It does get access to the age data as well, and *that's* important. So we simply use the value by destructuring the object. Nothing new here.

## Validating

One thing we haven't considered so far is validating each of those fields. Well there is a validating function as well there. More like two actually. One specifically made for `age`.

Those validator functions don't do anything much other than checking whether a field is empty and invalidating the field and subsequently the form whenever a field is so.

There's a bit of understanding to be done first. Specially, the way in which `react-final-form` stores errors.

Supposing that the last name was empty, the validator would report this to RFF (`react-final-form`) and RFF would simply store it in this format:

```js
{
  bio: {
    last: "Required",
  }
}
```

Assuming that there are no other errors for the time being, what you see above is what the `errors` would look like. So RFF keeps track of errors on name subscription basis. That would mean that if two fields were subscribed to one name such as `bio.last` both of them would receive the same error prop.

I find that smart. However, this very capability of RFF to store its errors with respect to its name subscriptions, results in its demise if not used properly.

Well anyway, now let's move on to the interesting bit.

What would happen if we clear the age field instead? How would the error be stored then? Here's how:

```js
{
  bio: "Required",
}
```

Do you see the problem? No worries, I'll explain.

While it would be straight forward for the age field to get its own error message, what error message would the first and last fields get?

Maybe I should have mentioned this before, but RFF passes to the corresponding fields their respective error messages (based on the `errors`). So you could always have a look at the `errors` and know which field has got which error.

**Note**:
The `errors` are printed in the example so that you are be able to see what error you got for every different field.

In our case the first and last name fields are not able to receive their error messages (no reason for them to have one), but since you cannot really de-structure a `string` like an `object` into `first` and `last` by using a `.` (dot operator).

Even then, something like that is considered fine by RFF. The first and last name fields would get something like an `undefined` value in their `meta.error`.

## Inversion

Now you are probably thinking of the inverse, right? What error value would the age field get if we have errors in the first or last fields (or both)?

The answer is simple. Since the age field is subscribed to `bio` it will just get everything contained by it. So in the case where both first and last fields are empty and thereby erroneous, our `errors` would look something like the following:

```js
{
  bio: {
    first: "Required",
    last: "Required",
  }
}
```

Right?

So that would mean that the `meta.error` prop for the age field would receive:

```js
{
  first: "Required",
  last: "Required",
}
```

The important thing to remember here is that that value is **not** a `string`! It's an `object` type instead!

# Conclusion

I would not call this a library bug, because this is something that's part of the library's design. Instead of taking the time to fix an issue like this by redesigning certain aspects of the library, I think would be better off using it more carefully while keeping in mind that such a thing could happen in my code.

I only wrote this article because something like this has happened to me, where I made a change in one place and got a totally unexpected prop-type error elsewhere.

If you see a similar pattern somewhere, you probably know now the reasons behind how this error could have come about.
