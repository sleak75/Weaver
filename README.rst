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

The ideas in `this blog post`_ about building code in layers, where each layer
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

.. _this blog post: http://akkartik.name/post/wart-layers
.. _literate programming: http://akkartik.name/post/literate-programming
.. _reStructuredText: http://docutils.sourceforge.net/docs/user/rst/quickref.html

