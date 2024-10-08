## Pandoc for Windows 
### Installing and working with Pandoc 

#### Sources
**Pandoc User's Guide** - <https://pandoc.org/MANUAL.html> <br>
<https://pandoc.org/installing.html> <br>
<https://pandoc.org/getting-started.html> <br>
**Pandoc's Markdown** - <https://allefeld.github.io/nerd-notes/Markdown/A%20writer's%20guide%20to%20Pandoc's%20Markdown.html> <br>
**Pandoc Examples** - <https://pandoc.org/demos.html> <br>
**Try pandoc online** - <https://pandoc.org/demos.html> <br>
**Using Pandoc with GitHub Actions** - <https://github.com/pandoc/pandoc-action-example> <br>

#### About Pandoc
Pandoc is a Haskell library for converting from one markup format to another, and a command-line tool that uses this library. <br>
Pandoc can convert between numerous markup and word processing formats, including, but not limited to, various flavors of *Markdown*, *HTML*, *LaTeX* and *Word docx*.

## 1. Install
### Go to the website and select the version, that you want to install
<https://docs.npmjs.com/downloading-and-installing-node-js-and-npm> 


## 2. Open PowerShell
* __Important Note:__ Pandoc is a command-line tool. There is no graphic user interface. So, to use it, you’ll need to open a terminal window.


### Verify Installation
* Type:
```json
pandoc --version
```

## 3. Changing Directories
* Go to a directory you would like to create a new folder. In the following example, you go to the Documents directory:
```json
cd Documents
```

* The following shows your current directory:
```json
pwd
```

* Create a subdirectory, type:
```json
mkdir pandoc-test
```

* Change to the *pandoc-test* directory:
```json
cd pandoc-test
```

## 4. Using pandoc as a filter
* In the *PowerShell*, type:
```json
pandoc
Hello *pandoc*!

- one
- two
``` 
and press *Ctrl-Z* followed by *Enter*. <br>

You should now see your text converted to HTML:
```json
<p>Hello <em>pandoc</em>!</p>
<ul>
<li>one</li>
<li>two</li>
</ul>
``` 
__Important Note:__ When pandoc is invoked without specifying any input files, it operates as a “filter,” taking input from the terminal and sending its output back to the terminal. You can use this feature to play around with pandoc. <br>
By default, input is interpreted as pandoc markdown, and output is HTML.

* Converting from *HTML* to *markdown*
In the *PowerShell*, type:
```json
pandoc -f html -t markdown
```
After that, type:
```json
<p>Hello <em>pandoc</em>!</p>
```
Don't forget pressing *Ctrl-Z*, followed by *Enter*

You should see:
```json
Hello *pandoc*!
```

## 5. Text editor basics
* Create a *md* file, named **test1.md** in the directory *pandoc-test. <br>
(I created the *test1.md* in *Visual Studio Code*)

* Type the following in the *test1.md* and save the file:
```json
---
title: Test
...

# Test!

This is a test of *pandoc*.

- list one
- list two
```
* In the terminal, type `dir` and you should see a list of the current directory, and the file you have already created.

* To convert it to *HTML*, use the command:
```json
pandoc test1.md -f markdown -t html -s -o test1.html
```
* In the directory, you can see the converted *html* file. Type `.\test1.html` to open it in the browser and see the document.

![No Image to show](pandoc.png)

__Important Note:__ If you get the error `error: This document format requires a nonempty <title> element.
 Defaulting to 'pandoc' as the title.
 To specify a title, use 'title' in metadata or --metadata title="...".` , it indicates that the markdown document does not have a title element. <br>
 To resolve this issue, add a title to the markdown document:
 ```json
 pandoc test1.md -f markdown -t html -s --metadata title="My Title" -o test1.html
 ```
Replace **My Title** with the desired title for the HTML document.

## 6. Pandoc's Markdown
Pandoc understands an extended and slightly revised version of John Gruber’s Markdown syntax.

## 7. Markdown Extensions
The behavior of some of the readers and writers can be adjusted by enabling or disabling various extensions.
* An extension can be enabled by adding `+EXTENSION` to the format name and 
*  disabled by adding `-EXTENSION`. <br>

For example: <br>
`--from markdown_strict+footnotes` - is strict Markdown with footnotes enabled <br>
`--from markdown-footnotes-pipe_tables`-  is pandoc’s Markdown without footnotes or pipe tables.




















