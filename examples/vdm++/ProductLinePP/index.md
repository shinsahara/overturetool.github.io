---
layout: default
title: ProductLine
---

~~~

#******************************************************
~~~
###CONTEXT-atmospheric-air-pressureplace-high.vdmpp

{% raw %}
~~~
class HighAtmosphericAirPressure is subclass of AtmosphericAirPressure
instance variables
end HighAtmosphericAirPressure
~~~{% endraw %}

###CONTEXT-atmospheric-air-pressureplace-low.vdmpp

{% raw %}
~~~
class LowAtmosphericAirPressure is subclass of AtmosphericAirPressure
instance variables
end LowAtmosphericAirPressure
~~~{% endraw %}

###CONTEXT-atmospheric-air-pressureplace-normal.vdmpp

{% raw %}
~~~
class NormalAtmosphericAirPressure is subclass of AtmosphericAirPressure
instance variables
end NormalAtmosphericAirPressure
~~~{% endraw %}

###CONTEXT-atmospheric-air-pressureplace.vdmpp

{% raw %}
~~~
class AtmosphericAirPressure
types
instance variables
operations
  public
end AtmosphericAirPressure
~~~{% endraw %}

###CONTEXT-liquid-water.vdmpp

{% raw %}
~~~
class Water is subclass of Liquid
operations
end Water
~~~{% endraw %}

###CONTEXT-liquid.vdmpp

{% raw %}
~~~
class Liquid
instance variables
operations
  public
  public
  public
  public
  public
  public
  public
end Liquid
~~~{% endraw %}

###REALWORLD-low-water.vdmpp

{% raw %}
~~~
class RealWorld1
instance variables
  -- only realworld_liquid variable can be accessed
  -- CONTEXT-atmospheric-air-pressureplace-low and
operations
end RealWorld1
~~~{% endraw %}

###REALWORLD-normal-water.vdmpp

{% raw %}
~~~
class RealWorld2
instance variables
  -- only realworld_liquid variable can be accessed
  -- CONTEXT-atmospheric-air-pressureplace-normal and
operations
end RealWorld2
~~~{% endraw %}

###SYSTEM-HW-heater.vdmpp

{% raw %}
~~~
class Heater
types
instance variables
operations
  public On: () ==> ()
  public Off: () ==> ()
end Heater
~~~{% endraw %}

###SYSTEM-HW-liquid-level-sensor.vdmpp

{% raw %}
~~~
class LiquidLevelSensor
instance variables
operations
  public
end LiquidLevelSensor
~~~{% endraw %}

###SYSTEM-HW-thermistor.vdmpp

{% raw %}
~~~
class Thermistor
instance variables
operations
  public
end Thermistor
~~~{% endraw %}

###SYSTEM-SW-controller.vdmpp

{% raw %}
~~~
class Software
instance variables
operations
  public
end Software
~~~{% endraw %}

###USER-test.vdmpp

{% raw %}
~~~
class UserTest
instance variables
operations
end UserTest
~~~{% endraw %}
