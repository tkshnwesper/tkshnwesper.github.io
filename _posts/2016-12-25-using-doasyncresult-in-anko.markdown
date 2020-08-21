---
date: 2016-12-25 20:34:57+00:00
layout: post
slug: using-doasyncresult-in-anko
title: Using doAsyncResult in Anko
categories:
- Hacks
---

I was stuck on this for a little while since it wasn't updated in the documentation and also since I am still a newbie at Kotlin ;)

```kotlin
class PopularActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        listView {

            id = ViewID.ID_POPULAR_LIST

          // This task will happen asynchronously in a separate thread (so you can perform network operations)
          // and the returned value can be accessed through the Future object
          // f.get() blocks till the task is finished
            fun getTitles() : ArrayList<String> {
                val arr = ArrayList<String>()
                // add elements to list
                return arr
            }

          // so yeah, this is the tricky part (for me at least) since it wasn't updated in the documentation. Enjoy! :)
            val m: (AnkoAsyncContext<ListView>.() -> ArrayList<String>) = {
                ::getTitles.invoke()
            }
            val f : Future<ArrayList<String>> = doAsyncResult(null, m)

            adapter = ArrayAdapter<String>(ctx, android.R.layout.simple_list_item_1, f.get())
        }
    }
}
```

Some of the description has been already added in the code, as you can see. Let me explain the premise of this solution in any case. The problem arose because I had to perform a network task in the `onCreate` method and as you know we can't normally perform a network task in the `uiThread`. Luckily, Anko provides an elegant solution to this without having the need to implement an `AsyncTask` interface. `doAsync` runs whatever task you give it but does not return anything. However, although `doAsyncResult` is similar to `doAsync` it can additionally return an object.

A Merry Christmas to you!
