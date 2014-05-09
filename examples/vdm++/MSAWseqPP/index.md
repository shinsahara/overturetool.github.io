---
layout: default
title: MSAWseq
---

~~~
This VDM++ model is made by August Ribeiro as input for the VDM
2011-12-28 This VDM++ model has been updated by Rasmus Lauritsen 
lib/radar.jar contains binary and source code for the java radar 
#******************************************************
~~~
###AirSpace.vdmpp

{% raw %}
~~~
class AirSpace is subclass of GLOBAL
instance variables
--airspace : set of FO := {};
airspace : map FOId to FO := {|->};
operations
public addFO : FO ==> ()
public removeFO : FOId ==> ()
public getFO : FOId ==> FO
public getAirspace : () ==> set of FO
public updateFO : FOId * Coordinates * Altitude ==> ()


end AirSpace
~~~{% endraw %}

###atc.vdmpp

{% raw %}
~~~
class AirTrafficController is subclass of GLOBAL
instance variables  
radars    : set of Radar    := {};  
operations
public getDirectionVectors : FOId ==> seq of Vector
public getAltitudeHistory : FOId ==> seq of nat

public updateHistory : () ==> ()
private registerHistory : FO ==> ()
public cleanUpHistory : () ==> ()
functions
operations
public addRadar : Radar ==> ()
public addObstacle : Obstacle ==> ()
public findThreats : () ==> ()
public detectedByTwoRadars : set of Radar ==> set of FO
public detectedByAllRadars : set of Radar ==> set of FO
public Step : () ==> ()
functions 
isFOatSafeAltitude : MinimumSafetyAltitude * Position -> bool
operations
isFOSafe : Obstacle * Position ==> bool
willFObeSafe : Obstacle * FO ==> ()

private writeObjectWarning : Obstacle * FO ==> ()  
private writeRadarWarning : Radar ==> ()


private isPredictPossible : FO ==> [set of Position]

private predictPosition : FO ==> set of Position
  in
functions
private predictAltitude : seq of nat -> nat
end AirTrafficController
~~~{% endraw %}

###dk_au_eng_Radar.vdmpp

{% raw %}
~~~
class dk_au_eng_Radar

		-- Add a FO to the radar to track
		-- Remove a FO from the radar
		-- Make the scan line progress one step
        -- Update the position of a flying object given its transponder code 
        -- Set the step size, that is the angle by which the scan line is progressed when
        -- Set the width of the scan cone
        -- Set the time a scan takes
        -- Set the position of the Radar window (a nice model with two radars would want to 
        -- Set the title of the Radar Window
        -- Force the Scan Angle to be angle
		-- Run operation that makes 400 steps with two planes one of which is moving across.
            return 0);

end dk_au_eng_Radar
~~~{% endraw %}

###environment.vdmpp

{% raw %}
~~~
class Environment is subclass of GLOBAL
types 
inline  = FOId * int * int * Altitude * Time;
protected FOOut = FOId * Coordinates * Altitude * FOWarning * 

protected outline = FOOut | RadarOut; 
instance variables 
  io : IO := new IO();
  airspace : [AirSpace] := nil;
operations
public Environment : String ==> Environment

public setAirSpace : AirSpace ==> ()
public handleFOWarningEvent : FOId * Coordinates * Altitude * FOWarning * 
public handleRadarWarningEvent : Coordinates * nat1 * 

public showResult : () ==> ()


public Run : () ==> ()
private updateFOs : () ==> ()

public isFinished : () ==> bool 
end Environment
~~~{% endraw %}

###FO.vdmpp

{% raw %}
~~~
class FO is subclass of GLOBAL
instance variables 

operations
public FO : FOId * Coordinates * Altitude ==> FO
public getId : () ==> FOId
public getCoordinates : () ==> Coordinates
public setCoordinates : Coordinates ==> ()
public getAltitude : () ==> Altitude
public setAltitude : Altitude ==> ()
public getPosition : () ==> Position

end FO
~~~{% endraw %}

###GLOBAL.vdmpp

{% raw %}
~~~
class GLOBAL
types
public Altitude = real
public FOId = token;
public
public Time = nat;
public String = seq of char;
public ObstacleType = <Natural> | <Artificial> | <Airport>  | <Military_Area>;
public FOWarning = ObstacleType | <EstimationWarning>;   
public RadarWarning = <Saturated>;
public MinimumSafetyAltitude = nat | <NotAllowed>;
public Position ::
public History = seq of Position;
public Vector ::
functions
protected isPointInRange : Coordinates * nat1 * Coordinates -> bool
protected vectorSum : Vector * Vector -> Vector
protected vectorDiv : Vector * int -> Vector 
protected addVectorToPoint : Vector * Position -> Coordinates
protected vectorLength : Vector -> real 
protected unitVector : Vector -> Vector
protected dotProduct : Vector * Vector -> real
protected angleBetweenVectors : Vector * Vector -> real
protected radians2degree : real -> real
protected atan2 : real * real -> real
protected signedVectorAngle : Vector * Vector -> real
protected vectorAngle : Vector -> real * real
protected vectorRotate : Vector * real -> Vector
protected round : real -> real
operations
public test : real * real * real * real ==> Vector * Vector * 
end GLOBAL

~~~{% endraw %}

###MSAW.vdmpp

{% raw %}
~~~
class MSAW is subclass of GLOBAL
instance variables 
public static atc : AirTrafficController := new AirTrafficController();

public static radar1 : Radar := new Radar(6,11,20);

public static radar2 : Radar := new Radar (30,30,5);  
public static airspace : AirSpace := new AirSpace();
public static militaryZone : Obstacle := 

end MSAW
~~~{% endraw %}

###obstacle.vdmpp

{% raw %}
~~~
class Obstacle is subclass of GLOBAL
instance variables
  MSA            : MinimumSafetyAltitude ;
operations 
public Obstacle : MinimumSafetyAltitude * Coordinates * nat * nat * 
public getType : () ==> ObstacleType 
public getCoordinates : () ==> Coordinates
public getSecureRange : () ==> nat1
public getMSA : () ==> MinimumSafetyAltitude


end Obstacle 
~~~{% endraw %}

###Radar.vdmpp

{% raw %}
~~~
class Radar is subclass of GLOBAL
types 

instance variables
  location : Coordinates;
operations
public Radar : int * int * nat1 ==> Radar
public Scan : AirSpace ==> ()
private InRange : FO ==> bool
public getDetected : () ==> set of FO
public getDetectedMap : () ==> map FOId to FO
public saturatedRadar : () ==> bool
public getSaturatingFOs : () ==> set of FOId
public getLocation : () ==> Coordinates
public getRange : () ==> nat1
private UpdatePriorityList : () ==> ()
private removeNotDetected : set of FO ==> ()
private addNewlyDetected : set of FO ==> ()
private setupRadar: dk_au_eng_Radar ==> ()
private DisplayScan: () ==> ()
functions

set2seqFOm : set of FO -> nat

end Radar
~~~{% endraw %}

###timer.vdmpp

{% raw %}
~~~
class Timer 
instance variables
  currentTime : nat := 0;
values 
  stepLength : nat = 100;
operations
public 
public
end Timer
~~~{% endraw %}

###UseATC.vdmpp

{% raw %}
~~~
class UseATC
values
  id : token = mk_token(1);
instance variables
  atc: AirTrafficController := new AirTrafficController()
traces
TestATC: let r = new Radar(-8,-9,42)
end UseATC
~~~{% endraw %}

###world.vdmpp

{% raw %}
~~~
class World
instance variables  
public static
public static 


operations
public 
public 
end World
~~~{% endraw %}
