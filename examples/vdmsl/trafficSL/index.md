---
layout: default
title: traffic
---

~~~
This example is for all small traffic light control kernel presented in the VDM-SL book
A safety kernel for traffic light control, Paul Ammann, Aerospace and Electronic 
#******************************************************
~~~
###traffic.vdmsl

{% raw %}
~~~
-- Traffic light control kernel
values
-- The following value definitions are used to construct a
  p1 : Path = mk_token("A1North");
  p2 : Path = mk_token("A1South");
  p3 : Path = mk_token("A66East");
  p4 : Path = mk_token("A66West");
  lights : map Path to Light
  conflicts : set of Conflict
  kernel : Kernel 
types
  Light = <Red> | <Amber> | <Green>;
  Time = real
  Path = token;
  Conflict :: path1: Path
-- the kernel data structure has two components representing 
  Kernel :: lights    : map Path to Light
functions
-- changing the light to green for a given path
  ToGreen: Path * Kernel -> Kernel
-- changing the light to red for a given path
  ToRed: Path * Kernel -> Kernel
-- changing the light to amber for a given path
  ToAmber: Path * Kernel -> Kernel
  ChgLight: (map Path to Light) * Path * Light -> (map Path to Light)
~~~{% endraw %}
