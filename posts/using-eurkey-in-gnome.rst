.. title: Using EurKEY in Gnome
.. slug: using-eurkey-in-gnome
.. date: 2016-08-21 16:07:23 UTC-07:00
.. tags: linux, gnome, desktop, keyboard
.. category: 
.. link: 
.. description: 
.. type: text

`Update:` Well, it seems that Gnome 3.20 on Fedora 24 actually has EurKEY
selectable from the GUI. Nice! The rest of the post is still accurate, if
a bit superfluous now.

`EurKEY <http://eurkey.steffen.bruentjen.eu/start.html>`_ is the best general
layout for basic US keyboards. It gives me easy access to keys required by
Finnish, Swedish, German or any other European languages. These days with
Unicode in all the things, it's pretty lame to write these languages in 7-bit
ASCII like it was still the 1970's. Unfortunately it is impossible to configure
in Gnome's Regional Settings.

Fortunately EurKEY is part of `xkb` and Gnome keyboard configuration is a
wrapper around it, it can be set up with `dconf` or dconf-editor.

To read the current settings:

.. code:: sh
   
  $ dconf read /org/gnome/desktop/input-sources/sources
  [('xkb', 'us')]

You can also browse to the above path in dconf-editor.

To set the key, write to it. The example show setting a secondary layout as well that
you can switch to in Gnome.

.. code:: sh
   
  $ dconf write /org/gnome/desktop/input-sources/sources "[('xkb', 'eu'), ('xkb', 'us+alt-intl')]" 

.. code:: sh

   $ dconf read /org/gnome/desktop/input-sources/sources
   [('xkb', 'eu'), ('xkb', 'us+alt-intl')]

Now ä is on RightAlt-a, ö is on RightAlt-o, and things like ß, ç, ü are pretty
much where you'd expect them to be. You also have various currency signs (£¥€¢),
symbols etc, right under your fingers.
          
Thanks to Steffen Brüntjen for EurKEY! Check `his site
<http://eurkey.steffen.bruentjen.eu/start.html>`_ for details.
