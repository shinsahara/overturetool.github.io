---
layout: default
title: SAFER
---

~~~
This VDM-SL model is a response to the PVS model of the SAFER system
S.Agerholm and P.G.Larsen, Modeling and Validating SAFER in VDM-SL, 
#******************************************************
~~~
###aah.vdmsl

{% raw %}
~~~



module AAH
imports from AUX all,
exports all
definitions
state AAH of 
types
  EngageState = <AAH_off> | <AAH_started> | <AAH_on> | <pressed_once> |
values
  click_timeout: nat = 10 -- was 100, changed for test purposes
operations
  Transition: HCM`ControlButton * AUX`SixDofCommand * nat ==> ()
  ActiveAxes: () ==> set of AUX`RotAxis
  IgnoreHcm: () ==> set of AUX`RotAxis
  Toggle: () ==> EngageState
functions
  AllAxesOff: set of AUX`RotAxis +> bool
  ButtonTransition: EngageState * HCM`ControlButton * set of AUX`RotAxis * 
end AAH


~~~{% endraw %}

###auxilary.vdmsl

{% raw %}
~~~

module AUX 
exports all
definitions
values
  arbitrary_value = mk_token(1001);
  axis_command_set : set of AxisCommand = {<Neg>,<Zero>,<Pos>};
  tran_axis_set : set of TranAxis = {<X>,<Y>,<Z>};
  rot_axis_set : set of RotAxis = {<Roll>,<Pitch>,<Yaw>};
  null_tran_command : TranCommand = {a |-> <Zero> | a in set tran_axis_set};
  null_rot_command : RotCommand = {a |-> <Zero> | a in set rot_axis_set};
  null_six_dof : SixDofCommand 
types
  AxisCommand = <Neg> | <Zero> | <Pos>;
  TranAxis = <X> | <Y> | <Z>;
  RotAxis = <Roll> | <Pitch> | <Yaw>;
  TranCommand = map TranAxis to AxisCommand
  RotCommand = map RotAxis to AxisCommand
  SixDofCommand ::
end AUX


~~~{% endraw %}

###geom.vdmsl

{% raw %}
~~~
dlmodule GEOM
exports
  operations

  uselib
end GEOM
~~~{% endraw %}

###gui.vdmsl

{% raw %}
~~~

dlmodule GUI
  exports
  uselib
end GUI

~~~{% endraw %}

###hcm.vdmsl

{% raw %}
~~~


module HCM
imports from AUX all
exports all
definitions
types
  SwitchPositions ::
  ControlModeSwitch = <Rot> | <Tran>;
  ControlButton = <Up> | <Down>;
  HandGripPosition:: vert  : AUX`AxisCommand
-- add inv to exclude impossible combinations???
functions
  GripCommand: HandGripPosition * ControlModeSwitch +> AUX`SixDofCommand
end HCM


~~~{% endraw %}

###safer.vdmsl

{% raw %}
~~~



module SAFER
imports from AUX all,

exports all
definitions
state SAFER of
operations
  ControlCycle: HCM`SwitchPositions * HCM`HandGripPosition * AUX`RotCommand ==> 
functions
  ThrusterConsistency: set of TS`ThrusterName +> bool

end SAFER


~~~{% endraw %}

###test.vdmsl

{% raw %}
~~~


module TEST
imports from SAFER all,


   from GUI 
    operations
  exports all
definitions
values 
  switches_tran_up = mk_HCM`SwitchPositions(<Tran>,<Up>);
  switch_positions : set of HCM`SwitchPositions = 
  zero_grip : HCM`HandGripPosition = mk_HCM`HandGripPosition(<Zero>,<Zero>,
  all_grip_positions : set of HCM`HandGripPosition =
  all_rot_commands : set of AUX`RotCommand =
  grip_positions : set of HCM`HandGripPosition 
  possibilities -- total of 972 cases
functions
  BigTest: () -> 
   HugeTest: () -> 
  ConvertAxisCmd: seq of char -> AUX`AxisCommand
  ConvertTIds: TS`ThrusterSet +> seq of seq of char
  ConvertTId: TS`ThrusterName +> seq of char
operations
  StartTest: () ==> ()
  RunTest: () ==> bool

  Loop : () ==> ()
  Move: () ==> ()
  NoMove: () ==> ()

end TEST


~~~{% endraw %}

###ts.vdmsl

{% raw %}
~~~



module TS
imports from AUX all,
exports all
definitions
types 
  ThrusterName = <B1> | <B2> | <B3> | <B4> | <F1> | <F2> | <F3> | <F4> |
  ThrusterSet = set of ThrusterName
functions
  RotCmdsPresent: AUX`RotCommand +> bool 
  PrioritizedTranCmd: AUX`TranCommand +> AUX`TranCommand
  CombinedRotCmds: AUX`RotCommand * AUX`RotCommand * set of AUX`RotAxis +> 
  IntegratedCommands: AUX`SixDofCommand * AUX`RotCommand * 
  BFThrusters: AUX`AxisCommand * AUX`AxisCommand * AUX`AxisCommand +> 
  LRUDThrusters: AUX`AxisCommand * AUX`AxisCommand * AUX`AxisCommand +> 
  SelectedThrusters: AUX`SixDofCommand * AUX`RotCommand * set of AUX`RotAxis * 
end TS



~~~{% endraw %}

###workspace.vdmsl

{% raw %}
~~~
module WorkSpace
imports from SAFER all,
exports all
definitions
types 
Input = seq of nat
ThrusterMatrix = seq of seq of bool
functions
RunControlCycle: Input -> ThrusterMatrix
TransformInput: Input -> HCM`SwitchPositions * HCM`HandGripPosition * 
ConvertAxisCmd: nat -> AUX`AxisCommand
GenerateThrusterMatrix: TS`ThrusterSet +> ThrusterMatrix
GenerateThrusterLabel: TS`ThrusterName +> nat * nat
values
switchpos = mk_HCM`SwitchPositions (<Tran>,<Down>);
end WorkSpace
~~~{% endraw %}
