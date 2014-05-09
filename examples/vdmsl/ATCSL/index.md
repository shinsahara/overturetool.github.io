---
layout: default
title: ATC
---

~~~

This example was developed by Natsuki Terada from the Japanese Railways 
Natsuki Terada, Formal Integrity Analysis of Digital ATC Database,
Natsuki Terada, Integrity Analysis of Digital ATC Database with 
Natsuki Terada, Application of Formal Methods to Automatic Train Control 
N. Terada and M. Fukuda, Application of Formal Methods to the Railway 
#******************************************************
~~~
###digitalATCdb.vdmsl

{% raw %}
~~~
--

--
Joint_id = token;
Joint::	position : nat

Insulation = bool; -- true if joint is insulated
Direction = <ADIR> | <BDIR>;
TD :: used : bool  -- true if TD is used
ATBT = <AT> | <BT> | <NULL>;
functions
--
functions
Joint_and_Next_TrackC: TrackC * TrackC * Joint_id -> bool

-- 
Remark_Compatible : Remark * Remark * bool-> bool
ATC_Terminal_and_ATC_Used : ATC * ATC * Remark * ATC * ATC * Remark * bool
pre     Remark_Compatible(rm1, rm2, dir_chng);

Adjacent_TD_Carrier_Differ : TD * ATBT * TD * ATBT * Insulation -> bool
--
Condition ::    kind : Cond_Kind
Cond_Kind = <LIMIT> | <GRADIENT> | <SECTION>;
functions
Overlap : Condition * Condition -> bool

types

Path_map = map Path_id to Path
functions

Not_Start_and_End : Path * Path -> bool
--
Route_map = map Route_id to Route
Route_id = token;
--
Area ::	trackcs : TrackC_map
inv mk_Area(trackcs, paths, routes, -, -) ==
Area_Kind = <PLAIN> | <COMPLEX>;
functions


Direction_Correct : TrackC_map * Path -> bool
Path_Exists : Path_map * seq of Path_id * Direction -> bool
-- next function is related with signal
Route_not_Circular : Path_map * Route-> bool
Path_Connected : Path_map * seq of Path_id * Direction-> bool
pre	Path_Exists(paths, route, dr);
--
--
Del_TrackC : Area * TrackC_id -> Area

Add_Joint : Area * TrackC_id * Joint_id * Joint -> Area
Del_Joint : Area * TrackC_id * Joint_id -> Area
Add_Path : Area * Path_id * Path -> Area

Del_Path : Area * Path_id -> Area
Add_Route : Area * Route_id * Route -> Area
Del_Route : Area * Route_id -> Area
Add_Condition : Area * Path_id * Condition -> Area
post    pid in set dom RESULT.paths and
Del_Condition : Area * Path_id * Cond_Kind * nat * nat -> Area
--

types
Line :: areas : Area_map
        (forall n1, n2 in set c & n1 <> n2 =>
Area_map = map Area_id to Area;
-- (Plan)

--
Connect = set of Area_Joint

Connect_map = map Connect to Remark_Connect

Remark_Connect :: chng_direction : bool
functions

Direction_for_Area_Joint : Path_map * Joint_id * Path_map * Joint_id * bool 

Joint_Compatible: Joint * Joint * Remark_Connect -> bool
Add_Area : Line * Area_id * Area_Kind * MaxSpeed -> Line
Change_Area : Line * Area_id * Area -> Line

Del_Area : Line * Area_id -> Line

Add_Connect : Line * Connect * Remark_Connect -> Line
        (forall n1, n2 in set con & n1 <> n2 =>
post	con in set dom RESULT.connect and
Del_Connect : Line * Area_Joint -> Line
Line_Add_TrackC : Line * Area_id * TrackC_id * TrackC -> Line
Line_Del_TrackC : Line * Area_id * TrackC_id -> Line
Line_Add_Joint : Line * Area_id * TrackC_id * Joint_id * Joint -> Line
Line_Del_Joint : Line * Area_id * TrackC_id * Joint_id -> Line

Line_Add_Path : Line * Area_id * Path_id * Path -> Line
Line_Del_Path : Line * Area_id * Path_id -> Line

Line_Add_Route : Line * Area_id * Route_id * Route -> Line
Line_Del_Route : Line * Area_id * Route_id -> Line
Line_Add_Condition : Line * Area_id * Path_id * Condition-> Line
Line_Del_Condition : Line * Area_id * Path_id * Cond_Kind * nat * nat-> Line
--
functions
        (forall aid in set dom ln.areas & let ar = ln.areas(aid) in
        Following_Path_Exists_at_Connect(ln) and


Joint_Completed : TrackC_map * Area_id * Connect_map -> bool
Path_Exists_for_Joint : TrackC_map * Path_map -> bool
Path_Exists_for_TrackC : TrackC_map * Path_map -> bool
Route_Exists_for_Path : Area -> bool
Path_Exists_before_Start :  Area * Area_id * Connect_map -> bool
Path_Exists_after_End :  Area * Area_id * Connect_map -> bool
StartJoint : Path * Direction -> Joint_id
EndJoint : Path * Direction -> Joint_id

--
                mk_Area_Joint(aid, tcid, jid) in set dunion dom connect or

--
Following_Route_Exists : Route_map * Route_id -> bool
--

--
Is_Plain_Area : Area * Area_id * Connect_map-> bool

--
One_Side_Unique_Path_at_Connection : Line -> bool
                (ln.areas(n1.aid).trackcs(n1.tcid).joints(n1.no).
                 let dr2 = if not ln.connect(con).chng_direction then dr
Following_Path_Exists_at_Connect : Line -> bool
Preceding_Path_Exists_at_Connect : Line -> bool
~~~{% endraw %}
