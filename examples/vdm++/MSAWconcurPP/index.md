---
layout: default
title: MSAWconcur
---

~~~
This VDM++ model is made by August Ribeiro as input for the VDM
This project is currently not running with the Overture interpreter.
#******************************************************
~~~
###AirSpace.vdmpp

{% raw %}
~~~
class AirSpace is subclass of GLOBAL
instance variables
airspace : map FOId to FO := {|->};
inv forall foid1, foid2 in set dom airspace & 
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
class AirTrafficController is subclass of GLOBAL, BaseThread
instance variables  
busy      : bool            := false;
operations
public AirTrafficController: nat1 * bool ==> AirTrafficController
OverviewAllRadars: () ==> map FOId to FO
private getDirectionVectors : FOId ==> seq of Vector
public getAltitudeHistory : FOId ==> seq of nat

public updateHistory : () ==> ()
private registerHistory : FO ==> ()
private cleanUpHistory : () ==> ()
functions
private last : History -> Position
operations
public addRadar : Radar ==> ()

public addObstacle : Obstacle ==> ()
public findThreats : () ==> ()
public UpdatesPresent:() ==> ()
operations
public detectedByTwoRadars : set of Radar ==> set of FO
public detectedByAllRadars : set of Radar ==> set of FO
isFOSafe : Obstacle * Position ==> bool

isFOatSafeAltitude : MinimumSafetyAltitude * Position ==> bool
willFObeSafe : Obstacle * FO ==> ()

private writeObjectWarning : Obstacle * FO ==> ()  
private writeRadarWarning : Radar ==> ()
private isPredictPossible : FO ==> [set of Position]

private predictPosition : FO ==> set of Position
  in
functions
private predictAltitude : seq of nat -> nat

operations  
public Step : () ==> ()
--thread
sync 
end AirTrafficController
~~~{% endraw %}

###BaseThread.vdmpp

{% raw %}
~~~
class BaseThread
instance variables
protected period : nat1 := 1;
operations
protected BaseThread : () ==> BaseThread
Step : () ==> ()
thread
end BaseThread
~~~{% endraw %}

###environment.vdmpp

{% raw %}
~~~
class Environment is subclass of GLOBAL, BaseThread
types 
InputTP   = (Time * seq of inline);
inline  = FOId * int * int * Altitude * Time;
FOOut = FOId * Coordinates * Altitude * FOWarning * 

outline = FOOut | RadarOut; 

instance variables 
  io : IO := new IO();
  airspace : [AirSpace] := nil;
public Environment : String * nat1 * bool ==> Environment
public setAirSpace : AirSpace ==> ()
public handleFOWarningEvent : FOId * Coordinates * Altitude * FOWarning * 
public handleRadarWarningEvent : Coordinates * nat1 * RadarWarning * nat *  Time ==> ()
public showResult : () ==> ()

private updateFOs : () ==> ()

public isFinished : () ==> () 
public Step: () ==> ()
sync
mutex(handleFOWarningEvent);
mutex(handleRadarWarningEvent);
--thread
--    if updating

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
public Altitude = real;
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
public test : real * real * real * real ==> Vector * Vector * real * real * Vector * real * real
end GLOBAL

~~~{% endraw %}

###MSAW.vdmpp

{% raw %}
~~~
class MSAW is subclass of GLOBAL
instance variables 
public static atc : AirTrafficController := new AirTrafficController(1, true);

public static airspace : AirSpace := new AirSpace();
public static militaryZone : Obstacle := 
public static radar1 : Radar := new Radar(6,11,20, 1, true);
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
class Radar is subclass of GLOBAL, BaseThread
types 
instance variables
operations
public Radar : int * int * nat1 * nat1 * bool ==> Radar
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
private addNewlyDetected : map FOId to FO ==> ()
public isFinished: () ==> ()
public Step: () ==> ()
functions
set2seqFOm : set of FO -> nat
--thread
--while true do
sync 
per isFinished => not busy;
end Radar
~~~{% endraw %}

###TimeStamp.vdmpp

{% raw %}
~~~

class TimeStamp


values
public stepLength : nat = 1;
instance variables
currentTime  : nat   := 0;
operations
public TimeStamp : nat ==> TimeStamp
public RegisterThread : BaseThread ==> ()
public UnRegisterThread : () ==> ()
public IsInitialising: () ==> bool
public DoneInitialising: () ==> ()
public WaitRelative : nat ==> ()
public WaitAbsolute : nat ==> ()
BarrierReached : () ==> ()
AddToWakeUpMap : nat * [nat] ==> ()
public NotifyThread : nat ==> ()
public GetTime : () ==> nat
Awake: () ==> ()
public ThreadDone : () ==> ()
sync
mutex(IsInitialising);
  mutex(AddToWakeUpMap, NotifyThread);
  mutex(AddToWakeUpMap, NotifyThread, BarrierReached);
end TimeStamp
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
      timerRef.DoneInitialising();
public Run : () ==> ()
  env.showResult()
end World
~~~{% endraw %}
