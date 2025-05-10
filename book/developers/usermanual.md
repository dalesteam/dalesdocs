# User Manual

The DALES documentation is built using [Jupyter Book](https://jupyterbook.org/en/stable/intro.html). In short, Jupyter Book allows you to easily build a website from Markdown files and/or Jupyter Notebooks. This page will explain the very basics of adding new content to the documentation.

## Prerequisites

Since Jupyter Book is a Python package, you will obviously need a Python distribution (preferably, a more recent version, say >= 3.10). Additionally, a fresh environment made with you favorite environment manager is recommended to prevent conflicts with other packages. The required packages to build te documentation are listed in `requirements.txt` in the root directory of the `dalesdocs` repository. These can be installed using `pip`:

```
pip install -r requirements.txt
```

## Writing content

Content can be provided in two formats: Markdown or Jupyter Notebooks. The former is the most straight-forward format to use and suitable if your content purely consists of text and images. On the other hand, Jupyter Notebooks are useful if you want to include, for example, Python code or Bash commands in your documentation. Jupyter Notebooks are executed during the build process of the website.


```{admonition} Note
:class: tip
If you use any Python packages outside the standard library, please make sure they're added to `requirements.txt`.
```

### MyST-Markdown
Jupyter Book supports a special flavour of Markdown: [MyST](https://mystmd.org). MyST is an extension of regular Markdown and provides the ability to include equations, tables, references, code blocks with syntax highlighting, figure captions, and much more, allowing you to make your content much more lively. For example, we can include a standard LaTeX equation (with label) like this:

````
```{math}
:label: pythagorean-theorem
a^2 = b^2 + c^2
```
````

which results in:

```{math}
:label: pythagorean-theorem
a^2 = b^2 + c^2
```

Then, we can refer to it in-text with the following syntax: `[](#pythagorean-theorem)`, resulting in: [](#pythagorean-theorem). For a complete overview of what MyST can do, see the [documentation](https://mystmd.org/guide/).

### Updating the table of contents
Finally, to make your content actually show up on the website, you need to add the file(s) to the table of contents(`book/_toc.yml`). The table of contents is relatively self-explainatory, but for more information you can consult the [Jupyter Book documentation](https://jupyterbook.org/en/stable/structure/toc.html) or the [Teachbooks manual](https://teachbooks.tudelft.nl/jupyter-book-manual/basic-features/jupyterbook.html#the-table-of-contents).

## Building the documentation
To build the website locally for reviewing the new content, invoke the Jupyter Book build command in the root directory of the repository:

```
jupyter book build book/
```

If the build was succesful, you will get some output that looks something like this:

```
===============================================================================

Finished generating HTML for book.
Your book's HTML pages are here:
    book/_build/html/
You can look at your book by opening this file in a browser:
    book/_build/html/index.html
Or paste this line directly into your browser bar:
    file:///path/to/repository/dalesdocs/book/_build/html/index.html            

===============================================================================
```

Now you can, like the output says, review the manual locally by pasting the provided link in your browser.