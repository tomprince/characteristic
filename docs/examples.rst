.. _examples:

Examples
========


``@attributes([attr1, attr2, …])`` enhances your class by:

- a nice ``__repr__``,
- comparison methods that compare instances as if they were tuples of their attributes,
- and – optionally but by default – an initializer that uses the keyword arguments to initialize the specified attributes before running the class’ own initializer (you just write the validator!).


.. doctest::

   >>> from characteristic import Attribute, attributes
   >>> @attributes(["a", "b"])
   ... class C(object):
   ...     pass
   >>> obj1 = C(a=1, b="abc")
   >>> obj1
   <C(a=1, b='abc')>
   >>> obj2 = C(a=2, b="abc")
   >>> obj1 == obj2
   False
   >>> obj1 < obj2
   True
   >>> obj3 = C(a=1, b="bca")
   >>> obj3 > obj1
   True
   >>> @attributes(["a", "b", Attribute("c", default_factory=list)])
   ... class CWithDefaults(object):
   ...     pass
   >>> obj4 = CWithDefaults(a=1, b=2)
   >>> obj5 = CWithDefaults(a=1, b=2, c=[])
   >>> obj4 == obj5
   True
