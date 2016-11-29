############
Introduction
############

Why Do We Need a Model Exchange Format?
=======================================

Over the years, research efforts have been made in the direction of
“next generation” PSA software and “declarative modeling”, which try to
present a more informative view of the actual systems, components, and
interactions which the model represents. The concern of these studies
has been to end the use of approximations: numerical approximations for
which we do not know the error factors, and modeling approximations
which leave out perhaps critical elements of the actual system under
study. From all these investigations, some issues related to large
nuclear PSA models have been raised, which need to be addressed before
to put new calculation engines or next generation user interfaces into
place. To address these issues enumerated below, an “Model Exchange
Format”, a representation which is independent of all PSA software, must
be in place. In this perspective software would retain their own
internal representation for a model; but each software would also be
able to share models and industry data by means of the Model Exchange
Format.

*Quality assurance of calculations:* at the moment, a model built with
one software, cannot be simply quantified with another software, and
visa versa; there are too many software dependent features used by
modelers to make inter-calculation comparisons a one-step process. The
Model Exchange Format will allow models to be quantified by several
calculation engines; therefore quality assuring results in a strong way.

*Over reliance on numerical approximations and truncation:* while this
cannot be solved directly by the Model Exchange Format, as new
calculation engines are completed, the Model Exchange Format will allow
new engines to be snapped into new (or existing) user interfaces without
changing the model or user interface software.

*Portability of the models between different software:* at the moment,
models are essentially non-portable between calculation engines, as
pointed out above. The Model Exchange Format allow complete, whole
models to be shared right now between software; the bonus will be on
each software to correctly interpret the model representation.

*Clarity of the models:* For one who examined a number of large nuclear
PRA models, it is obvious that just looking at the basic events, gates
and fault trees/event trees is of little help in understanding the
“where”, “why”, and “how” of model elements: common cause failures,
initiating events, sequence information, alignment information, systems
and trains, flags, logic of recovery rules, or the dreaded “delete
terms”. The Model Exchange Format employs what is becoming known as
structured modeling. Structured Modeling takes its name from the
structured programming movement in the 1970s. Before that time,
variables, arrays, and other data structures, were used with no
definitions and explanations. Structured programming techniques forced
programmers to “declare variables” at the beginning of a program by name
and also by the type of variable it was: an integer, a real number, and
so on. In this way the meaning of the program became clearer, and
calculation speeds were increased. Structured Modeling, as applied to
PRA models and software, has the same goal of making the meaning of the
model more clear, more transparent, and to improve the speed and
accuracy of the calculation. The user interface to create such a model
is not of concern here. The concern is to discover the distinct model
elements which are needed to quantify and clarify large PRA models.

*Completeness of the models:* without clarity, there can be no knowledge
of the completeness of the model, since their very size and complexity
strains the brain. The Model Exchange Format will create more
survey-able models.

*Difficulty of different software working with the same PSA model:* as
more risk applications are being requested (seismic, fire, balance of
plant assessments, risk monitors, release calculations), difficulties
are arising because each risk application and major PSA software have
different internal data formats. The Model Exchange Format will be able
easily to share model data between applications and specialized software
would be available for all models.

*Lack of data and software backward and forward compatibility:* again,
as more diverse software need to interact, such as safety monitors,
calculation engines, and fault tree editors, the need to have data and
programs separate becomes of high importance. The Model Exchange Format
solves this problem by allowing programs to change without the need for
the data format to change and for other programs to change their
operations.

*No universal format for industry data:* The Model Exchange Format will
be a perfect way to publish industry data such as common cause, failure
rates, incidents, and initiating event frequencies.

Requirements for the Model Exchange Format
==========================================

To be acceptable and widely accepted, the Model Exchange Format for PSA
must fulfill a number of requirements. The following list is an attempt
to summarize these requirements.

*Soundness:* the Model Exchange Format must be unambiguous. The
semantics of each construct must be clearly given, in such way that no
two correct implementations of the Model Exchange Format can differ in
their interpretation of models (they may differ however, at least to a
certain extent, in the results they provide if they use different
calculation methods).

*Completeness:* the Model Exchange Format should cover as much as
possible; not only all aspects of PSA models, but also references to
external documentations and format of the results. These issues have to
be covered by the Model Exchange Format in order to make models actually
portable and to be able to cross check calculations.

*Clarity:* the Model Exchange Format should be self-documenting to a
large extent. The constructs of the Model Exchange Format should reflect
what the designer of the model has in mind. Low level constructs would
help in making the format universal (any model can be eventually
represented by means of a Fortran or C program, not to speak of a Turing
machine or a Church lambda term). But constructs which are at too low a
level would be of little help, and even counter-productive, for model
review.

*Generality:* it should be possible to cast all of the existing models
into the Model Exchange Format without rewriting them from scratch. The
translation of existing models should be automated, at least to a large
extent. Moreover, any existing tool should be able to use the Model
Exchange Format as its representation language. Indeed, most of the
tools implement only a subpart of the Model Exchange Format but the
Model Exchange Format should be a superset of the underlying formalisms
of all existing tools.

*Extensibility:* the Model Exchange Format should not restrict
developers if they wish to introduce interesting new features in their
tools. This means that it should be easy to introduce new constructs
into the Model Exchange Format, even if these constructs are not
recognized by all of the tools. On the other hand, these new constructs
should be clearly identified; their semantics should be clear and public
in such way that any other developer can embed the feature in his own
tool.


Choice of XML
=============

