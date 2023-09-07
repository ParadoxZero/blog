---
layout: post
title: Exceptions, good or bad?
date: 2023-09-07 
tags: general opinion
categories: code
author: Sidhin
---

Whether or not to use exceptions is always a topic of contention. Everyone has their own different opinion. But what exactly are those? And should I use them or not? 

## What are Exceptions? 
When a program is running, it can encounter a lot of unexpected or “exceptional” situations. For example, A file you are trying open doesn’t exist, or the contents are not what you expect. Or something more severe like ‘malloc’ fails to allocate memory, an important OS module crashed etc.

Writing production quality code means you must handle all these potential situations such that your program always behaves in a deterministic way. Which means, it should always behave in an expected way regardless of the situation. Which in turn means, the language must provide support to facilitate the said handling.

Exceptions or more accurately exception handling is one of the mechanisms which a lot of programing languages provide to handle these exceptional situations.

The principle behind is simple really – 
1.	There will be a keyword to raise or throw exceptions
2.	There will be keyword to handle them.
3.	Then there will a construct to pass along information regarding the exceptional situation.

Different languages handle them differently some notable examples – in python<link>, in C#<link>, in C++<link>.

In other words, Exceptions are a kind of control flow statements a lot of languages offer. Like ‘if-else’ or loops.

## So how do we handle failures?
Let’s take an example –
 
{% highlight cpp %}
list<string> get_lines_from_file(char *filepath) {
  FILE *f = fopen(filepath);
  string content = f->read();
  return parse_content(content);
}
{% endhighlight %}

The above snippet is a pseudocode example of a of a simple function to open a file, then read it’s content and parse it to get number of lines.

Now let’s just check from points of failure within the method.

{% highlight cpp %}
list<string> get_lines_from_file(char *filepath) {
  FILE *f = fopen(filepath);     // The file might not exists

  string content = f->read();    // Internal buffer might fail to allocate

  return parse_content(content); // parse might fail due to
                                 // invalid token, like CRLF
                                 // instead of LF
}
{% endhighlight %}
 
Now let’s see how to handle these potential failures such that our program will be more robust and behaves in expected ways irrespective of the conditions.

{% highlight cpp %}
RESULT get_lines_from_file(char *filepath, list<string> &list) {
  FILE *f = fopen(filepath);
  if (f != NULL) {
    string content;
    if (f->try_read(&content)) {
      return parse_content(content, list);
    }
  }
}
{% endhighlight %}

Now here, RESULT can be maybe numeric code or an Enum with different values for different kind of failures.

Example – 
```
0 -> Success
1 -> Access Denied
2 -> File not found
```

With a standardized constants for the failures you can pretty much handle all failures. But it’s not without cost. The cost is, your APIs and functions are no longer pretty. Since we’ll need to use the return values we need to use references and pointers in arguments to return the values to caller. This makes simple statements like below impossible-

`string content = f->read();`

Instead, it will always return the result. So no pretty, easy to read, assignment operations. That means, the caller of function will look something like this.

{% highlight cpp %}
// In the caller function
list<string> list;
RESULT res = get_lines_from_file("test.txt", list);
if (res != R_SUCCESS) {

  // Handle failure
  switch (res) {
  case R_FILE_NOT_FOUND:
    print("Oh no file not found!");
    break;
  case R_NO_MEMORY:
    print("Oh no you ran out of memory!");
    break;
  case R_INVALID_ARGS:
    print("Oh no your input is incorrect!");
    break;
  default:
    print("Failed.");
    break;
  }
}
{% endhighlight %}


Using Exceptions
The same code when using exception can easily look like this.

{% highlight c++ %}
list<string> get_lines_from_file(char *filepath) {
  FILE *f = fopen(filepath);
  string content = f->read();
  return parse_content(content);
}

// In the caller function
try {
  list<string> line_list = get_lines_from_file("test.txt");
} 
// handle failures
catch (FileNotFoundException) {
  print("Oh no file not found!");
} catch (InsuffecientMemoryException) {
  print("Oh no you ran out of memory!");
} catch (InvalidInputException) {
  print("Oh no your input is incorrect!");
} catch (Exception) {
  print("Failed");
}
{% endhighlight %}

Notice that the get_lines function can remain unchanged from the unsafe version, with only modification being required in the caller side.

The handling part of exception looks pretty similar to handling return status. Infact the difference between the two would
be about being explicit in returning the status in one case (return status) vs being implicitly done in case of exception.

## Exceptions look perfect. Right?
Exceptions solve a lot of problems, but also introduces new ones. Knowing the pros and cons of exception approach is important since that will help you make the right choice when you encounter different situations.

The biggest problems with exceptions in my opinion are – 
* Readability of non-golden path is horrible, i.e., finding out where is a certain exception being handled. Or whether it's
being handled at all. Because the control flow jumps out to an arbitary place somewhere down the stack. And it might not always be explicitly defined.
* Debugging code that heavily uses exception can be hard. One minute you are stepping over line by line, sudennly you are traveling down the stack.
* Very easy to misuse exception causing stack traces to be worthless. Especially when you incorrectly rethrow exceptions, the stack trace becomes reset and you lose all information about the original exception.
* Very easy to forget error handling causing bugs and crashes later. Since there is no strongly typed information around exceptions, it's very easy to forget to handle them in code.
