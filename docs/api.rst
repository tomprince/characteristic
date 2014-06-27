.. _api:

API
===

``characteristic`` consists of class decorators that add features to your classes.
There are three that start with ``@with_`` that add *one* feature to your class based on a list of attributes.
Then there's the helper ``@attributes`` that combines them all into one decorator so you don't have to repeat the attribute list multiple times.

.. currentmodule:: characteristic


.. autofunction:: with_repr

   .. doctest::

      >>> from characteristic import with_repr
      >>> @with_repr(["a", "b"])
      ... class RClass(object):
      ...     def __init__(self, a, b):
      ...         self.a = a
      ...         self.b = b
      >>> c = RClass(42, "abc")
      >>> print c
      <RClass(a=42, b='abc')>


.. autofunction:: with_cmp

   .. doctest::

      >>> from characteristic import with_cmp
      >>> @with_cmp(["a", "b"])
      ... class CClass(object):
      ...     def __init__(self, a, b):
      ...         self.a = a
      ...         self.b = b
      >>> o1 = CClass(1, "abc")
      >>> o2 = CClass(1, "abc")
      >>> o1 == o2  # o1.a == o2.a and o1.b == o2.b
      True
      >>> o1.c = 23
      >>> o2.c = 42
      >>> o1 == o2  # attributes that are not passed to with_cmp are ignored
      True
      >>> o3 = CClass(2, "abc")
      >>> o1 < o3  # because 1 < 2
      True
      >>> o4 = CClass(1, "bca")
      >>> o1 < o4  # o1.a == o4.a, but o1.b < o4.b
      True


.. autofunction:: with_init

   .. doctest::

      >>> from characteristic import with_init
      >>> @with_init(["a", "b"], defaults={"b": 2})
      ... class IClass(object):
      ...     def __init__(self):
      ...         if self.b != 2:
      ...             raise ValueError("'b' must be 2!")
      >>> o1 = IClass(a=1, b=2)
      >>> o2 = IClass(a=1)
      >>> o1.a == o2.a
      True
      >>> o1.b == o2.b
      True
      >>> IClass()
      Traceback (most recent call last):
        ...
      ValueError: Missing keyword value for 'a'.
      >>> IClass(a=1, b=3)  # the custom __init__ is called after the attributes are initialized
      Traceback (most recent call last):
        ...
      ValueError: 'b' must be 2!

   .. note::

    The generated initializer explicitly does *not* support positional
    arguments.  Those are *always* passed to the existing ``__init__``
    unaltered.


.. autofunction:: attributes


.. autoclass:: Attribute
