---
layout: default
title: ElectronicPurse
---

~~~
This example is made by Steve Riddle as an example for an 
#******************************************************
~~~
###Purse.vdmpp

{% raw %}
~~~
class Purse
types
instance variables
private balance: nat;
operations
public IncreaseBal: nat ==> ()
public DecreaseBal: nat ==> ()
public GetBalance:() ==> nat
public GetCardNo: () ==> CardId
public Purse: CardId * nat ==> Purse
functions
~~~{% endraw %}

###System.vdmpp

{% raw %}
~~~
class System
instance variables
private Purses: map Purse`CardId to Purse;
types
public Transaction :: 
operations
public Transfer: Purse`CardId * Purse`CardId * nat ==> ()
public System: set of Purse ==> System
public TotalTransferred:() ==> nat
functions
TotalSum: seq of Transaction -> nat
Len: seq of Transaction -> nat
end System
~~~{% endraw %}
