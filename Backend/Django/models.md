Models.py
=========

Models fields
-------------

TODO: lowercase, single word name or separated by an underscore

Models functions
----------------

Almost every base models will have some, heavy or complicated, functions
assigned to them, the reason behind this is pretty straight forward: We
don't want to have too large or too specific functions in our `views.py`
file, nor heavy-logic processes in our `queries.py` file (in case this
file was created) or in our `lambda`s.

This can be better understood if you use the models not only as "models",
but rather use them as actual objects which can execute actions by their own,
just like in other programming languages.

Inside this functions, there is just one single restriction: You **CANNOT**
validate for permissions to execute the action, all the permissions will be
checked inside the `views.py` file. Besides that you can create as much class
methods or instance methods as you see fit.

Caveats: Django's Function Overrides
------------------------------------

The function overrides are something that has quite some restrictions, not
just for our standards, but for the reason that some dependent functions
will not work correctly if an override is done:

### save

There is one serious problem with overriding this function: The bulk_insert
function is prone to breaking when there exists more than two `super()`
calls inside the function, it doesn't matter if just one is called, there
must not exist more than one `super()` in the whole function. A workaround for
this, would be using the signals `pre_save` or `post_save` to prevent the
function overriding.
