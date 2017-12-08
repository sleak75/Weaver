#################################
Bootstrap requirements and design
#################################

The bootstrap is a simple python script to extract python codeblocks from an
rst file and write them into a python file

***********
Usage Model
***********

Running the bootstrap on the `SimpleWeaver.rst` file should scan it for blocks
delimited by `.. code-block:: python` and `.. end`, and write the contents of 
those blocks into a `.py` file

In the spirit if TDD the `SimpleWeaver.rst` file will start from the test 
scaffold and work backwards to the actual code, so the bootstrap should write
the blocks it finds in reverse order (ie, the main internal data structure is
a stack of code blocks)

The bootstrap doesn't need to cope with anchors and layers, the SimpleWeaver
should introduce support for those

**********************
Bootstrap architecture
**********************

Trivial, an input file, an output file and a stack. 

.. code-block:: plantuml
  @startuml
  class Weaver {
  }
  @enduml
.. end

****************
Acceptance tests
****************

1. When given this file, the bootstrap should produce the same code as itself

2. When given the `SimpleWeaver.rst` file, the bootstrap should produce a 
   simple, single-file weaver that can run its own acceptance tests

****
Code
****

.. code-block:: python
  

  import unittest
  class TestBootstrap(unittest.TestCase):

    def 

  if __name__ == "__main__":
    unittest.

import unittest
class TestExpandNodelist(unittest.TestCase):

    def test_expandnodelist(self):
        # slightly arbitrary list of test cases:
        nlist = 'nid02085'
        self.assertEqual(expand_nodelist(nlist), 'nid02085')

.. end


