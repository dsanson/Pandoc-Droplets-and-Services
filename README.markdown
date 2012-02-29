These are some simple-minded Automator wrappers around shell script wrappers
around [pandoc][], aimed at making it easy to make simple use of pandoc
in OS X.

Drag and Drop Applets
=====================

The applications in the Applets folder are drag-and-drop applets. Put
them in the Dock or on your desktop. Drag a file (or several files) onto
them to convert. You can use this README as a test.

System Services
===============

The .workflow files in the Services folder are services. To install,
double-click and follow the instructions, or just copy them to
`~/Library/Services/`.

Requirements
============

To use these, you need to install pandoc, version 1.9 or later. If you
use the OS X installer, pandoc will be installed to `/usr/local/bin`; if
you install from source using cabal, pandoc will likely be installed in
`~/.cabal/bin`. The scripts look in both places, and give priority to
`~/.cabal/bin`. 

Pandoc depends on LaTeX to convert to PDF. If you don't have LaTeX
installed, I recommend installing [BasicTeX][], which is only 69 MB. The
scripts assume that xetex is in `/usr/texbin`, which is where the
BasicTeX installer puts it.

If you have an earlier version of pandoc, or if your version of pandoc
or your Tex executables are in some other place, you'll need to modify
the embedded scripts.

Input Formats
=============

The scripts should work with any of the input formats pandoc supports,
but note that pandoc's support for some input formats is less robust
than others; the best support is for markdown.

The scripts pass the input filename to pandoc, and pandoc tries to guess
the format from the extension. Files with non-standard extensions will
be treated as markdown.

Customization
=============

Open the applets or the workflow files in Automator to customize them.
The embedded scripts are simple. Making additional applets or services
for other output formats, or for some specific set of options, is
trivial.

Here is a commented example of the script embedded in `pandoc2pdf.app`:

~~~~ {.bash}
PATH=$HOME/.cabal/bin:/usr/local/bin:/usr/texbin:$PATH

for file in "$@"
do
    output=${file%%.*}.pdf
    pandoc "$file" -o "$output" --latex-engine xelatex
done
~~~~


  [pandoc]: http://johnmacfarlane.net/pandoc/
  [BasicTeX]: http://www.tug.org/mactex/morepackages.html
