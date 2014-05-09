---
layout: default
title: express
---

~~~
The (building) industry in Europe currently uses the 
#******************************************************
~~~
###express.vdmsl

{% raw %}
~~~

module Database
exports all
definitions
types
PhysicalFile ::
HeaderEntity ::
Scope :: ;
Record = SimpleRecord | SuperRecord ;
SuperRecord ::
SimpleRecord ::
Parameter = StringParameter |
StringParameter ::
RealParameter ::
IntegerParameter ::
EntityInstanceName ::
EnumerationParameter ::
BinaryParameter ::
ListParameter ::
TypedParameter::
OmittedParameter:: ;
UnknownParameter::
operations
CheckReferences: Parameter ==> set of nat
FindAllReferencesToEntity: nat ==> set of nat
FindAllInstances: seq of char ==> set of nat
LookUpEntityInstance: nat ==> [Record]
TransformRmVertex: nat ==> nat
TransformRmEdge: nat ==> set of (nat * nat)
       TransformRmLoop: nat ==> seq of nat
       Transform: () ==> set of seq of nat
      Create: set of seq of nat ==> ()
       DoMapping: PhysicalFile ==> PhysicalFile
    functions
      LenPar1: seq of nat * map nat to nat -> nat
      Collect : set of seq of nat -> set of nat
      SetCard: set of seq of nat -> nat
      IsA: Record * seq of char -> bool
      SortInnerLeft: set of (nat * nat) * nat -> seq of nat
      SortInnerRight: set of (nat * nat) * nat -> seq of nat
      SortPoints : set of (nat * nat) -> seq of nat
    state Kernel of
end Database

~~~{% endraw %}
