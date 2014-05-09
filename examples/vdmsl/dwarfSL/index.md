---
layout: default
title: dwarf
---

~~~

This is a VDM-SL Specification of the Dwarf Signal Controller. The VDM

#******************************************************
~~~
###dwarf.vdmsl

{% raw %}
~~~
types
  LampId = <L1> | <L2> | <L3>;
values
  darklamps: set of LampId = {};
  stoplamps: set of LampId = {<L1>,<L2>};
  warninglamps: set of LampId = {<L1>,<L3>};
  drivelamps: set of LampId = {<L2>,<L3>};
types
  Signal = set of LampId;
  LogCom = <stop> | <dark> | <drive> | <warning>;
  Message = LogCom | <unknown> | <port_failure>;
  Errors = set of LampId;
operations
  Control: [LogCom] * set of LampId ==> Message * Errors * Trace
functions
  AllowedCommand: [LogCom] * Signal +> bool
types
  Trace = seq of set of LampId
state Dwarf of
operations
  NormalTrans: [LogCom] ==> Dwarf
  ErrorCorrection: [LogCom] * Dwarf * set of LampId ==> 
  NoCorrection: [LogCom] * Dwarf * set of LampId ==> 
functions
  MaxOneLampChange: Trace +> bool
  StopToDriveOrWarning: Trace +> bool
  ToAndFromDark: Trace +> bool
  ToOrFromStop: Trace * nat1 +> bool
  AlwaysDefinedState: Signal +> bool
traces
  SeqTest: (let com : LogCom
~~~{% endraw %}
