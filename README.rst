#################################
Weaver: Code organised by concern
#################################

Code is hard to navigate, which makes it hard to maintain, especially if you
were not the original author, or haven't looked at it for 6 months 

UML is a handy notation, but it lacks linkage between diagrams (ie models), 
their elements and the actual code (unless you have a complex round-trip tool).
Semantic web / linked data concepts offer an interesting opportunity: we might
develop an ontology of how elements of models relate to each other and to the
eventual code. Each unit of code could be traced back through the model it 
concretizes to the requirements that drive it 

The ideas in [Kartik-layers]_  about building code in layers, where each layer
with those below it make a buildable, runnable program. Layers implement a 
single concern, and use directives to indicate where in the underlying layers 
each framgment belongs. These are all woven togather in a similar manner as 
aspect-oriented programming. In a related post the same author argues that 
`literate programming`_ ought to emphasize program organization rather than 
typesetting

This project is an attempt to support model-based design via semantic linking
between code and models, enabled by a 'weaver' tool that extracts code snippets
from documentation (using reStructuredText_, for its semantic capabilites)

The main components of this tool will be a Weaver, which is itself written in
layers woven together by a SimpleWeaver (a bootstrap-type arrangement); and an
ontology describing how models elements in terms of each other

.. [Kartik-layers] http://akkartik.name/post/wart-layers
.. _literate programming: http://akkartik.name/post/literate-programming
.. _reStructuredText: http://docutils.sourceforge.net/docs/user/rst/quickref.html


Directory layout
****************

Rather than a ``src/`` subdirectory Weaver uses ``design/`` for the 
human-written design documents and ``code/`` for code woven from those design
documents::

  README.rst  This file
  design/     Design documents, correpsonds to src/ in more traditional setup
  code/       Where generated code is put
  lib/        Just SimpleWeaver.py, for bootstrapping (imported by Weaver.py) 
  etc/        Holds ontology for design->code validation
  bin/        Utilities such as shell script calling SimpleWeaver.py


Design process
**************

This chapter is meant to inform the bootstrap, and (currently) draws heavily on
notes from http://agilemodeling.com/. The bootstrap follows an 
acceptance-test-driven-design process:

.. code-block:: plantuml
  @startuml
  partition envisioning {
    (*) --> "initial requirements vision" as reqs
    reqs --> "initial architecture vision" as arch
    reqs -right-> "acceptance tests for MVP"
  }
  partition development {
    reqs --> "iteration planning"
    arch --> "iteration planning"
    "iteration planning" --> "model storming"
    "model storming" --> TDD
  }
  @enduml

  




The basic (activity) flow is:

.. code-block:: plantuml

  @startuml
  partition envisioning {
    (*) --> "initial requirements vision
              - usage model
              - initial domain model
              - user interface model" as ir
    ir --> "initial architecture vision" as ia
    ia --> ir
    ir -right-> "acceptance tests for MVP"
    partition nested {
    }
  }
  partition development {
    ia --> "iteration planning: 
            what will be implemented 
            and roughly how" as ip 
    note right 
      each development iteration should 
      implement a small set of related 
      requirements and result in 
      something that works 
  end note
  
    ip --> "model storming:
      analysis and design models
      for this iteration " as ms
    ms --> "acceptance test driven development"
  } 
  @enduml

.. end

and a pass of acceptance-test-driven-development looks like:

.. code-block:: plantuml
  @startuml
  partition "acceptance TDD" {
    (*) --> "write acceptance test" as wat      
    wat -down-> "run acceptance tests" as rat 
    rat -up->[pass] wat
    partition "TDD" {
      rat -right->[fail] "write unit test"
      "write unit test" --> "run unit test" as rut
      rut --> [fail] "edit code" as ec
      ec --> rut
      rut -left-> [pass] rat
    }
  }
  @enduml
.. end

The idea is that 



  requirements -> models -> snippets -> artifacts -> products
    -> architecture should follow requiremnts (?)
    -> constraints are global requirements (not necessarily features)

Starting with initial requirements, leading to high-level architecture. Models
for initial requirements are high-level, aim is to work out broadly what to 
build

- Domain Model :
    either an object/class diagram or IDAR graph showing the entities in the 
    (business) domain and how they interact (class-responsibility-collaborators
    -like approach)

- Usage Model - either:

   essential use cases 
      Has a name, pre- and post-conditions, then a 2-column list of "user 
      intention" - "system responsibility". Fairly high-level.

   system use cases
      Has a name, then a list of basic steps

   user stories
     More rigorous use-case, with context, goal, requirement, sunny-day flow,
     alternate flows, dependencies (on other use cases/user stories). This 
     might be too detailed for initial requirements..
     User stories inform "acceptance tests"

   feature list
      A list of simple features the system must support

- UI Model :
    UI Flow diagram and/or screen sketches. Aim is to explore how the user will
    interact with the application

Iterate between the Usage and UI model to create a prioritized list of 
requirements

