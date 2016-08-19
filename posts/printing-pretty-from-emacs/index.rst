.. title: Printing pretty from Emacs
.. slug: printing-pretty-from-emacs
.. date: 2016-08-18 13:29:22 UTC-07:00
.. tags: emacs, orgmode, latex
.. category: emacs 
.. link: 
.. description: 
.. type: text

Emacs (The One True Editor) and Orgmode are both awesome tools and a fantastic
pairing and also an infuriating, fiendishly complex system. But still lovable.
For printing or -- more likely -- producing PDFs, Orgmode naturally 
comes with a working LaTeX exporter. The default output from it is,
well, if you've seen default Latex output, not very pretty. Not only does it use
traditional Latin Modern fonts, but things like hyperlinks are rendered in
hideous red boxes. 

Of course, this being Emacs and Org, this can be fixed. Here's my current
setup.


First of all, let's use XeLaTeX for fonts and Unicode support. LatexMk will know
how many times the source needs to processed.

.. code:: lisp

   (setq texcmd "latexmk -xelatex")
   (setq org-latex-pdf-process (list texcmd))

          
Since I don't write formal articles, I don't usually need a table of contents or section
numbers. If I do, I can set them up in the Org file header. 

.. code:: lisp

   (setq org-export-with-toc 'nil)
   (setq org-export-with-section-numbers 'nil)

Import fontspec for easy loading of fonts, and microtype because why not.
ox-latex needs to be loaded so the alist variables are available.

.. code:: lisp
   
   (require 'ox-latex)
   ;; these packages will be used by all org latex exports
   (add-to-list 'org-latex-packages-alist '("" "fontspec" nil))
   (add-to-list 'org-latex-packages-alist '("" "microtype" nil))

          
Now, define an export class that uses nice fonts and non-eye-bleeding
hyperlinks.

.. code:: lisp

  (add-to-list 'org-latex-packages-alist '("usenames,dvipsnames" "color" nil))
  (add-to-list 'org-latex-classes
          '("sjj-org-article"
          "\\documentclass[10pt,letterpaper]{scrartcl}
  [DEFAULT-PACKAGES]
  [PACKAGES]
  \\setromanfont{TeX Gyre Pagella}
  \\setsansfont{Linux Biolinum O}
  \\setmonofont[Scale=0.9]{DejaVu Sans Mono}
  \\pagestyle{empty}
  \\hypersetup{
  colorlinks = true, % colored links, no hideous boxes 
  urlcolor   = MidnightBlue, % external hyperlinks
  linkcolor  = PineGreen, % internal links
  citecolor  = PineGreen  % citations
  }
  \\title{}
  [EXTRA]"
          ("\\section{%s}" . "\\section*{%s}")
          ("\\subsection{%s}" . "\\subsection*{%s}")
          ("\\subsubsection{%s}" . "\\subsubsection*{%s}")
          ("\\paragraph{%s}" . "\\paragraph*{%s}")
          ("\\subparagraph{%s}" . "\\subparagraph*{%s}")))


Make this the default Latex export.

.. code:: lisp

   (setq org-latex-default-class "sjj-org-article")

Here is a `sample output file </butterchicken.pdf>`_  (PDF) from this setup. Not
too shabby. 

You can check out my whole Emacs configuration at `github
<https://github.com/samjuvonen/dot-emacs/>`_. Comments welcome!

*Credits:* `This emacs-fu post
<http://emacs-fu.blogspot.com/2011/04/nice-looking-pdfs-with-org-mode-and.html>`_
was for an earlier version of Org.

