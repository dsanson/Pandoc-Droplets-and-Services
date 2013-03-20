Pandoc Droplets and Services
============================

This is a collection of OS X applets and system services, providing a
simple way to use [pandoc][] to convert documents between different
formats.

These wrappers don't expose the full power of pandoc. For that, you need
to open a terminal and use the command line. What they offer is easy
drag-and-drop conversion from supported input formats---mostly markdown,
but also rst, textile, html, latex, mediawiki, docbook, opml---to select
supported output formats---html, pdf, odt, docx, epub, slidy, dzslides,
and markdown.

Drag and Drop Applets
=====================

The applications in the Applets folder are drag-and-drop applets. Put
them in the Dock or on your desktop. Drag a file (or several files) onto
them to convert it. The resulting file(s) will be created in the same
folder as the original.

**Warning**: existing files with the same name and extension will be
overwritten.

System Services
===============

The files in the Services folder are [Services for the OS X Services
Menu][]. To install, double-click and follow the instructions, or just
copy them to `~/Library/Services/`. Services can be used from contextual
menus (right click on a markdown file in the Finder, go to Services, and
select the Service). The resulting file(s) will be created in the same
folder as the original.

**Warning**: existing files with the same name and extension will be
overwritten.

Requirements
============

Pandoc
------

You'll need to [install pandoc][], version 1.9 or later (earlier
The scripts for converting to PDF assume that your LaTeX executables are
in `/usr/texbin`, which is where BasicTeX and MacTeX put them. But if you have installed LaTeX somewhere else, you will need to modify the scripts or create appropriate symbolic links.
versions of pandoc have different command line options).

LaTeX
-----

Pandoc depends on LaTeX to convert to PDF. If you don't have LaTeX
installed, I recommend installing [BasicTeX][], which is only 69 MB.

Input Formats
=============

The scripts should work with any of the input formats pandoc supports,
but note that pandoc's support for some input formats is less robust
than others. The most robust input format is [pandoc's extended
markdown][].

The scripts make no attempt to determine the input filetype. They pass
the input filename to pandoc, and pandoc tries to infer the format from
the filename extension. If pandoc cannot infer the filetype from the
extension, it will treat it as markdown.

Conversion *To* Markdown
========================

`pandoc2markdown.app` and the `Convert to Pandoc using Markdown` Service
are both wrappers around my [`any2pandoc.sh`][] script. This script will
do its best to convert whatever you throw at it.

doc, docx, webarchive, rtf, rtfd, and odt files
-----------------------------------------------

Pandoc does not support these as input formats. So they are first
converted to html, using OS X's built-in `textutil` command, and then
converted from html to markdown using pandoc.

This usually works okay. Things like footnotes will *not* be preserved.

pdf
---

If you have [`pdftohtml`][] installed (I install it using [homebrew][]),
that will be used to convert PDF files to html, which is then converted
to markdown using pandoc.

This sometimes works okay, but often it does not.

tex
---

Files with the extension `.tex` are treated as latex files, and
converted by pandoc to markdown.

everything else
---------------

Everything else is passed to pandoc as is, and pandoc tries to infer the
format from the filename extension. This works well for the input
formats pandoc supports. Other formats will be treated as markdown.

Troubleshooting
===============

The scripts look for pandoc in `/usr/local/bin` and `$HOME/.cabal/bin`.
If your copy of pandoc is installed somewhere else, you'll either need
to modify the embedded scripts or put a symbolic link to pandoc in
`/usr/local/bin`,

    $ ln -s $(which pandoc) /usr/local/bin/pandoc

Depending on the permissions you have set for `/usr/local/bin`, you may
need to use this instead:

    $ sudo ln -s $(which pandoc) /usr/local/bin/pandoc

The scripts for converting to PDF assume that your LaTeX executables are
in `/usr/texbin`, which is where BasicTeX and MacTeX put them. But if
you have installed LaTeX somewhere else, you will need to modify the
scripts or create appropriate symbolic links.

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
  [Services for the OS X Services Menu]: https://www.macworld.com/article/1163996/how_to_use_services_in_mac_os_x.html
  [install pandoc]: http://johnmacfarlane.net/pandoc/installing.html
  [BasicTeX]: http://www.tug.org/mactex/morepackages.html
  [pandoc's extended markdown]: http://johnmacfarlane.net/pandoc/README.html#pandocs-markdown
  [`any2pandoc.sh`]: https://gist.github.com/1181510
  [`pdftohtml`]: http://pdftohtml.sourceforge.net/
  [homebrew]: http://mxcl.github.com/homebrew/
  [modify the embedded scripts]: #customization
