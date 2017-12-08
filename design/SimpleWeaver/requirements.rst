#########################
SimpleWeaver Requirements
#########################

SimpleWeaver is the bootstrap core of the system, it should be a single python
file and have base classes for each of the core components. It need only 
support one mark-up language (rst) and one programming programming language
(python) as a later layer will add broader support

***********
Usage model
***********

Usage is via command line.

The "bootstrap" core of the tool just does one thing:

.. code-block:: plantuml

  @startuml
  usecase UC1 as "weave python source files from code snippets in
                  a provided list of rst documents"
  Developer -> (UC1)
  @enduml

.. end 

The user stories around this are:

**US1**. As a developer, I want to deal with one concern at a time so that I can 
incrementally add functionality without messing up what I already have

  Implication here is that the design is grouped (into documents) by concern
  instead of class or component, and that as concerns are added the code should
  build and run, even though it is not complete (see [Kartik-layers]_) 







As essential use cases: (additional detail):

Weave code from snippets
    :ID: _`UC1`
    :Preconditions:  User provides a manifest identifying design files
    :Postconditions: A set of code files is produced
    :Notes:          The user intentions are described in the manifest and 
                     design files

      ======================  =========================
      **User intent/action**  **System Responsibility**
      ----------------------  -------------------------
      User provides manifest  - scan the files for code snippets
      
      Snippet has filepath
      
      Snippet has anchor
      ======================  =========================



A more complete Weaver will connect the design, implementation and testing in
a traceable way. Some user stories to illustrate:


**US2**. As a developer, I want my (produced) code to be logically arranged in 
files and folders by component, so I can deliver code in a familier format and
separate out independently-useful components

  This will require Weaver to know what file to add each code snippet to. Most
  snippets will be marked as intended for some anchor, so snippets that are the
  top-level skeleton for a Package (or sub-package) could be similarly marked 
  with the file path 

**US3**. As a developer, I want to write tests alongside the concern or design 
element thay are meant to test, so I can easily keep code and tests in sync and
easily verify that all code has an associated test

**US4**. As a maintainer, I want to easily find the design notes for the piece 
of code I am looking at, and trace it back to the requirements motivating it,
so that when I am debugging or modifying it, I understand the intent behind the
code I see

  Keeping the anchors in the code (comments), and using start/end delimiters 
  (to unravel nested inserts), should help here


############
Domain model
############

needs some fleshing out:

- "data":
  - design docs - have design notes (eg diagrams in a markup language, notes,
    table, etc) code snippets surrounded by design notes
  - manifest - some description (list?) of input files to be scanned
  - artifacts - the produced code

- the weaver itself:
  - reader - an object/class to scan design docs for code snippets. Want to
             (eventually) support variety of formats (just rst to start with),
             so maybe a base class + language-specific specialization class.
             Main feature is to extract code snippets and pass them to other
             part of tool, but if adding eg model verification things, might
             want to extract other things too
  - weaver - scans snippets for directives indicating anchor points and 
             targets. Also needs to be language aware (or at least language-
             family-aware, to identify comments with directives)
  - writer - assembles all the extracted snippets into code files. 

- policies: some description of eg directve format for different languages,
            filename extensions by language

- datastore: snippets need to be collated and then selected and combined, some
             sort of database, Each snippet has a target, and can have any 
             number of anchor points (ie, more fields than just a key-value
             store, maybe an sqlite3 db)
            
Ideally I want the code to refer back to the model - is the implicit reference 
by the fact that code for a given concern is written in the same design file 
enough? 

Eventually maybe understanding models to some degree - not sure how yet

Taking an IDAR-graph approach, I think we have something like:
(mix of control and dataflow here...)

.. code-block:: plantuml

  @startuml
  object driver
  note left "role of driver?"

  object theReader
  note left "extracts code snippets and design artifacts from design docs"

  object theWriter
  note "merges snippets into snippets at anchors and writes to code files"

  object theWeaver
  note "finds anchor points in snippets, verifies that snippets have targets"

  database datastore
  note "hold extracted snippets and anchors"

  driver --> theReader : manifest >
  driver --> theWriter
  theReader ..> theWeaver : snippets >
  theWriter --> theWeaver : snippets <

  theWeaver ..> datastore : snippets and anchors >
  
  @enduml

.. end

For more detailed design step, will need to work out what the datastore should look like 


