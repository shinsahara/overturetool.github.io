---
layout: default
title: CyberRail
---

~~~
This VDM-RT model was produced as a part of an MSn thesis investigating and analyzing
~~~
###ActivePlanManager.vdmrt

{% raw %}
~~~


class ActivePlanManager is subclass of Strategy

instance variables

private activeTokens : inmap TokenDevice to TransportPlan := {|->};
private busy : bool := false;
private q_CR_out: CyberRail; 
private tokenDevices : set of TokenDevice;
private state : State := <run>;
types
public State =  <run> | <halt>;
operations

--Strategy-------------------------------------------------
strategyNotify : () ==> ()

strategyEnd : () ==> ()

	public isFinished : () ==> ()
  public inactiveRoute : nat ==> ()
			if t.containsRoute(id_route)

public addTransportPlan : TransportPlan * TokenDevice ==> ()
   activeTokens := activeTokens ++ {tokenDevice|-> plan};
public removeTransportPlan : TransportPlan * TokenDevice ==> ()
public getPlans : () ==> set of TransportPlan

public handleEvent : () ==> ()
		else if(state = <run> and len q_Tok_out <> 0)
		else if(state = <run> and len q_Tok_in <> 0)

private handleQ_Tok_out : () ==> ()
private handleQ_CR_in : () ==> ()
		let msg = hd q_CR_in in
private handleQ_Tok_in : () ==> ()
private handleStrategyInit : () ==> ()
private handleStrategyEnd : () ==> ()
private handleTransportPlan :  MessageTypes`MessageT ==> ()
	let mk_MessageTypes`RETURNPLAN(dto, tok) = msg in
private handleRequestPlan : MessageTypes`MessageT ==> ()
	let mk_MessageTypes`REQUESTPLAN(navi, tok) = msg in
private reduce_Q_Tok : () ==> ()
private reduce_Q_CR : () ==> ()
--Initialize CyberRail ref.


public addToSystemQueue : MessageTypes`MessageT ==> ()
async public addToClientQueue : MessageTypes`MessageT ==> ()

thread



sync
per handleEvent => ((len q_CR_in + len q_Tok_in + len q_Tok_out) > 0);
end ActivePlanManager


~~~{% endraw %}

###Company.vdmrt

{% raw %}
~~~

class Company is subclass of Environment
instance variables
  private cyberrail : CyberRail;
operations
  public Company : seq of char ==> Company
  public composeTransportGrid : () ==> ()
	public isFinished : () ==> ()
  public stimulate : () ==> ()
thread
sync
per isFinished => not busy;


~~~{% endraw %}

###CRSystem.vdmrt

{% raw %}
~~~

system CRSystem
instance variables
-- cpu for CyberRail
-- cpu for TokenDevice
-- cpu for APM
-- bus to connect CyberRail and APM
-- bus to connect TokenDevice to APM
--bus to connect cb token device
public static tok1 : TokenDevice := new TokenDevice(1);
operations
CRSystem : () ==> CRSystem
)
end CRSystem

~~~{% endraw %}

###Customer.vdmrt

{% raw %}
~~~

class Customer is subclass of Environment
instance variables
operations
public Customer : seq of char ==> Customer
	outfileName := "Results for " ^ fname
public Customer : () ==> Customer
public addCyberRail : CyberRail ==> ()
public addTokenDevice : TokenDevice ==> ()
	public isFinished : () ==> ()
	public test : () ==> map nat to TokenDevice

  public stimulate : () ==> ()
		if len inlines > 0
							reduceInline();
private isDone : () ==> ()
private reduceInline : () ==> ()
inputStimuli : () ==> ()
thread

sync
end Customer


~~~{% endraw %}

###CyberRail.vdmrt

{% raw %}
~~~



class CyberRail
instance variables
private normalState : bool := true;
private railway : RailwayGrid;
types
public NavigationInput :: 
inv n == len n.departureLocation > 0 and
public String = seq of char;

operations

public setRailwayGrid : RailwayGrid ==> ()
public isFinished : () ==> ()

public calculateTransportPlan : NavigationInput * TokenDevice ==> 

public setActiveRoute : nat ==> ()

private findCheapest : RailwayGrid`Grid ==> [seq of TransportPlan`Route]
	for all s in set list do
private findQuickest : RailwayGrid`Grid ==> [seq of TransportPlan`Route]
public setQ_APM_out : ActivePlanManager ==> ()
async public addToStimuliQueue : MessageTypes`MessageT ==> ()
public addToSystemQueue : MessageTypes`MessageT ==> ()
public handleEvents : () ==> ()
		else if ( normalState and len q_APM_in > 0)
		if(not normalState and (curtime + timeout) <= time)
);

private handleQ_Env_in : () ==> ()
private handleQ_APM_in : () ==> ()
private handleCalcPlan : CyberRail`NavigationInput * TokenDevice ==> ()
private handleInactiveRoute : MessageTypes`MessageT ==> ()
);
private finalizeInactiveRoute : () ==> ()

public setInactiveRoute : nat ==> ()
private reduce_Q_APM : () ==> ()
private reduce_Q_Env : () ==> ()
thread

