
# Why use (and how to configure) Pan-Book-Machine

Pan-Book-Machine was built to make it fairly easy to create good documents (PDF, ePUB, DOCX, HTML)
from markdown using the (great) PanDoc application.

The idea behind this set of scripts was:
* Render different output formats from the same source
* Configure metadata and global configuration for your book in one place
* Allow tweaking for different formats in a separate config file for each

Visit
[https://pandoc.org/MANUAL.html#pandocs-markdown](https://pandoc.org/MANUAL.html#pandocs-markdown) 
for more information on the PanDoc markdown.
Try clicking on this URL even if you are inside a PDF, it should open your browser.

You can also find a chapter at the end of this manual where I collect links regarding PanDoc
tutorials, tricks, information.

## Quickstart

### Start a new book

To create a new book, **go into the `pan-book-machine` and type:**

~~~{.cpp}
./cli_createBook.sh
~~~

Here you can provide

* the folder name for the new book
* decide if you want sample content (possibly a good idea if you want to play with pan-doc-machine)

**No edit the file inside the newly created folder:**

~~~
CONFIG/book.conf
~~~

Each option (like PDF or DOCX you can set to TRUE or FALSE)

**Finally, edit the file:**

~~~
CONFIG/metadata-info.yaml
~~~

To make the output match your information (title and the like)

### Edit the content of your new book

All the markdown files with an ending `.md` are merged into one big file in alphanumerical order.

Then this one master file is used to create the book.

**Including images**

If you want to use images in a subfolder, do so inside the `CONTENT` folder. 
`pan-book-machine` will work with relative paths from inside that folder.

### Create formatted book

**Inside your book folder, run the command:**

~~~
./cli_pandocEbook.sh
~~~

The resulting files will be placed in the book directory. 

## Configuring Pan-Book-Machine

The *global* configuration for all books can be found in the two files:

* `CONFIG/book.conf`
* `CONFIG/metadata-info.yaml`

These two files contain information that affect *ALL* output formats (such as file name or title of the book).
The only exception is the *table of contents* variable `TOCDEPTH` which only affects PDF / LaTeX. 
But I wanted to keep this close to the global TOC setting, which decides IF there is a table of contents at all.

There are a number of format specific configuration files such as:

* `CONFIG/metadata-pdf.yaml`
* `CONFIG/metadata-epub.yaml`

## ePUB cover (e-book)

Creating an ePUB for eReaders allows to specify a cover image. 
And I suggest you do, because it makes it easier to find in your eReader.
Currently, Pan-Book-Machine expects the file:

* `CONFIG/coverEpub.jpg`

If you want to replace the cover image in our ePUB, replace this image.
If you do not want a cover image, open the file:

* `CONFIG/metadata-epub.yaml`

Delete the line:

~~~
cover-image: ../CONFIG/coverEpub.jpg
~~~

... or change it into some non-sensical variable by adding a hash:

~~~
#cover-image: ../CONFIG/coverEpub.jpg
~~~

NOTE: while JPEG is not officially an ePUB file format for images (PNG is), all readers support this format and the files are much smaller.

## Document language

Settings the language is important because:
* hyphenation at the end of the line (in e.G. PDF)
* additional wording (like "Contents" or "Figure" in PDF)

The language can (and should) be set inside the file `CONFIG/metadata-info.yaml`

~~~
lang: 'en'
~~~

See PanDoc Manual for more information: [https://pandoc.org/MANUAL.html#language-variables](https://pandoc.org/MANUAL.html#language-variables)

### Smart quotes in TeX

It's possible to include `{csquotes}` in the yaml metadata block for PDF.
Below is an example which would need to be added in the file 
`CONFIG/metadata-pdf.yaml`.

~~~
header-includes:
    - \usepackage{csquotes}
~~~

The language MUST be set in the same file, e.g. like this for German:

~~~
lang: "de"
~~~

or this, it's also German:

~~~
lang: "de-DE"
~~~

Does it work? 
"Here is an example" or another one 'here'. Let's see the single quote inside a word (beginning of this sentence).

And what if there " is a space " between letters and double quotes?

## Images

You can find more information on images in the file:

* `CONTENT/Images.md.off`

Rendering PDFs with images takes much more CPU and longer than text-only books.
This is why the file is named `Images.md.off`.
Because this file has the file ending `off` it does not show up in the created book if you run the script.

If you want to play with images, create a new book with the sample content 
using the instructions in the **Quickstart** section above.
Then rename `Images.md.off` to `Images.md` and create the book.
