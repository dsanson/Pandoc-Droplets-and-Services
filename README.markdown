% Pandoc Droplets and Services

These are some simple Automator wrappers around shell script wrappers
around [pandoc][]. Pandoc is a powerful command-line tool for converting
between different document formats. These wrappers don't expose the full
power of pandoc, but they allow for easy drag-and-drop conversion from
supported formats to pdf, odt, docx, dzslides, and (extended) markdown
formats.

Drag and Drop Applets
=====================

The applications in the Applets folder are drag-and-drop applets. Put
them in the Dock or on your desktop. Drag a file (or several files) onto
them to convert. The resulting file(s) will be created in the same
folder as the original. **Warning**: existing files with the same name
and extension will be overwritten.

System Services
===============

The files in the Services folder are Services for the OS X Services
Menu. To install, double-click and follow the instructions, or just copy
them to `~/Library/Services/`. Services can be used from contextual
menus (right click on a markdown file in the Finder, go to Services, and
select the Service). They can also be used from within applications like
[Quicksilver][]. The resulting file(s) will be created in the same
folder as the original. **Warning**: existing files with the same name
and extension will be overwritten.

Requirements
============

You'll need pandoc, version 1.9 or later (earlier versions of pandoc
have different command line options). If you install pandoc with the OS
X installer, pandoc will be installed to `/usr/local/bin`; if you
install from source using cabal, pandoc will likely be installed in
`~/.cabal/bin`. The scripts look in both places, and give priority to
`~/.cabal/bin`. If your copy of pandoc is installed somewhere else,
you'll either need to modify the embedded scripts or put a symbolic link
to pandoc in `/usr/local/bin`,

    $ ln -s `which pandoc` /usr/local/bin/pandoc

Pandoc depends on LaTeX to convert to PDF, so if you want to convert to
PDF, you will need a working LaTeX installation. If you don't have LaTeX
installed, I recommend installing [BasicTeX][], which is only 69 MB. The
scripts assume that your LaTeX executables are in `/usr/texbin`, which
is where BasicTeX and MacTeX put them.

Input Formats
=============

The scripts should work with any of the input formats pandoc supports,
but note that pandoc's support for some input formats is less robust
than others.

The scripts make no attempt to determine the input filetype. They pass
the input filename to pandoc, and pandoc tries to infer the format from
the filename extension. If pandoc cannot infer the filetype from the
extension, it treats it as markdown.

Conversion *To* Markdown
========================

`pandoc2markdown.app` and the `Convert to Pandoc using Markdown` Service
are both wrappers around my [`any2pandoc.sh`][] script. This script will
do its best to convert whatever you throw at it.

doc, docx, webarchive, rtf, rtfd, and odt files
-----------------------------------------------

These are first converted to html, using OS X's built-in `textutil`
command, and then converted to markdown using pandoc.

This usually works okay. Things like footnotes will *not* be preserved.

pdf
---

If you have [`pdftohtml`][] installed (I install it using [homebrew][]),
that will be used to convert PDF files to html, which is then converted
to markdown using pandoc.

This sometimes works okay, and sometimes works not at all.

tex
---

Files with the extension `.tex` are treated as latex files, and
converted by pandoc to markdown.

everything else
---------------

Everything else is passed to pandoc as is, and pandoc tries to infer the
format from the filename extension. This works well for the input
formats pandoc supports. Other formats will be treated as markdown.

Customization
=============

Open the applets or the workflow files in Automator to customize them.
The embedded scripts are simple. Making additional applets or services
for other output formats, or for some specific set of options, is
trivial.

Here is an example of the script embedded in `pandoc2pdf.app`:

~~~~ {.bash}
PATH=$HOME/.cabal/bin:/usr/local/bin:/usr/texbin:$PATH

for file in "$@"
do
    output=${file%%.*}.pdf
    pandoc "$file" -o "$output" --latex-engine xelatex
done
~~~~

  [pandoc]: http://johnmacfarlane.net/pandoc/
  [Quicksilver]: http://qsapp.com
  [BasicTeX]: http://www.tug.org/mactex/morepackages.html
  [`any2pandoc.sh`]: https://gist.github.com/1181510
  [`pdftohtml`]: http://pdftohtml.sourceforge.net/
  [homebrew]: http://mxcl.github.com/homebrew/
