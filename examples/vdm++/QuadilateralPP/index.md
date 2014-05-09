---
layout: default
title: Quadilateral
---

~~~
This example deals with quadilaterals (figures with four 
#******************************************************
~~~
###math.vdmpp

{% raw %}
~~~
class MATH
-- 	VDMTools STANDARD LIBRARY: MATH
  functions
public static
public static
public static
public static
public static
public static
public static
public static
public static
  operations
public static
public static
public static
  functions
public static
public static
public static
  values

end MATH
~~~{% endraw %}

###mathematics.vdmpp

{% raw %}
~~~
class Mathematics
  values
  types
  functions
    sqrt (r: real) res: real
end Mathematics
~~~{% endraw %}

###parallelogram.vdmpp

{% raw %}
~~~
class Parallelogram is subclass of Quadrilateral
   instance variables
   operations
end Parallelogram

~~~{% endraw %}

###quadrilateral.vdmpp

{% raw %}
~~~
class Quadrilateral is subclass of Vector
  instance variables
  operations
    public
    public
end Quadrilateral

~~~{% endraw %}

###rectangle.vdmpp

{% raw %}
~~~
class Rectangle is subclass of Parallelogram
  instance variables
end Rectangle
~~~{% endraw %}

###rhombus.vdmpp

{% raw %}
~~~
class Rhombus is subclass of Parallelogram
  instance variables
end Rhombus
~~~{% endraw %}

###square.vdmpp

{% raw %}
~~~
class Square is subclass of Rhombus, Rectangle
end Square
~~~{% endraw %}

###vector.vdmpp

{% raw %}
~~~
class Vector 
  values
  types
    public
    public
  functions
    public
    public
end Vector

~~~{% endraw %}

###workspace.vdmpp

{% raw %}
~~~
class WorkSpace is subclass of Vector
  types
  instance variables
  operations
    LookUp: Token ==> Quadrilateral
    GetAngle: Token ==> real
    Display: Token * Quadrilateral ==> ()
    UnDisplay: Token ==> ()
    Move: Token * (nat * nat) * (nat * nat) ==> ()
end WorkSpace

~~~{% endraw %}
