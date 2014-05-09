---
layout: default
title: PacemakerSeq
---

~~~
This model is made by Hugo Macedo as a part of his MSc thesis of a
Hugo Macedo, Validating and Understanding Boston Scientific Pacemaker
Hugo Daniel Macedo, Peter Gorm Larsen and John Fitzgerald, Incremental 
#******************************************************
~~~
###Accelerometer.vdmpp

{% raw %}
~~~

class Accelerometer is subclass of GLOBAL
operations
 public 
end Accelerometer

~~~{% endraw %}

###Environment.vdmpp

{% raw %}
~~~

class Environment is subclass of GLOBAL
types
public InputTP   = (Time * seq of Inpline)
public Inpline = (Sense * Chamber * ActivityData * Time);
public Outline = (Pulse * Chamber * Time);  
 instance variables
-- Input/Output 
inplines : seq of Inpline := [];
instance variables
busy : bool := true;
-- Amount of time we want to simulate
 instance variables
-- Leads
leads : map Chamber to Lead := {|->};
-- Accelerometer

 operations
-- Constructor

public 
public 


public 


private 


public 

public

public 
end Environment


~~~{% endraw %}

###GLOBAL.vdmpp

{% raw %}
~~~

class GLOBAL
types 

-- Sensed activity
-- Heart chamber identifier

-- Accelerometer output

-- Paced actvity
-- Operation mode

-- PPM

-- Time
end GLOBAL

~~~{% endraw %}

###HeartController.vdmpp

{% raw %}
~~~

class HeartController is subclass of GLOBAL
instance variables 
 leads     : map Chamber to Lead;
operations
 public 

 public 
 public 

 public 
 private
 private
 private
 public 
 public 
 public 
 public 
end HeartController

~~~{% endraw %}

###Lead.vdmpp

{% raw %}
~~~

class Lead is subclass of GLOBAL
instance variables
 private chamber : Chamber;       
operations
 public 

 public 

 public 

 public
 public 

public
 private 
 private 
   );
end Lead 

~~~{% endraw %}

###Pacemaker.vdmpp

{% raw %}
~~~

class Pacemaker 
 instance variables
 public static 
 public static 

 instance variables
 public static 
 public static 
 instance variables
 public static 
end Pacemaker

~~~{% endraw %}

###RateController.vdmpp

{% raw %}
~~~

class RateController is subclass of GLOBAL
instance variables
instance variables
operations
 public 
   );
public

 public 

 private
 public 
 private

 private
values
V_LOW : ActivityData = 1;
end RateController

~~~{% endraw %}

###Timer.vdmpp

{% raw %}
~~~

class Timer is subclass of GLOBAL
 instance variables
currentTime : Time := 0;

 values
stepLength : Time = 50;

 operations
public 

public 

end Timer

~~~{% endraw %}

###World.vdmpp

{% raw %}
~~~

class World is subclass of GLOBAL
types
instance variables
public static env      : [Environment] := nil;
operations
public World: seq of char * Mode ==> World
     -- bind leads to the environment
     -- bind accelerometer to the environment
     -- bind leads to the controler
     -- set up mode
public Run: () ==> ()

end World

~~~{% endraw %}
