Models.py
=========

Caveats: Models
---------------

There are several common problems when writing the models. This problems may
be related to code maintainability, reusability or due to logic. Please,
take in account the following list when writting your own models:

*   **Take care of circular dependencies**: This is because some applications
may depend directly from other applications when using foreign keys to set
relationships between the models. You must define a principal application,
which will not depend from any other and can be called by every other
application, (commonly this one will be named "MainAPP"), This will be a
great starting point for defining the models and applications. In case the
AppOne needs to talk to AppTwo and viceversa, consider using an intermediate
model and prevent AppOne from calling AppTwo.

*   **Go for maximum granularity without wasting queries**: When building a
MER to design a database, a rule of thumb is to use maximum granularity to
define some extra models, and although this is not wrong, we need to consider
all the extra queries that will be used. This could mean to combine certain
models, or to define some model's functions to call this relationships while
using the [`prefetch_related`][pre_rel] or [`select_related`][sel_rel]
functions (to minimize the number of sql calls).

*   **Be careful with the migrations**: This is because all the migration
files will be uploaded to the repositories. The reason behind this is for
using the same files on the server for some kind of deployments (including
docker and AWS). If you are trying to test a new pip package which generates
migrations, test them before making a new commit. 

Models fields
-------------

TODO:

*   Lowercase, single word name or separated by an underscore.
*   Always add `verbose_name`
*   Do not use `unique`

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

Also remember, when you overwrite this function, some signals may not work
properly as they get executed when a `super()` is detected, but the sql
transaction will not finish until the `save` function has ended. (This is
just a theory)


[pre_rel]: https://docs.djangoproject.com/en/1.10/ref/models/querysets/#prefetch-related
[sel_rel]: https://docs.djangoproject.com/en/1.10/ref/models/querysets/#prefetch-related
