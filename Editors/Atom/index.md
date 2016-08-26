ATOM Editor
===========

Packages used
-------------

Inside the [packages-list][packages-list] file, you can find the preferred
Atom packages which are used for all of our projects. In case you wish to
understand better how they work or what they do, please visit the links below:

*   Bla

Caveats
-------

Do not try to open single-line files, like `.min` files. The reason behind
this is that Atom has some memory-leak problems when opening files which
lines surpasses a 1024 characters limit, this memory-leak will end up
freezing the editor, and possibly your OS.

[packages-list]: https://github.com/letops/FacturaBot/blob/master/Editors/Atom/packages-list.txt
