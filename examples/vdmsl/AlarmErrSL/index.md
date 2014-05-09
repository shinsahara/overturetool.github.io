---
layout: default
title: Alarm
---

~~~

This is the alarm example from the VDM-SL book, John Fitzgerald and
#******************************************************
~~~
###alarm.vdmsl

{% raw %}
~~~
types
  Plant :: schedule : Schedule
  Schedule = map Period to set of Expert
  Period = token;
  Expert :: expertid : ExpertId
  ExpertId = token;
  Qualification = <Elec> | <Mech> | <Bio> | <Chem>;
  Alarm :: alarmtext : seq of char
functions
  NumberOfExperts: Period * Plant -> nat
  ExpertIsOnDuty: Expert * Plant -> set of Period
  ExpertToPage(a:Alarm,peri:Period,plant:Plant) r: Expert
  QualificationOK: set of Expert * Qualification -> bool

~~~{% endraw %}

###changeexpert.vdmsl

{% raw %}
~~~
functions
-- this function is NOT correct. Why not?
~~~{% endraw %}

###testalarm.vdmsl

{% raw %}
~~~
values
  p1:Period = mk_token("Monday day");
  eid1:ExpertId = mk_token(134);
  e1:Expert = mk_Expert(eid1,{<Elec>});
  s: map Period to set of Expert
  a1:Alarm = mk_Alarm("Power supply missing",<Elec>);
  plant1 : Plant = mk_Plant(s,{a1,a2,a3})
operations
Run: Expert ==> set of Period
traces 
  Test1: let a in set alarms
  Test2: let ex in set exs

~~~{% endraw %}
