---
layout: default
title: realm
---

~~~
This document is simply an attempt to model the basic data 
Realms: A Foundation for Spatial Data Types in Database 
Map Generalisation, Ngo Quoc Tao, UNU/IIST, Macau, 
~~~
###realm.vdmsl

{% raw %}
~~~

module REALM
imports from TEST all
exports all
definitions
values
  max: nat = 10
types
  N = nat
  NPoint ::
  NSeg :: 
functions
  SelPoints: NSeg +> NPoint * NPoint
functions
  Points: NSeg +> set of NPoint
types
  Rat = int * int
functions
  Slope: NPoint * NPoint +> Rat
  RatEq: Rat * Rat +> bool
  DiffX: NPoint * NPoint +> set of N
  DiffY: NPoint * NPoint +> set of N
  On: NPoint * NSeg +> bool
  In: NPoint * NSeg +> bool
  Meet: NSeg * NSeg +> bool
  Parallel: NSeg * NSeg +> bool
  Overlap: NSeg * NSeg +> bool
  Aligned: NSeg * NSeg +> bool
  Intersect: NSeg * NSeg +> bool
  Coliner: NSeg * NSeg +> bool
  Disjoint: NSeg * NSeg +> bool
  Intersection: NSeg * NSeg -> NPoint
  RoundToN: nat * nat +> nat 
types
  Realm ::
functions
  InsertNPoint: Realm * NPoint +> Realm
  InsertNSegment: Realm * NSeg +> Realm
  ChopNPoints: set of NPoint * set of NSeg +> set of NSeg

  ChopNSegs: set of NSeg * set of NSeg * set of NSeg * set of NPoint +> 
  E: NSeg +> set of NPoint
  EndPoints: set of NSeg -> set of NPoint

  CycleCheck: set of NSeg +> bool
  AllLists: set of NSeg +> set of seq of NSeg
types
  Cycle = set of NSeg
functions
  OnCycle: NPoint * Cycle +> bool
  InsideCycle: NPoint * Cycle +> bool
  OutsideCycle: NPoint * Cycle +> bool
  SR: NPoint * Cycle +> set of NSeg
  SI: NPoint * Cycle +> set of NSeg
  SP: NPoint +> NSeg
  IsOdd: nat +> bool

  Partition: (NPoint * set of NSeg -> bool) * Cycle +> set of NPoint

  P: Cycle +> set of NPoint

  AreaInside: Cycle * Cycle +> bool
  EdgeInside: Cycle * Cycle +> bool
  VertexInside: Cycle * Cycle +> bool
  AreaDisjoint: Cycle * Cycle +> bool
  EdgeDisjoint: Cycle * Cycle +> bool
  VertexDisjoint: Cycle * Cycle +> bool
  AdjacentCycles: Cycle * Cycle +> bool
  MeetCycles: Cycle * Cycle +> bool

  SAreaInside: NSeg * Cycle +> bool
  SEdgeInside: NSeg * Cycle +> bool
  SVertexInside: NSeg * Cycle +> bool

  PAreaInside: NPoint * Cycle +> bool
  PVertexInside: NPoint * Cycle +> bool
types
  Face :: c  : Cycle
functions
  PAreaInsideF: NPoint * Face +> bool
  SAreaInsideF: NSeg * Face +> bool

  FAreaInside: Face * Face +> bool
  FAreaDisjoint: Face * Face +> bool
  FEdgeDisjoint: Face * Face +> bool
end REALM

~~~{% endraw %}

###test.vdmsl

{% raw %}
~~~

module TEST
imports from REALM all
exports all
definitions
values
  p1: REALM`NPoint = mk_REALM`NPoint(1,1);
  p2: REALM`NPoint = mk_REALM`NPoint(5,3);
  p3: REALM`NPoint = mk_REALM`NPoint(1,9);
  p4: REALM`NPoint = mk_REALM`NPoint(2,3);
  p5: REALM`NPoint = mk_REALM`NPoint(9,5);
  p6: REALM`NPoint = mk_REALM`NPoint(6,9);
  p7: REALM`NPoint = mk_REALM`NPoint(4,5);
  p8: REALM`NPoint = mk_REALM`NPoint(4,6);
  p9: REALM`NPoint = mk_REALM`NPoint(1,6);
  p10:REALM`NPoint = mk_REALM`NPoint(5,0);
  p11:REALM`NPoint = mk_REALM`NPoint(5,1);
  p12:REALM`NPoint = mk_REALM`NPoint(6,0);
  p13:REALM`NPoint = mk_REALM`NPoint(6,1);
  s1: REALM`NSeg = mk_REALM`NSeg({p1,p2});
  s2: REALM`NSeg = mk_REALM`NSeg({p1,p3});
  s3: REALM`NSeg = mk_REALM`NSeg({p2,p4});
  s4: REALM`NSeg = mk_REALM`NSeg({p4,p3});
  s5: REALM`NSeg = mk_REALM`NSeg({p3,p2});
  s6: REALM`NSeg = mk_REALM`NSeg({p5,p4});
  s7: REALM`NSeg = mk_REALM`NSeg({p6,p1});
  s8: REALM`NSeg = mk_REALM`NSeg({p5,p3});
  s9: REALM`NSeg = mk_REALM`NSeg({p5,p7});
  s10:REALM`NSeg = mk_REALM`NSeg({p9,p3});
  s11:REALM`NSeg = mk_REALM`NSeg({p10,p8});
  s12:REALM`NSeg = mk_REALM`NSeg({p1,p5});
  s13:REALM`NSeg = mk_REALM`NSeg({p10,p13});
  s14:REALM`NSeg = mk_REALM`NSeg({p11,p12});
  r1: REALM`Realm = mk_REALM`Realm({p1,p2},{s1});
  r2: REALM`Realm = mk_REALM`Realm({p5,p4},{s6});
  r3: REALM`Realm = mk_REALM`Realm({p5,p4,p3},{s6,s8});
  r4: REALM`Realm = mk_REALM`Realm({p1,p3,p4,p5,p6,p7,p8},{s6,s8});
  r5: REALM`Realm = mk_REALM`Realm({p10,p13},{s13})
end TEST

~~~{% endraw %}