To create the Model Exchange Format, we must make formal definitions for
representing existing PRA models and define a syntax to write them. The
Model Exchange Format is defined as a XML document type. XML is widely
used on the internet as a common way for programs to share data. It is
well structured and makes it possible to give explicit name to each
construct. XML is therefore well suited for structured modeling. By
giving the elements of a model a formal designation (“this is an
initiating event”, “this is a basic event”, and so on), quantification
results and understanding of the model can be improved.

XML presents another major advantage for tool developers: many
development teams have more or less already designed its own XML parser
and many such parsers are anyway freely available on internet. Therefore
the choice of a XML based syntax discharges programmers of PSA tools of
the tedious task to design parsers and to perform syntactic checks.
Moreover, due to their tree-like structure, it is easy to ignore parts
of a XML description that are not relevant for a particular purpose.
Therefore tools which do not implement the whole Model Exchange Format
can easily pick up what they are able to deal with.

A Four-Plus-One Layers Architecture
===================================

The Model Exchange Format relies on a four-plus-one layers architecture,
as pictured `Figure Introduction-1 <#anchor-6>`__. Each layer
corresponds to a specific class of objects/mathematical constructs.

Figure ‑. Architecture of the Model Exchange Format

-  The first, or stochastic, layer is populated with all stochastic
   aspects of models: probability distributions for the failure rates of
   basic events, parameters of these distributions and distributions of
   these parameters, flags...
-  The second, or fault tree layer, is populated with logical components
   of fault trees (gates, basic events, house events). This layer is the
   core of PSA models. The two first layers can be used in isolation.
   Some existing tools implement them only.
-  The third, or meta-logical, layer is populated constructs like common
   cause groups, delete terms, recovery rules that are used to give
   flavors to fault trees...
-  The fourth, or event tree, layer is populated with event trees,
   initiating events and consequences. The Model Exchange Format sees
   event trees as (graphical) programs. The execution of such a program
   produces a set of sequences, i.e. a set (a disjunction) of Boolean
   formulae. Probability distributions are also collected while walking
   the event tree.
-  The fifth, or report layer, is populated with constructs to store
   results of calculations. This includes constructs to describe
   calculations (version of the model, used engine, used cutoffs,
   targeted group of consequences, calculated quantities...) and well as
   minimal cutsets and other results.

This five layers architecture helps to understand what the different
elements of a model are and what their respective roles are. In a word,
it is the backbone of the Model Exchange Format. It should be clear
however that any model will contain elements of the first fourth levels
and that these elements may be not arranged by levels. For instance, a
fault tree description will probably contain probability distributions
of basic events as well as common cause groups. Again, the five layers
architecture intends to differentiate elements according to their
meanings and operational behaviors.

Formalism
=========

Throughout this document, we shall present a number of syntactic
constructions such as Boolean formulae, probability distributions, and
so on. These constructions will be eventually represented by means of
XML terms. XML is however a bit too verbose to make clear the underlying
mathematical nature of objects at hand. Therefore we shall use (in a
rather loose way) the Extended Backus-Naur form to define constructs. A
presentation of the Extended Backus-Naur form can be found in `Appendix
A. <#anchor-9>`__.

There are several formal ways to describe a XML grammar. The most
popular one is probably the XML Document Type Definition (DTD). A DTD is
associated with an XML document via a Document Type Declaration, which
is a tag that appears near the start of the XML document. The
declaration establishes that the document is an instance of the type
defined by the referenced DTD. DTD are a good verification tools, but
hard to interpret by a human. Therefore, we shall present the grammar of
the Model Exchange Format mainly by means of examples and semi-formal
descriptions with the Extended Backus Naur form. A formal DTD for the
whole Model Exchange Format is given `Appendix B. <#anchor-10>`__. A
semi-formal Backus-Naur form for the Model Exchange Format is given
`Appendix C. <#anchor-11>`__.

It is worth noting that the XML descriptions we are giving here can be
extended in any way to fulfill the needs of a particular tool. In
particular, comments and pointers to documentation should be added here
and there to the model.

Organization of the document
============================

The remainder of this document is organized into six chapters
corresponding to an introductive overview, one chapter per layer of the
architecture of the Model Exchange Format and one additional chapter for
models as a whole.

-  Chapter `III <#anchor-13>`__ gives an overview of main elements of a
   model and shows how these elements are organized. It discusses how to
   split a description into several files, how to solve naming
   conflicts...
-  Chapter `IV <#anchor-14>`__ presents the fault tree layer. The fault
   tree layer is not the lowest one in the hierarchy. However, fault
   trees are the most basic and the central concept of PSA models. For
   this reason, we put it in front.
-  Chapter `V <#anchor-15>`__ present the stochastic layer, i.e. all the
   mechanisms to associate probability distributions to basic events.
-  Chapter `VI <#anchor-16>`__ presents the meta-logical layer.
-  Chapter `VII <#anchor-17>`__ presents the event tree layer.
-  Chapter `VIII <#anchor-18>`__ discusses the organization of models.
-  Finally, chapter presents the report/results layer, i.e. the
   normalized format for results of assessment of PSA models.

Three appendices give additional details or summarize the contents of
these six chapters.

-  `Appendix A. <#anchor-9>`__ presents the Backus-Naur form we use
   throughout this document to describe both the mathematical structure
   of the constructs and their XML representation.
-  `Appendix B. <#anchor-19>`__ gives the Document Type Definition (DTD)
   of the full Model Exchange Format.
-  `Appendix C. <#anchor-11>`__ gives the Backus-Naur form of the Model
   Exchange Format.