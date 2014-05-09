---
layout: default
title: ACS
---

~~~

This specification describes the safety requirements involved in adding and 

#******************************************************
~~~
###acs.vdmsl

{% raw %}
~~~

types
Cg = <A>|<B>|<C>|<D>|<E>|<F>|<G>|<H>|<J>|<K>|<L>|<S>;
Inf = <INFINITY>;
Kg = real|Inf
Realp = real
Metre = Realp;
Object::   neq: Kg
Element_label = token;
Element::  object: Object
Point::   x: Realp
Pes_types = <EARTHCOVEREDBUILDING>|<HEAVYWALLEDBUILDING>|
Magazine::   type: Pes_types

Storage_building :: kind: <IGLOOSEVENBAR>|<IGLOOTHREEBAR>|
Process_building :: kind: <WITHPROTECTIVEROOFTRAVERSED>|
Other_building :: kind: <INHABITEDBUILDING>|<TRAFFICROUTE>;
Exs_types = Storage_building | Process_building | Other_building;
Building::  type: Exs_types
Quad = seq of Point
Site_label = token;
Exposed_site:: building: Building
Pot_explosion_site:: mgzn: Magazine
Line:: m: real
RelOrientation = <PERP> | <FACING> | <AWAY>;
OrientedExs = Exs_types * (RelOrientation | <NONE>)
OrientedPes = Pes_types * (RelOrientation | <NONE>)
Table_Co_ordinate = OrientedExs * OrientedPes


values
asharp: map Hzd to (map Table_Co_ordinate to real)
bsharp: map Hzd to (map Table_Co_ordinate to real)
exceptions_hd1_1 : set of Table_Co_ordinate
exceptions_hd1_2 : set of Table_Co_ordinate
exceptions_hd1_3a : set of Table_Co_ordinate
exceptions_hd1_3b : set of Table_Co_ordinate
Xmax = 5; -- is not yet defined;
next_point: map nat to nat
Compatible_pairs: set of (Cg * Cg) = 
hzdnum: map Hzd to nat
orientation: map nat to RelOrientation

esharp: nat = let x : nat in x -- is not yet defined
state Store of

functions
rectangular: seq of Point -> bool
distance: Point * Point -> Metre
sqrt (x: real) s:Realp
suff_space_at:Object * Magazine * Point -> bool
find_point(o:Object, m:Magazine) pt:Point
within_hazard: Object * Magazine -> bool
compatible: Cg * Cg -> bool
all_compatible: Object * Magazine -> bool
sum: set of real -> real
Card: set of real -> nat
suff_capacity: Object * Magazine -> bool
safe_addition: Object * Magazine * Point -> bool
rel_pos:Pot_explosion_site * Exposed_site -> nat
table_entry: Pot_explosion_site * Exposed_site ->  Table_Co_ordinate
min(s: set of Realp) m: Realp
max(s: set of Realp) m:Realp
truncated: Realp -> bool
side: Point * Point -> set of Point
perimeter: (Pot_explosion_site|Exposed_site) -> (set of Point)
shortest_dist:Pot_explosion_site * Exposed_site -> Metre
min_separation:Pot_explosion_site * Exposed_site -> bool
qd: Pot_explosion_site * Exposed_site -> Kg
nearest_storage_building(pes:Pot_explosion_site,
nearest_inhabited_building(pes:Pot_explosion_site,
nearest_traffic_route(pes:Pot_explosion_site,
nearest_process_building(pes:Pot_explosion_site,
nearest_buildings(pes:Pot_explosion_site, exs: set of Exposed_site) 
find_max_neq:Pot_explosion_site * set of Exposed_site -> Kg
centre(v: Quad) p: Point
line_eqn: Point * Point * Point -> Line
incline:Point * Point * Point * Point * Point * Point -> real

ang_sep(pes:Pot_explosion_site, exs:Exposed_site) qsharp:real
arctan: real -> real
operations
ADD_OBJECT(o:Object, elt: Element_label, site: Site_label)

REMOVE_OBJECT(elt: Element_label, site: Site_label)

ADD_PES(pex:Pot_explosion_site, label: Site_label,type:Storage_building)




ADD_EXP(ex:Exposed_site, label: Site_label)

~~~{% endraw %}