sync
per isFinished =>  (len q_Env_in + len q_APM_in) = 0;
mutex(reduce_Q_Env,addToStimuliQueue);

~~~{% endraw %}

###Environment.vdmrt

{% raw %}
~~~

class Environment
types 
public outline = [TransportPlan] * [nat] * nat;
instance variables
operations
  public stimulate : () ==> ()
  public isFinished : () ==> ()
  public respons : [TransportPlan] * [TransportPlan`Route] * nat ==> ()
  public showResults : () ==> ()
  public Environment : seq of char ==> Environment
sync
thread
end Environment


~~~{% endraw %}

###Logger.vdmrt

{% raw %}
~~~


class Logger
types
	public TP::
	public Fnc::
instance variables 
operations
	public static writeFnc : CyberRail`String ==> ()
	public static write : logType ==> () 
	public static flush : () ==> ()
	public static printLog : () ==> seq of logType	
sync
end Logger


~~~{% endraw %}

###MessageQueue.vdmrt

{% raw %}
~~~

class MessageQueue
instance variables
queue : seq of Message := [];

types
public Message::
operations
--Constructor

public push: Message ==> ()
public pop: () ==> Message
sync
end MessageQueue

~~~{% endraw %}

###RailwayGrid.vdmrt

{% raw %}
~~~


class RailwayGrid
instance variables
private routeList : set of TransportPlan`Route; 
types
public String = seq of char;

operations
--Constructor	
	routeList := {R1,R2,R3,R4,R5,R6,R7,R8,R9,R10}; 
	grid := recAlgo({},[], "A") union 
private recAlgo : Grid * Plan * String ==> Grid
public getGrid: () ==> Grid
public setInactiveRoute : nat ==> ()



---------------------------------------------------------------------------------------------------------------
private writef : Grid ==> ()
end RailwayGrid

~~~{% endraw %}

###snw.vdmrt

{% raw %}
~~~



class SNW is subclass of Strategy

instance variables
private state : State := <run>;
types
public State =  <run> | <halt>;
operations



strategyInit : () ==> ()
strategyNotify : () ==> ()

strategyEnd : () ==> ()
handleEvents : ActivePlanManager ==> ()

end SNW


~~~{% endraw %}

###strategy.vdmrt

{% raw %}
~~~


class Strategy
types

operations
strategyInit : () ==> ()
strategyNotify : () ==> ()
strategyEnd : () ==> ()
handleEvents : () ==> ()


end Strategy


~~~{% endraw %}

###TokenDevice.vdmrt

{% raw %}
~~~


class TokenDevice
instance variables
  private id_token : nat := 1;

operations
 public TokenDevice : nat ==> TokenDevice

 public notifyPassenger : TransportPlan ==> ()

  public requestTransportPlan : CyberRail`NavigationInput ==> ()
public getTokenId : () ==> nat
public routeTraveled : () ==> ()
public setTransportPlan : TransportPlan ==> ()
public travel : () ==> ()
public onTheRoad : () ==> ()
public isFinished : () ==> ()




--Setup handles----------------------------------

public setQ_Env_out : Environment ==> ()
public setQ_APM_out : ActivePlanManager ==> ()

thread
sync
per onTheRoad => (transportPlan <> nil) and (len transportPlan.routeList > 0);
per isFinished => (transportPlan = nil) or (len transportPlan.routeList = 0);
mutex(requestTransportPlan);
end TokenDevice


~~~{% endraw %}

###TransportPlan.vdmrt

{% raw %}
~~~



class TransportPlan
instance variables
inv len routeList > 0 => forall i in set inds routeList 

types
public Route ::
public DTO ::


operations
--Constructor
public getNextRoute : () ==> TransportPlan`Route
public containsRoute: nat ==> bool
public addRoute: Route ==> ()
public routeTraveled: () ==> ()
public routesRemaining : () ==> nat
public getByValue : () ==> DTO

public getPlanAsNaviInput: () ==> CyberRail`NavigationInput
public getTokenId: () ==> nat
--debug
sync
end TransportPlan

~~~{% endraw %}

###types.vdmrt

{% raw %}
~~~


class MessageTypes
types
--Message Types
public MessageT = REQUESTPLAN | RETURNPLAN | CALCPLAN | 



operations
--public test : MessageT ==> seq of char


end MessageTypes



~~~{% endraw %}

###World.vdmrt

{% raw %}
~~~

class World
instance variables
operations

  public World : seq of char ==> World
		envCustomer.addCyberRail(CRSystem`cb);
		CRSystem`tok2.setQ_Env_out(envCustomer);
		CRSystem`tok3.setQ_Env_out(envCustomer);
		CRSystem`tok4.setQ_Env_out(envCustomer);
		CRSystem`tok5.setQ_Env_out(envCustomer);
		CRSystem`tok6.setQ_Env_out(envCustomer);
	);
	public test : () ==> TokenDevice
	public run : () ==>  seq of Logger`logType
		);
			i := i - 1;


		envCustomer.showResults();

	);
end World

~~~{% endraw %}
