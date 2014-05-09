---
layout: default
title: pacemaker
---

~~~
The Pacemaker Challenge problem has been proposed by the 
Hugo Daniel Macedo, Peter Gorm Larsen and John Fitzgerald, 
~~~
###PacemakerAAI.vdmsl

{% raw %}
~~~

module PacemakerAAI
definitions 
values 
types 
Sense = <NONE> | <PULSE>;

ReactionTimeline = seq of Reaction; 
Reaction = <NONE> | <PULSE>;
functions
Pacemaker (inp : SenseTimeline) r : ReactionTimeline

end PacemakerAAI

~~~{% endraw %}

###PacemakerAAT.vdmsl

{% raw %}
~~~

module PacemakerAAT
definitions 
values 
types 
Sense = <NONE> | <PULSE>;
Time = nat1;
ReactionTimeline = seq of Reaction;
Reaction = <NONE> | <PULSE>;
functions
Pacemaker (inp : SenseTimeline) r : ReactionTimeline

end PacemakerAAT

~~~{% endraw %}

###PacemakerAOO.vdmsl

{% raw %}
~~~

module PacemakerAOO
definitions 
values 
types 
Sense = <NONE> | <PULSE>;
Time = nat1;
ReactionTimeline = map Time to Reaction; 
Reaction = <NONE> | <PULSE>;
functions
Pacemaker (inp : SenseTimeline) r : ReactionTimeline
end PacemakerAOO

~~~{% endraw %}

###PacemakerAOOR.vdmsl

{% raw %}
~~~

module PacemakerAOOR
definitions 
types 
Time = nat;
SenseTimeline = seq of (Sense * [AccelerometerData] * Time)
AccelerometerData = nat
Sense = <NONE> | <PULSE>;


ReactionTimeline = seq of (Reaction * Time); 
Reaction = <NONE> | <PULSE>;
state Sigma of

operations
Pacemaker : SenseTimeline ==> ReactionTimeline
HeartController : (Sense * [AccelerometerData] * Time) ==> (Reaction * Time)
   );   
 applyChange : <INC> | <DEC> ==> ()
 AdjustRate : AccelerometerData * Time ==> ()




values 
LOW  : AccelerometerData = 0;
sensedData : seq of (Sense * [AccelerometerData] * Time) = 
end PacemakerAOOR

~~~{% endraw %}

###PacemakerDDD.vdmsl

{% raw %}
~~~

module PacemakerDDD
definitions 
values 
types 
Chamber = <ATRIUM> | <VENTRICLE>;
Time = int;
Alarm = nat;
ReactionTimeline = set of (Time * Chamber); 

functions
Pacemaker : Time * SenseTimeline -> ReactionTimeline

PM : (Time * Time * SenseTimeline * ReactionTimeline * Alarm * Alarm * Time * Time) -> 
                                 if i = t

SensedAtrium : Time * ReactionTimeline * Alarm * Alarm * Time * Time -> ReactionTimeline * Alarm * Alarm * Time * Time
                             if t - LastA < ARP or VA > 0 or t - LastA < PVARP   -- 5.4.2  or 5.4.5 or 5.4.3

SensedVentricle : Time * ReactionTimeline * Alarm * Alarm * Time * Time -> ReactionTimeline * Alarm * Alarm * Time * Time
                                if t - LastV < VRP -- 5.4.3

SensedNothing : Time * ReactionTimeline * Alarm * Alarm * Time * Time -> ReactionTimeline * Alarm * Alarm * Time * Time
             if AA > 0 and t >= AA                                           -- Atrium alarm is set and fired



-- Auxiliar funtcions
-- A curry function
end PacemakerDDD

~~~{% endraw %}

###PacemakerDOO.vdmsl

{% raw %}
~~~

module PacemakerDOO
definitions 
types 
Time = nat;
SensedTimeline = set of (Chamber * Time);
Chamber = <ATRIA> | <VENTRICLE>;
ReactionTimeline = set of (Chamber * Time);

values
functions
Pacemaker (mk_(inp,n) : SensedTimeline * nat1) r : ReactionTimeline
end PacemakerDOO

~~~{% endraw %}

###RateController.vdmsl

{% raw %}
~~~

module RateController
definitions 
types 
Input = map Time to ActivityData;
Time = nat1;
ActivityData = nat1

RF = nat1

Output = map Time to PPM; 
PPM = nat1

values
functions
Simulate(inp : Input)  out : Output



end RateController

~~~{% endraw %}
