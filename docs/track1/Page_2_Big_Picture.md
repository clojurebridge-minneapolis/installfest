
# Chapter 2: Big Picture

### Clojure, the Big Picture

Clojure is a modern Lisp, which is a programming language, with a focus on functional programming. There are lots of programming languages, and Clojure is just one of them. The image below shows similar programming languages grouped by color. Clojure is in the bottom half in the center; it is a dialect of the Common-Lisp language on the far right.

<img src="http://griffsgraphs.files.wordpress.com/2012/07/programming-languages_label.png"/>


#### Clojure is great because:

* The core language is small and easy to learn.
* The design makes it easy to write correct programs.
* Clojure makes it easier to write concurrent programs &mdash; programs that do more than one thing at a time.
* Clojure programs are fast.
* Clojure programs can build on Java libraries.

Most programmers have to use multiple languages to get their jobs done.  Web applications often use HTML, CSS, and JavaScript.  We'll touch on each of those as we build our web app.

### The Web: a basic overview

The Internet is a bunch computers all over the world communicating with
each other using a variety of computer programs.  Some of those programs
are servers that listen for requests and respond with data.

Your web browser is a program that sends requests over HTTP (HyperText Transfer Protocol). Entering, [https://github.com](https://github.com) into your browser's address bar tells your browser that you want to see that page.

[https://github.com](https://github.com) is actually a human-readable alias for the numerical address of GitHub's servers.  Since your computer isn't directly connected to GitHub, your computer (through your web browser) asks the computers connected to GitHub to forward the request. This request may be passed through various computers to retrieve the data (web page).

On Linux or a Mac, the command `traceroute` shows you the number of hops request takes to get to GitHub.  (On Windows, the command is called `tracert`.)

On my machine, working from home:

        $: traceroute github.com
        traceroute to github.com (192.30.252.130), 30 hops max, 60 byte packets
        1  192.168.1.1 (192.168.1.1)  5.913 ms  5.908 ms  6.000 ms
        2  * 96.120.49.33 (96.120.49.33)  30.033 ms  30.066 ms
        3  te-0-3-0-1-sur02.webster.mn.minn.comcast.net (68.85.167.149)  30.553 ms  32.776 ms  32.785 ms
        4  te-0-7-0-12-ar01.roseville.mn.minn.comcast.net (69.139.219.134)  32.778 ms  36.059 ms te-0-7-0-13-ar01.roseville.mn.minn.comcast.net (68.87.174.185)  35.686 ms
        5  he-1-13-0-0-cr01.350ecermak.il.ibone.comcast.net (68.86.94.81)  42.354 ms *  43.213 ms
        6  be-10206-cr01.newyork.ny.ibone.comcast.net (68.86.86.225)  63.574 ms  59.894 ms  62.441 ms
        7  pos-2-5-0-0-cr01.dallas.tx.ibone.comcast.net (68.86.85.25)  64.643 ms  64.659 ms  64.653 ms
        8  he-3-14-0-0-cr01.dallas.tx.ibone.comcast.net (68.86.85.1)  67.039 ms  67.079 ms  67.056 ms
        9  he-0-10-0-0-pe07.ashburn.va.ibone.comcast.net (68.86.83.66)  63.013 ms  67.045 ms  88.273 ms
        10  * * *
        11  * * *
        12  * * *
        13  * * *
        14  * * *
        15  * * *
        16  * * *
        17  * * *
        18  * * *
        19  * * *
        20  * * *
        21  * * *
        22  * * *
        23  * * *
        24  * * *
        25  * * *
        26  * * *
        27  * * *
        28  * * *
        29  * * *
        30  * * *

+ Line 1 is my computer server address.
+ Line 2 is my router server address.
+ Lines 3-30, the request is moving through Comcast's network of servers to get the information.

When you enter [https://github.com](httpss://github.com) in the address bar, your browser makes a GET request to the GitHub server. There are several types of HTTP requests, but GET is the one that asks the server to send data from a specified resource.  The server sends back an HTML page and the Web browser turns the HTML into the web page you see through a process called rendering. You can see the HTML by right-clicking on the page and selecting `View Page Source`.


### HTML Proper


HTML stands for "HyperText Markup Language".

Hypertext means documents can contain links to other pages or images. The structure of the HTML document is encoded using a markup language consisting of opening and closing elements (also called tags). Each segment of text is formatted according to the type of tag (`<body>`, `<title>`, `<h2>`, etc). Each segment of text needs an opening tag, `<body>`, and a closing tag, `</body>`. The `/` signifies the end of the formatting for that segment.

#### Let's see it in action
Open a text editing program and enter the following text: 

```HTML
<!DOCTYPE html>
<html>
  <head>
    <title>Sample HTML Page</title>
  </head>
  <body>
    This is a sample html page.  This is more text.
  </body>
</html>
```

Save the file as 'sample.html'. Then open the file in the web browser. On Windows or a Mac, double-click the file to open it in the default browser, or right-click and select `Open With`. In your browser you should see something like this,

![](https://github.com/clojurebridge-minneapolis/track1-chatter/raw/master/images/sample%20html.png "sample html")


Notice the address bar. Instead of making an HTTP request to a server over the Internet, the browser  opened a local file and displayed the HTML. Remember, the web browser renders the HTML to make it appear as you see it in the browser. Right-click the page and select `View Page Source` to see the HTML elements of the file you saved.

The first line of the source, the DOCTYPE, announces the document is HTML.  The text is enclosed between the _opening_ `<html>` and _closing_ `</html>` tags, or elements.

##### Titles, Headers, & Body Text
Inside the HTML document, we have a `head` and `body`. These are both _elements_ or _tags_. The `head` contains the `title`; in our example it's "Sample HTML Page". The title is the name of the web page. You will see this text as the tab name and when you bookmark the page in your browser. It is not actually a part of the text on the page.

To add a title on the page itself, add it within the `body` of the HTML using a header tag. HTML uses various heading tags to indicate the size of the title or heading. The largest is `<h1>` and the smallest header is `<h6>`.

Let's add some headers to our example. In your text editing file, add `<h2> Our HTML </h2>` in the `body` of the HTML. This will display "Our HTML" as a title. Your text file will look like this:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Sample HTML Page</title>
  </head>
  <body>
  <h2>Our HTML</h2>
    This is a sample html page.  This is more text.
  </body>
</html>
```


Save the file and refresh the browser. Experiment with different size heading tags from `<h1>` to `<h6>`. Remember to open and close the HTML tags, meaning you must surround the header text with the an opening and closing tag.

##### Tables
HTML supports tables as well. Add a table to your sample HTML by adding the following within the `<body>` tags of the text file:


```HTML
<table>
  <tr>
    <td>Hydrogen</td>
    <td>1</td>
    <td>H</td>
  </tr>
  <tr>
    <td>Helium</td>
    <td>2</td>
    <td>He</td>
  </tr>
  <tr>
    <td>Neon</td>
    <td>10</td>
    <td>Ne</td>
  </tr>
</table>
```

![](https://github.com/clojurebridge-minneapolis/track1-chatter/raw/master/images/HTML%20table.png "HTML table")

`table` encloses the entire table.

`tr` wraps a table row.

`td` wraps table data, creating a cell within a row.

When you save and refresh the page, you see the table. You might notice it also looks pretty bad. The HTML we've been using describes the basic structure of the document and leaves the display entirely up to the browser. Another language, called CSS, is used so the browser can display the page in a more pleasing way.  We'll touch on CSS later. 

The HTML file we have is _static HTML_. The HTML we see in the web browser is simply the code we have written in the text file. Static HTML works great for some kinds of pages, but our page will change depending on the messages people post to it.
So instead of having a file with HTML, we will have a program listening for requests that generate the HTML. As people make requests and post messages, it will generate HTML that reflects the posts.

There's a lot more to HTML, but this gives us enough knowledge for our Chat app.

In the next section, [Chapter 3: Starting the Project](Page%203_Start%20project.md), we'll start coding our Chatter app.



####More HTML Resources

[w3schools.com](http://www.w3schools.com) is a great source for
learning more about html. Start with the
[HTML Introduction](http://www.w3schools.com/html/html_intro.asp).

[Mozilla Developer Network](http://developer.mozilla.org/) is another useful resource.
