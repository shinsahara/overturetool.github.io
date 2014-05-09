---
layout: default
title: iioss
---

~~~
This project was made by Christian Thillemann and Bardur Joensen in a
~~~
###Actuator.vdmrt

{% raw %}
~~~

class Actuator is subclass of IIOSSTYPES
instance variables
	public SetValues : EventId * PigPosition ==> ()


~~~{% endraw %}

###Environment.vdmrt

{% raw %}
~~~

class Environment is subclass of IIOSSTYPES
types
	public InputTP   = Time * seq of inline;
	public outline = EventId * EventType * seq of char * nat; 
instance variables
	-- access to the VDMTools stdio
	server : [Server] := nil;
	-- Amount of time we want to simulate
operations
	public Environment: seq of char ==> Environment
	public addServer : Server ==> ()
	public addSensor: Sensor ==> ()
	public getServer: () ==> Server
	public getNoSensors: () ==> nat
	private createSignal: () ==> () 

	public handleEvent: EventId * EventType *  seq of char * Time ==> ()
	public showResult: () ==> ()
	public isFinished : () ==> ()
	public GetAndPurgeOutlines: () ==> seq of outline
sync
	mutex (handleEvent);
thread

end Environment


~~~{% endraw %}

###IIOSS.vdmrt

{% raw %}
~~~


system IIOSS
instance variables

	--BUSs



	-- stable controller
	-- Sensors for stableController1
	-- Sensors for stableController2
	-- Actuators for stableController1
	-- Actuators for stableController2
operations
	cpu6.deploy(sensor3);
	cpu10.deploy(actuator3);
);
end IIOSS


~~~{% endraw %}

###iiosstypes.vdmrt

{% raw %}
~~~

class IIOSSTYPES
types
	public Position::
	public EventId = nat;
	public EventType = <SHOW_PIG> | <PIG_MOVED> | <PIG_NEW> | <NEED_MEDIC> | <NONE>;

operations
end IIOSSTYPES


~~~{% endraw %}

###Sensor.vdmrt

{% raw %}
~~~

class Sensor is subclass of IIOSSTYPES 

operations

	async public trip : EventType * PigId * [Position] ==> ()
end Sensor


~~~{% endraw %}

###Server.vdmrt

{% raw %}
~~~

class Server is subclass of IIOSSTYPES
types
	private medicTimes : seq of medicTime := [mk_(1,1,5000), mk_(2,5,8000)];
operations
	public GetNoPigs: () ==> nat
	public PointAtPig: EventId * PigId ==> ()
	-- Add a Pig to 
		-- Remove a Pig 
	public NeedMedic : () ==> ()
				);
	public isFinished: () ==> ()
sync
	per PointAtPig => card rng stables > 0;
	per isFinished => not busy
thread


~~~{% endraw %}

###StableController.vdmrt

{% raw %}
~~~

class StableController is subclass of IIOSSTYPES
instance variables
	private sensors: map Sensor to PigStyId := {|->}; --map sensor to grisesti
	private busy : bool := false;
	private pigsInSty : map PigPosition to PigStyId := {|->}; --map pigID to pigsty;
operations
	-- Add a Pig to pigSty and inform the server

	-- Remove a Pig from pigSty and inform the server

	async public PointAtPig: EventId * PigId ==> ()
	public AddActuator: Actuator * PigStyId ==> ()
	public AddSensor: Sensor * PigStyId ==> ()

sync
end StableController


~~~{% endraw %}

###World.vdmrt

{% raw %}
~~~

class World
instance variables
	-- maintain a link to the environment
operations
	public World: () ==> World
		-- add sensors
	   --server
		-- controller 1
	   IIOSS`StableController1.AddActuator(IIOSS`actuator1,1);

		-- controller 2
	   IIOSS`StableController2.AddActuator(IIOSS`actuator3,3);
	   --env.isFinished();
		start(IIOSS`server);
	 );  

	public Run: () ==> ()
end World


~~~{% endraw %}
