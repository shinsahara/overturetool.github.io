---
layout: default
title: MondexPP
---

## MondexPP
Author: 




| Properties | Values          |
| :------------ | :---------- |
|Language Version:| classic|
|Entry point     :| new AbPurseFunctional().RunTest()|


### abpursefunctional.vdmpp

{% raw %}
~~~
class AbPurseFunctional

types

 public AbPurse :: balance : nat
                   lost : nat;

 public  PurseId = token;

 public AbWorld ::
   authentic : set of PurseId
   abPurses : map PurseId to AbPurse
 inv mk_AbWorld(authentic, abPurses) ==
  forall name in set dom abPurses &
       name in set authentic;

functions

public GetBalance: AbPurse -> nat
  GetBalance(p) == p.balance;

public GetLost: AbPurse -> nat
  GetLost(p) == p.lost;

public IncreaseBalance: AbPurse * nat -> AbPurse
  IncreaseBalance(p, val) ==
    mk_AbPurse(p.balance + val, p.lost);

public IncreaseLost: AbPurse * nat -> AbPurse
  IncreaseLost(p, val) ==
    mk_AbPurse(p.balance, p.lost + val);

public ReduceBalance: AbPurse * nat -> AbPurse
  ReduceBalance(p, val) ==
    mk_AbPurse(p.balance - val, p.lost)
  pre p.balance >= val;

public GetTotal : AbPurse -> nat
  GetTotal(p) ==
    p.balance + p.lost;

newAbWorld : map PurseId to AbPurse * set of PurseId -> AbWorld
  newAbWorld(purses, auth) ==
    mk_AbWorld(auth, purses)
  pre dom purses subset auth;

public TransferOk: AbWorld * PurseId * PurseId * nat -> AbWorld
  TransferOk(wrld, frm, too, val) ==
  (let
  newFrm =
   ReduceBalance(wrld.abPurses(frm),val),
  newTo =
   IncreaseBalance(wrld.abPurses(too),val)
  in
   mk_AbWorld(wrld.authentic,
     wrld.abPurses ++
       {frm |-> newFrm, too |-> newTo})
   )
  pre frm <> too and
    frm in set dom wrld.abPurses
    and
    too in set dom wrld.abPurses
    and
    (GetBalance(wrld.abPurses(frm)) >= val)
  post (GetTotal(RESULT.abPurses(frm)) +
    GetTotal(RESULT.abPurses(too)) =
	 (GetTotal(wrld.abPurses(frm)) +
       GetTotal(wrld.abPurses(too)))	
    and
	(GetBalance(RESULT.abPurses(frm)) +
      GetBalance(RESULT.abPurses(too))) =
	 (GetBalance(wrld.abPurses(frm)) +
       GetBalance(wrld.abPurses(too)))	
    and
	forall name in
     set (dom RESULT.abPurses) \ {frm, too}
     &
	(GetBalance(wrld.abPurses(name)) =
      GetBalance(RESULT.abPurses(name)))
	and
    (GetLost(wrld.abPurses(name)) =
      GetLost(RESULT.abPurses(name))));

public TransferLost: AbWorld * PurseId * PurseId * nat -> AbWorld
  TransferLost(wrld, frm, -, val) ==
  (
  let
   newFrm =
    ReduceBalance(wrld.abPurses(frm),val)
   in
   mk_AbWorld(wrld.authentic,
    wrld.abPurses ++
     {frm |-> IncreaseLost(newFrm, val)})
   )
  pre frm in set dom wrld.abPurses and
   GetBalance(wrld.abPurses(frm)) >= val
  post GetTotal(RESULT.abPurses(frm)) =
    GetTotal(wrld.abPurses(frm)) and
	 GetBalance(wrld.abPurses(frm)) >=
    GetBalance(RESULT.abPurses(frm))
    and
	forall name in
      set (dom RESULT.abPurses) \ {frm} &
	   GetBalance(wrld.abPurses(name)) =
    GetBalance(RESULT.abPurses(name))
    and
	 GetLost(wrld.abPurses(name)) =
      GetLost(RESULT.abPurses(name));

operations

public RunTest : () ==> AbWorld
RunTest () ==
  (let w : AbWorld = mk_AbWorld({mk_token(1), mk_token(2)}, 
                                {mk_token(1) |-> mk_AbPurse(100,0), mk_token(2) |-> mk_AbPurse(10,0)})
   in
     TransferOk(w, mk_token(1), mk_token(2), 30);
  );

end AbPurseFunctional
~~~
{% endraw %}

