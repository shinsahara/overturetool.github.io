---
layout: default
title: Planner
---

~~~
The specification is of the input language, and the central operations, 
Planning for Conjunctive Goals, D.Chapman, AI Journal no 32, 1987. 
The Construction of Formal Specifications: an Introduction to the 
~~~
###planner.vdmsl

{% raw %}
~~~

types
	Literal = seq of token; 
	Planning_Problem :: AS : set of Action
	inv  mk_Planning_Problem ( AS, I, G) ==
Action_id = token;
Arc ::
Bounded_Poset = set of Arc
 Goal_instance :: 
Goal_instances = set of Goal_instance
state Partial_Plan of
functions
get_nodes : set of Arc -> set of Action_id
before : Action_id * Action_id * set of Arc -> bool 
possibly_before : Action_id * Action_id * set of Arc -> bool 
completion_of : Bounded_Poset * Bounded_Poset -> bool 
initposet: () -> Bounded_Poset
add_node : Action_id * Bounded_Poset -> Bounded_Poset
make_before : Action_id * Action_id * Bounded_Poset -> Bounded_Poset

newid(isa : set of Action_id) i: Action_id 
achieve : Action_instances * Bounded_Poset * Action_id * Goal_instance -> bool 
declobber:Action_instances * Bounded_Poset * Action_id * Goal_instance -> bool 
operations
INIT (ppi : Planning_Problem)

ACHIEVE_1(gi : Goal_instance)

ACHIEVE_2(gi: Goal_instance)


~~~{% endraw %}
