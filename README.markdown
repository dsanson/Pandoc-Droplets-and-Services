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
the cd "${file%/*}"
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
