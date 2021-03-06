---
layout: default
title: raildirSL
---

## raildirSL
Author: Kirsten Mark Hansen



In this specification a model of interlocking systems is presented,
and it is describe how the model may be validated by
simulation. Station topologies are modelled by graphs in which the
nodes denote track segments, and the edges denote connectivity for
train traffic. Points and signals are modelled by annotations on the
edges, thereby restricting the driving possibilities. We define the
safe station states as predicates on the graph, and present a first
step towards an implementation of these predicates. The model
development illustrates how concepts may be captured and validated for
a non-trivial system. This work was conducted by Kirsten Mark Hansen
work for the Danish State Railways. More about this work can be found
at:

K.M. Hansen, Formalising Railway Interlocking Systems, 
Nordic Seminar on Dependable Computing Systems, Department 
of Computer Science, Technical University of Denmark, August 
1994, pp. 83-94.

Kirsten Mark Hansen. Linking Safety Analysis to Safety Requirements
- Exemplified by Railway Interlocking Systems. PhD thesis, 
Department of Information Technology, Technical University of 
Denmark, 1996. 


| Properties | Values          |
| :------------ | :---------- |
|Language Version:| classic|


### rail.vdmsl

{% raw %}
~~~
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     
module station

exports
  types struct Station_topo;
        struct Tracks;
        struct Track_id ;
        struct Points;
        struct Point_state;
        struct Crossings;
        struct Signals;
        struct Signal_id;
        struct Station_state;
        struct Track_states;
        struct Point_states;
        struct Signal_states;
        struct Track_state;
        struct Train_id;
        struct Train_type;
        struct Point_control;
        struct Operation;
        struct Signal_state

  functions
        Is_wf_Station_topo  : Station_topo +> bool ;
        Is_wf_Station_state : Station_topo * Station_state +> bool

definitions


types
        Tracks = set of (Track_id * Track_id);
        Track_id  =  seq of char

                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          
functions

    NoReflexive : Tracks +> bool
    NoReflexive (tracks) ==
      forall mk_(t1,t2) in set tracks & t1 <> t2;
                                                                                                                                                                                                                     
  Symmetric : Tracks +> bool
  Symmetric (tracks) ==
     forall mk_(t1,t2) in set tracks & mk_(t2,t1) in set tracks
                                                                                          
functions

    Is_wf_Tracks : Tracks +> bool
    Is_wf_Tracks (tracks)  ==
          NoReflexive (tracks) and
          Symmetric (tracks)
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             
types
     Points      = map (Track_id * Track_id) to 
                       (map Track_id to Point_control);
     Point_control = <left> | <right>;
     Crossings   =  set of (Track_id * Track_id)

                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   
functions

   PointsInTracks : Tracks * Points +> bool

   PointsInTracks (tracks,points) ==
    dom points subset tracks;

                                                                                                                                                                                                                                                                                          
   Inverses : Points +> bool

   Inverses (points) ==
     forall mk_(t1,t2) in set dom points &
      mk_(t2,t1) in set dom points and
      points (mk_(t1,t2)) = points (mk_(t2,t1));
                                                                                                                                                                                                                                                                                                        
   OkDomRng: Points +> bool

   OkDomRng (points) ==
     forall mk_(t1,t2) in set dom points &
      points (mk_(t1,t2)) <> {|->} and
      dom (points (mk_(t1,t2))) subset {t1,t2};

                                                                                                                                                                                                                                                                                                                                                             
   RelateToTracks: Tracks * Points +> bool

   RelateToTracks (tracks,points) ==
    (forall tpair in set dom points &
     forall pid in set dom points (tpair) &
      card {t | mk_(t,t') in set tracks & t' = pid } = 3) and
    (forall mk_(t1,-) in set tracks &
     (not exists tpair in set dom points & 
             t1 in set dom points (tpair)) =>
      card {t|mk_(t,t') in set tracks & t'=t1} <= 2 );
                                                                                                                                                                                                                                                                                                                        
   Control2Dir: Points +> bool

   Control2Dir (points) ==
     forall mk_(t1,t2) in set dom points &
      ({t1} <: points (mk_(t1,t2)) = {t1 |->  <left>} =>
           exists mk_(t,t') in set dom points &
             t = t1 and
            {t1} <: points (mk_(t1,t')) = {t1 |->  <right>}) and
      ({t1} <: points (mk_(t1,t2)) = {t1 |->  <right>} =>
           exists mk_(t,t') in set dom points &
             t = t1 and
            {t1} <: points (mk_(t1,t')) = {t1 |->  <left>});
                                                                                                    
   Is_wf_Points: Tracks * Points +> bool

   Is_wf_Points (tracks,points) ==
     PointsInTracks (tracks,points) and
     Inverses (points)              and
     OkDomRng (points)              and
     RelateToTracks (tracks,points) and
     Control2Dir (points);

                                                                                                                                                                                                                                                        
   SeperateBranches: Tracks * Crossings +> bool

   SeperateBranches (tracks,crossings) ==
     crossings inter tracks = {};

                                                                                                                          
   CrossInTracks: Tracks * Crossings +> bool

   CrossInTracks (tracks,crossings) ==
     forall mk_(t1,t2) in set crossings &
       (exists mk_(t3,-) in set tracks & t1 = t3) and
       (exists mk_(t4,-) in set tracks & t2 = t4);

                                                                                                 
   UniqueCross: Crossings +> bool

   UniqueCross (crossings) ==
     forall mk_(t1,t2) in set crossings &
       t1 <> t2 and
      forall mk_(t3,t4) in set crossings \ {mk_(t1,t2)} &
       (t1 <> t3 and t1 <> t4 and t2 <> t3 and t2 <> t4);

                                                                                                                                                 
   DiffPointsCrossings: Points * Crossings +> bool

   DiffPointsCrossings (points,crossings) ==
     forall mk_(t1,t2) in set crossings &
       forall pointcontrol in set rng points &
         t1 not in set dom pointcontrol and
         t2 not in set dom pointcontrol;
                                                                                                        
   Is_wf_Crossings: Tracks * Points * Crossings +> bool

   Is_wf_Crossings (tracks,points,crossings) ==
     SeperateBranches (tracks,crossings)    and
     CrossInTracks (tracks,crossings)       and
     UniqueCross (crossings)                and
     DiffPointsCrossings (points,crossings)
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
types
     Signals   = map (Track_id * Track_id) to Signal_id;
     Signal_id = seq of char
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              
functions

   UniqueSignals: Signals +> bool

   UniqueSignals (signals) ==
     card rng signals = card dom signals;

                                                                                                                                                                                                                                                                                                                                                       
   SigInTracks: Tracks * Signals +> bool

   SigInTracks (tracks,signals) ==
     forall mk_(t1,t2) in set dom signals &
      (t1 = "ol" => card { t | mk_(t,t') in set tracks 
                             & t' = t2 } = 1 ) and
      (t2 = "ol" => card { t | mk_(t,t') in set tracks 
                             & t' = t1 } = 1 ) and
      (t1 <> "ol" and t2 <> "ol" => mk_(t1,t2) in set tracks );
                                                                                                                   
   Is_wf_Signals: Tracks * Signals +> bool

   Is_wf_Signals (tracks,signals) ==
     UniqueSignals (signals) and
     SigInTracks (tracks,signals)

                                                                                                                                                                              
types
     Station_topo :: tracks    : Tracks
                points    : Points
                crossings : Crossings
                signals   : Signals
                                                                                                                                      
functions
   Is_wf_Station_topo: Station_topo +> bool

   Is_wf_Station_topo (stationtopo) ==
     Is_wf_Tracks (stationtopo.tracks) and
     Is_wf_Points (stationtopo.tracks,stationtopo.points) and
     Is_wf_Crossings (stationtopo.tracks,stationtopo.points,
                      stationtopo.crossings) and
     Is_wf_Signals (stationtopo.tracks,stationtopo.signals)
                                                                                                                                                                                                                                                                                                                                                                               

types
     Station_state :: trackstates  : Track_states
                      pointstates  : Point_states
                      signalstates : Signal_states;
                                                                                                                                                                                                                                                                                                        
     Track_states  = map Track_id  to Track_state;
     Point_states  = map Track_id  to Point_state;
     Signal_states = map Signal_id to Signal_state;
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                
   Track_state   =  map Train_id to Train_type;
   Train_id      = seq of char;
   Train_type    =  <fixedroute> | <autonomous> ;
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
   Point_state    =  Point_control * Operation ;
   Operation      =  <manual> | <interlock>;
                                                                                                                                                                                                                       
      Signal_state = <stop> | <driveaspect>
                                                                                                                                                                                                                                                                                                                                               
functions

   ConformTracks: Tracks * Track_states  +> bool

   ConformTracks (tracks,trackstates)  ==
     TracksStateInTopo (tracks,trackstates) and
     TracksTopoInState (tracks,trackstates);

                            
   TracksStateInTopo: Tracks * Track_states  +> bool

   TracksStateInTopo (tracks,trackstates)  ==
     forall t in set dom trackstates &
       exists mk_(t1,t2) in set tracks & t = t1 or t = t2;

                            
   TracksTopoInState: Tracks * Track_states  +> bool

   TracksTopoInState (tracks,trackstates)  ==
     forall mk_(t1,t2) in set tracks &
       t1 in set dom trackstates and
       t2 in set dom trackstates;

                                                                                                                                                                                                                                                    
   ConformPoints: Points * Point_states  +> bool

   ConformPoints (points,pointstates)  ==
     PointsStateInTopo (points,pointstates) and
     PointsTopoInState (points,pointstates);

                            
   PointsStateInTopo: Points * Point_states  +> bool

   PointsStateInTopo (points,pointstates)  ==
     forall t in set dom pointstates &
      exists mk_(t1,t2) in set dom points  &
       t in set dom points (mk_(t1,t2));
                            
   PointsTopoInState: Points * Point_states  +> bool

   PointsTopoInState (points,pointstates)  ==
     forall mk_(t1,t2) in set dom points &
      (t1 in set dom points (mk_(t1,t2)) => 
       t1 in set dom pointstates) and
      (t2 in set dom points (mk_(t1,t2)) => 
       t2 in set dom pointstates);
                                                                                                                                                                                                                      
   ConformSignals: Signals * Signal_states  +> bool

   ConformSignals (signals,signalstates)  ==
     rng signals = dom signalstates;

                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  
   Is_wf_Trains: Tracks * Points * Crossings * Track_states +> 
                 bool

   Is_wf_Trains (tracks,points,crossings,trackstates)  ==
     UniqueTrain (trackstates)    and
     let trains = Trains (trackstates,{|->}) in
         Connected (trains,tracks)     and
         OkPointTrains (trains,points) and
         OkCrossTrains (trains,crossings);

                                                                                                                                                                              
   UniqueTrain : Track_states +> bool

   UniqueTrain (trackstates) ==
     forall trackid in set dom trackstates &
      forall trainid in set dom trackstates (trackid) &
       (trackstates (trackid)(trainid) = <fixedroute> =>
        (forall trackid' in set dom trackstates &
        (trainid in set dom trackstates (trackid') =>
         trackstates (trackid')(trainid) = <fixedroute>))) and
       (trackstates (trackid)(trainid) = <autonomous> =>
        (forall trackid' in set dom trackstates &
        (trainid in set dom trackstates (trackid') =>
         trackstates (trackid')(trainid) = <autonomous>)));
                                                                                                                                                       
   Trains: Track_states * (map Train_id to set of Track_id) +> 
           (map Train_id to set of Track_id)

   Trains (trackstates,sorted)  ==
     if   trackstates = {|->}
     then sorted
     else let t in set dom trackstates in
              if   trackstates (t) = {|->}
              then Trains ({t} <-: trackstates,sorted)
              else Trains ({t} <-: trackstates, 
                           Update (mk_(t,trackstates(t)),sorted));
                                                                                                                                                                                                                                                                                                
   Update: (Track_id * Track_state) * 
           (map Train_id to set of Track_id) +> 
           (map Train_id to set of Track_id)

   Update (mk_(t,trackstate),sorted)  ==
     if   trackstate = {|->}
     then sorted
     else let tid in set dom trackstate in
              if   tid in set dom sorted
              then Update (mk_(t,{tid} <-: trackstate), 
                               sorted ++ 
                               { tid |-> {t} union sorted (tid)} )
              else Update (mk_(t,{tid} <-: trackstate), 
                               sorted munion { tid |-> {t} } );
                                                                                                                             
   Connected: (map Train_id to set of Track_id) * Tracks +> bool

   Connected (trains,tracks)  ==
     forall trackset in set rng trains &
      ExistsPath (trackset,tracks);

                                                                                                                                                                  
   ExistsPath: set of Track_id * Tracks +> bool

   ExistsPath (trackset,tracks)  ==
     exists t in set trackset &
         Path (trackset \ {t},[t],tracks,{});

                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             
   Path: set of Track_id * seq of Track_id * Tracks * 
         set of Track_id +> bool

   Path (trackset,connected,tracks,tried)  ==
     if   trackset = {}
     then tried = {}
     else let t in set trackset in
              if   mk_(t,hd connected) in set tracks
              then Path ((trackset \ {t}) union tried,
                          [t]^connected,tracks,{})
              else if mk_(connected (len connected),t) in set 
                      tracks
                   then Path ((trackset \ {t}) union 
                               tried,connected^[t],tracks,{})
                   else Path ((trackset \ {t}),connected,tracks,
                               tried union {t});
                                                                                                                                                                 
   OkPointTrains: (map Train_id to set of Track_id) * Points +> 
                  bool

   OkPointTrains (trains,points)  ==
     forall trackset in set rng trains &
      forall t1,t2 in set trackset &
         (mk_(t1,t2) in set dom points and
          t1 in set dom points (mk_(t1,t2))) =>
          (forall t3 in set trackset &
            mk_(t1,t3) in set dom points and
            t1 in set dom points (mk_(t1,t3)) =>
              points (mk_(t1,t2))(t1) = points (mk_(t1,t3))(t1) );
                                                                                                                                                                      
   OkCrossTrains: (map Track_id to set of Track_id) * Crossings +> 
                  bool

   OkCrossTrains (trains,crossings)  ==
     forall trackidset in set rng trains &
      forall t1,t2 in set trackidset &
        mk_(t1,t2) not in set crossings;
                                                                                                                                          
Is_wf_Station_state :  Station_topo * Station_state +> bool

Is_wf_Station_state (stationtopo,stationstate) ==
  ConformTracks (stationtopo.tracks,stationstate.trackstates)    and
  ConformPoints (stationtopo.points,stationstate.pointstates)    and
  ConformSignals (stationtopo.signals,stationstate.signalstates) and
  Is_wf_Trains (stationtopo.tracks,stationtopo.points,
                stationtopo.crossings,stationstate.trackstates)


end station
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           
module safe_req

imports
 from station
  types Station_topo :: tracks : station`Tracks
                   points : station`Points
                   crossings : station`Crossings
                   signals : station`Signals renamed Station_topo;
        Tracks = set of (station`Track_id * station`Track_id) 
        renamed Tracks; 
        Track_id = seq of char renamed Track_id;
        Points = map (station`Track_id * station`Track_id) to
	         (map station`Track_id to station`Point_control)
                 renamed Points;
        Point_control = <left> | <right> renamed Point_control;
        Crossings = set of (station`Track_id * station`Track_id) 
                    renamed Crossings;
        Signals  = map (station`Track_id * station`Track_id) 
                   to station`Signal_id
                   renamed Signals;
        Signal_id = seq of char
                    renamed Signal_id;  

        Station_state :: trackstates  : station`Track_states
                         pointstates  : station`Point_states
                         signalstates : station`Signal_states
                         renamed Station_state;
        Track_states  = map station`Track_id  to station`Track_state
                        renamed Track_states;
        Track_state   =  map station`Train_id to station`Train_type
                         renamed Track_state;
        Train_id      =  seq of char
                         renamed Train_id;
        Train_type    = <fixedroute> | <autonomous> 
                        renamed Train_type;
        Point_states  = map station`Track_id  to station`Point_state
                        renamed Point_states;
        Point_state   = station`Point_control * station`Operation
                        renamed Point_state;
        Operation     = <manual> | <interlock> 
                        renamed Operation; 
        Signal_states = map station`Signal_id to station`Signal_state 
                        renamed Signal_states;
        Signal_state  = <stop> | <driveaspect> renamed Signal_state

  functions
        Is_wf_Station_topo  : station`Station_topo +> bool 
                              renamed Is_wf_Station_topo;
        Is_wf_Station_state : station`Station_topo * 
                              station`Station_state +> 
                              bool
                              renamed Is_wf_Station_state

exports
   functions SafeReq : Station_topo * Station_state +> bool


definitions

                                                                                                                                                 
functions
   SafeReq: Station_topo * Station_state +> bool

   SafeReq (stationtopo,stationstate)  ==
     NoCollision (stationtopo,stationstate) and
     NoDerail (stationtopo,stationstate)

  pre Is_wf_Station_topo (stationtopo) and 
      Is_wf_Station_state (stationtopo,stationstate); 
                                                                                                                                                                                                                                                                                   
   NoCollision: Station_topo * Station_state +> bool

   NoCollision (stationtopo,stationstate)  ==
     forall tid in set dom stationstate.trackstates &
      card dom (stationstate.trackstates(tid)) <= 1 and
      OneTrainAtCross (stationtopo.crossings,stationstate.trackstates);
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           
   OneTrainAtCross: Crossings * Track_states +> bool

   OneTrainAtCross (crossings,trackstates)  ==
     forall mk_(tid1,tid2) in set crossings &
      card dom (trackstates(tid1)) + card dom (trackstates(tid2)) <= 1;
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             
   NoDerail: Station_topo * Station_state +> bool

   NoDerail (stationtopo,stationstate)  ==
     let trains = Trains (stationstate.trackstates,{|->}) in
         forall t in set dom trains &
          forall t1,t2 in set trains (t) &
           mk_(t1,t2) in set dom stationtopo.points =>
           Ok_Point_states (mk_(t1,t2),stationtopo.points,
                            stationstate.pointstates);
                                                                                                                                                                      
   Trains: Track_states * (map Train_id to set of Track_id) +> 
           (map Train_id to set of Track_id)
   Trains (trackstates,sorted)  ==
     if   trackstates = {|->}
     then sorted
     else let t in set dom trackstates in
              if   trackstates (t) = {|->}
              then Trains ({t} <-: trackstates,sorted)
              else Trains ({t} <-: trackstates, 
                           Update (mk_(t,trackstates(t)),sorted));
                            
   Update: (Track_id * Track_state) * 
           (map Train_id to set of Track_id) +> 
   (map Train_id to set of Track_id)

   Update (mk_(t,trainids),sorted)  ==
     if   trainids = {|->}
     then sorted
     else let tid in set dom trainids in
              if   tid in set dom sorted
              then Update (mk_(t,{tid} <-: trainids), 
                           sorted ++ 
                           { tid |-> {t} union sorted (tid)} )
              else Update (mk_(t,{tid} <-: trainids), 
                           sorted ++ { tid |-> {t} });
                                                                                                                                                                                                                                                                            
   Ok_Point_states: (Track_id * Track_id) * Points * Point_states +> 
                    bool

   Ok_Point_states (mk_(t1,t2),points,pointstates) ==
     if   {t1,t2} = dom points (mk_(t1,t2))
     then Point_state_ok (mk_(t1,t2),t1,points,pointstates) and
          Point_state_ok (mk_(t1,t2),t2,points,pointstates)
     else if   {t1} = dom points (mk_(t1,t2))
          then Point_state_ok (mk_(t1,t2),t1,points,pointstates)
          else Point_state_ok (mk_(t1,t2),t2,points,pointstates)
               
   pre mk_(t1,t2) in set dom points;
                                                                                                                                                                                                                                                                       
   Point_state_ok: (Track_id * Track_id) * Track_id * Points * 
                   Point_states +> bool

   Point_state_ok (tpair,t,points,pointstates) ==
     let mk_(pcnt,-) = pointstates (t) in
         (points (tpair)) (t) = pcnt

   pre t in set dom pointstates and tpair in set dom points and 
       t in set dom points(tpair)

end safe_req
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             
module impl

imports
 from station
  types Station_topo :: tracks : station`Tracks
                   points : station`Points
                   crossings : station`Crossings
                   signals : station`Signals renamed Station_topo;
        Tracks = set of (station`Track_id * station`Track_id) 
                 renamed Tracks;
        Track_id = seq of char renamed Track_id;
        Points = map (station`Track_id * station`Track_id) to
	         (map station`Track_id to station`Point_control)
                 renamed Points;
        Point_control = <left> | <right>
                        renamed Point_control;
        Crossings = set of (station`Track_id * station`Track_id) 
        renamed Crossings;
        Signals = map (station`Track_id * station`Track_id) 
                  to station`Signal_id
                  renamed Signals;
        Signal_id = seq of char
                    renamed Signal_id; 

   Station_state :: trackstates : station`Track_states
                     pointstates : station`Point_states
                     signalstates: station`Signal_states
                    renamed Station_state;

   Track_states  =  map station`Track_id to station`Track_state
                    renamed Track_states;
   Track_state   =  map station`Train_id to station`Train_type
                    renamed Track_state;
   Train_id      = seq of char
                   renamed Train_id;
   Train_type    =  <fixedroute> | <autonomous>
                    renamed Train_type;

   Point_states   =  map station`Track_id to station`Point_state
                     renamed Point_states;
   Point_state    =  station`Point_control * station`Operation
                     renamed Point_state;
   Operation      =  <manual> | <interlock>
                     renamed Operation;

      Signal_states = map station`Signal_id to station`Signal_state
                      renamed Signal_states;
      Signal_state = <stop> | <driveaspect>
                     renamed Signal_state

  functions
        Is_wf_Station_topo  : station`Station_topo +> bool 
                              renamed Is_wf_Station_topo;
        Is_wf_Station_state : station`Station_topo * 
                              station`Station_state +> 
                              bool
                              renamed Is_wf_Station_state

exports
  functions Impl : Station_topo * Station_state +> bool

definitions




types
     Areas = map Train_id to Area;
     Area = set of Track_id
                                                                                                                                                                                                                                                                       
functions
   Impl: Station_topo * Station_state +> bool

   Impl (stationtopo,stationstate) ==
     let areas = FindAreas (stationtopo,stationstate) in
         NoCollision (areas,stationtopo.crossings) and
         NoDerail (areas,stationtopo,stationstate)

   pre Is_wf_Station_topo (stationtopo) and 
        Is_wf_Station_state (stationtopo,stationstate);


                                                                                                                                                                          
   NoCollision: Areas * Crossings +> bool

   NoCollision (areas,crossings) ==
     forall train1, train2 in set dom areas &
      train1 <> train2 =>
       (areas (train1) inter areas (train2) = {} and
                             forall t1 in set areas (train1) &
                              (forall t2 in set areas (train2) &
                                mk_(t1,t2) not in set crossings));
                                                                                                                                                                                                                                                                                                        
   NoDerail: Areas * Station_topo * Station_state +> bool

   NoDerail (areas,stationtopo,stationstate) ==
     NoDerailUnderTrains (stationtopo,stationstate) and 
     NoDerailPossible (areas,stationtopo,stationstate); 
                                                                                                                                    
   NoDerailUnderTrains: Station_topo * Station_state +> bool 

   NoDerailUnderTrains (stationtopo,stationstate) == 
     let trains = Trains (stationstate.trackstates,{|->}) in
         forall t in set dom trains &
          forall t1,t2 in set trains (t) &
           mk_(t1,t2) in set dom stationtopo.points =>
           Ok_Point_states (mk_(t1,t2),stationtopo.points,
                            stationstate.pointstates);
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       
  NoDerailPossible : Areas * Station_topo * Station_state +> bool 

  NoDerailPossible (areas,stationtopo,stationstate) ==
      forall area in set rng areas &
      let traintype = TrainType (area,stationstate) in
          if   traintype = <fixedroute>
          then NoDerailFixed (area,stationtopo,stationstate)
          else NoDerailAutonomous (area,stationtopo,stationstate);
                                                                                                                                              
   Trains: Track_states * (map Train_id to set of Track_id) +> 
           (map Train_id to set of Track_id)

   Trains (trackstates,sorted)  ==
     if   trackstates = {|->}
     then sorted
     else let t in set dom trackstates in
              if   trackstates (t) = {|->}
              then Trains ({t} <-: trackstates,sorted)
              else Trains ({t} <-: trackstates, 
                           Update (mk_(t,trackstates(t)),sorted));

   Update: (Track_id * Track_state) * 
           (map Train_id to set of Track_id) +> 
           (map Train_id to set of Track_id)

   Update (mk_(t,trainids),sorted)  ==
     if   trainids = {|->}
     then sorted
     else let tid in set dom trainids in
              if   tid in set dom sorted
              then Update (mk_(t,{tid} <-: trainids), 
                               sorted ++ 
                               { tid |-> {t} union sorted (tid)} )
              else Update (mk_(t,{tid} <-: trainids), 
                               sorted ++ { tid |-> {t} });
                                                                                                                                                                                
   TrainType : Area * Station_state +> Train_type

   TrainType (area,stationstate) ==
      let t in set area be st stationstate.trackstates (t) <> {|->} in
          let tt in set dom stationstate.trackstates (t) in
              stationstate.trackstates (t) (tt)

   pre forall t in set area & t in set dom stationstate.trackstates ;
                                                                                                                                                                                                                                                
   NoDerailFixed : Area * Station_topo * Station_state +> bool

   NoDerailFixed (area,stationtopo,stationstate) ==
     forall t1,t2 in set area &
      mk_(t1,t2) in set dom stationtopo.points => 
      Ok_Point_states  (mk_(t1,t2),stationtopo.points,
                        stationstate.pointstates) and
      InterlockPoints (mk_(t1,t2),stationtopo.points,
                       stationstate.pointstates);
                                                                                                                                                                               
   Ok_Point_states: (Track_id * Track_id) * Points * Point_states +> 
                    bool

   Ok_Point_states (mk_(t1,t2),points,pointstates) ==
     if   {t1,t2} = dom points (mk_(t1,t2))
     then Point_state_ok (mk_(t1,t2),t1,points,pointstates) and
          Point_state_ok (mk_(t1,t2),t2,points,pointstates)
     else if   {t1} = dom points (mk_(t1,t2))
          then Point_state_ok (mk_(t1,t2),t1,points,pointstates)
          else Point_state_ok (mk_(t1,t2),t2,points,pointstates)
               

   pre mk_(t1,t2) in set dom points;


   Point_state_ok: (Track_id * Track_id) * Track_id * Points * 
                   Point_states +> bool

   Point_state_ok (tpair,t,points,pointstates) ==
     let mk_(pcnt,-) = pointstates (t) in
         (points (tpair)) (t) = pcnt

   pre t in set dom pointstates and tpair in set dom points and 
       t in set dom points(tpair); 
                                                                                                                                                                                                                                                                 
   InterlockPoints: (Track_id * Track_id) * Points * Point_states +> 
                    bool

   InterlockPoints (mk_(t1,t2),points,pointstates) ==
     if   IsPoint (t1,points) and IsPoint (t2,points)
     then IsInterlockPoint (t1,points,pointstates) and
          IsInterlockPoint (t2,points,pointstates)
     else if   IsPoint (t1,points)
          then IsInterlockPoint (t1,points,pointstates)
          else IsInterlockPoint (t2,points,pointstates)

pre IsPoint (t1,points) or IsPoint (t2,points);
                                                                                               
   IsPoint: Track_id * Points +> bool

   IsPoint (t,points) ==
     exists mk_(t1,t2) in set dom points &
      t in set dom points (mk_(t1,t2));
                                                                                                                                                        
   IsInterlockPoint: Track_id * Points * Point_states +> bool

   IsInterlockPoint (t,points,pointstates) ==
     if   IsPoint (t,points)
     then let mk_(-,operation) = pointstates (t) in
              operation = <interlock>
     else false;
                                                                                                                                                                                                                                                                          
   NoDerailAutonomous: Area * Station_topo * Station_state +> bool

   NoDerailAutonomous (area,stationtopo,stationstate) ==
     forall t1,t2 in set area &
      mk_(t1,t2) in set dom stationtopo.points =>  
      OkAutonomPoint_states (mk_(t1,t2),stationtopo.points,
                             stationstate.pointstates);


   OkAutonomPoint_states: (Track_id * Track_id) * Points * 
                          Point_states +> bool

   OkAutonomPoint_states (mk_(t1,t2),points,pointstates) ==
     if   IsInterlockPoint (t1,points,pointstates)  and
          IsInterlockPoint (t2,points,pointstates)
     then Point_state_ok (mk_(t1,t2),t1,points,pointstates) and
          Point_state_ok (mk_(t1,t2),t2,points,pointstates)
     else if   IsInterlockPoint (t1,points,pointstates)
          then Point_state_ok (mk_(t1,t2),t1,points,pointstates)
          else if IsInterlockPoint (t2,points,pointstates)
               then Point_state_ok (mk_(t1,t2),t2,points,pointstates)
               else true; 
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       
   FindAreas : Station_topo * Station_state +> Areas

   FindAreas (stationtopo,stationstate) ==
     let occupied = Occupied (stationstate.trackstates,{|->}) in
         DeduceAreas (occupied,stationtopo,stationstate);
                                                                                                                      
   Occupied: Track_states * (map Train_id to set of Track_id) +> 
             map Train_id to set of Track_id
   Occupied (trackstates,occupied) ==
     if   forall s in set rng trackstates & s = {|->}
     then occupied
     else let trains in set rng trackstates be st trains <> {|->} in
              let train in set dom trains in
                  Occupied (RemoveTrain (train,trackstates),
                            occupied munion 
                            {train |-> {tid 
                                       | tid in set dom trackstates 
                                        & train in set 
                                          dom trackstates (tid)} } )

   pre forall trackstate in set rng trackstates & 
       dom trackstate inter dom occupied = {};

                                                                                                                                                                              
   RemoveTrain : Train_id * Track_states +> Track_states

   RemoveTrain (train,trackstates) ==
     if   trackstates = {|->}
     then {|->}
     else let tid in set dom trackstates in
              if   train in set dom trackstates (tid)
              then RemoveTrain (train,trackstates ++ 
                                      {tid |-> ({train} <-: 
                                      trackstates (tid))})
              else {tid |-> trackstates (tid)} munion 
                   RemoveTrain (train,{tid} <-: trackstates);

                                                                                                       
   DeduceAreas: (map Train_id to set of Track_id) * 
                Station_topo * Station_state +> Areas
   DeduceAreas (occupied,stationtopo,stationstate) ==
     if   occupied = {|->}
     then {|->}
     else let train in set dom occupied in
          let area = FindArea (occupied (train),{},
                               stationtopo,stationstate) in
             {train |-> area} munion
             DeduceAreas ({train} <-: occupied, 
                          stationtopo,stationstate);
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    
   FindArea : set of Track_id * Area * Station_topo * Station_state +> 
              Area

  FindArea (tracks,area,stationtopo,stationstate) ==
    if   tracks = {}
    then area
    else let t in set tracks in
             let neighbours = Neighbours (t,area,stationtopo,
                                          stationstate) in
                 FindArea ((tracks\ {t}) union neighbours, 
                           area union {t},
                           stationtopo,stationstate);

                                                                                                                                                                                                                                                                                           
   Neighbours : Track_id * set of Track_id * Station_topo * 
                Station_state +> 
                set of Track_id

   Neighbours (t,area,stationtopo,stationstate) ==
     let neighcand = NeighbourCandidates (t,area,stationtopo.tracks) in
         {t' | t' in set neighcand 
             & OkEdge (mk_(t,t'),stationtopo,stationstate)};
                                                                                                                                                                                                                              
   NeighbourCandidates: Track_id * set of Track_id * Tracks +> 
                        set of Track_id

   NeighbourCandidates (t,area,tracks) ==
     {t'| mk_(t1,t') in set tracks & t = t1 and t' not in set area };
                                                                                                                                                                                            
   OkEdge: (Track_id * Track_id) * Station_topo * Station_state +> bool

   OkEdge (tpair,stationtopo,stationstate) ==
     OkPoints (tpair,stationtopo.points,stationstate.pointstates) and
     not (StopSignal (tpair,stationtopo.signals,
                      stationstate.signalstates));
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     
   OkPoints: (Track_id * Track_id) * Points * Point_states +> bool

   OkPoints (mk_(t,t'),points,pointstates) ==
     IsInterlockPoint (t,points,pointstates) and
     Branch (mk_(t,t'),points) =>
     Point_state_ok (mk_(t,t'),t,points,pointstates);
                                                                                                                                                                                              
   StopSignal: (Track_id * Track_id) * Signals * Signal_states +> bool

   StopSignal (tpair,signals,signalstates) ==
     if   tpair in set dom signals
     then let sigid = signals (tpair) in
              signalstates (sigid) = <stop>
     else false;
                                                                                                                                                                                                                 
   Branch : (Track_id * Track_id) * Points +> bool

   Branch (mk_(t1,t2),points) ==
     mk_(t1,t2) in set dom points and
     t1 in set dom points (mk_(t1,t2))

end impl 
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          
module Test

exports all

definitions
values


tracksv = { mk_("K12","26"), mk_("26","K12"),
            mk_("25","26"),  mk_("26","25"),
            mk_("25","24"),  mk_("25","17s"),
            mk_("24","25"),  mk_("24","23"),
            mk_("24","15"),  mk_("23","24"),
            mk_("23","22"),  mk_("22","23"),
            mk_("22","21"),  mk_("21","22"),
            mk_("21","A12"), mk_("A12","21"),
            mk_("M12","18"), mk_("18","M12"),
            mk_("18","17b"), mk_("17b","18"),
            mk_("17s","25"), mk_("17b","16"),
            mk_("17s","27"), mk_("16","17b"),
            mk_("16","19"),  mk_("16","15"),
            mk_("15","24"),  mk_("15","16"),
            mk_("15","14"),  mk_("14","15"),
            mk_("14","13"),  mk_("13","14"),
            mk_("13","11"),  mk_("11","13"),
            mk_("11","C12"), mk_("C12","11"),
            mk_("27","17s"), mk_("27","O12"),
            mk_("O12","27"), mk_("19","16"),
            mk_("19","Q12"), mk_("Q12","19") }; 

pointsv = { mk_("25","26")  |-> { "25" |-> <right> },
            mk_("26","25")  |-> { "25" |-> <right> },
            mk_("25","17s") |-> { "25" |-> <left> },
            mk_("17s","25") |-> { "25" |-> <left> },
            mk_("24","23")  |-> { "24" |-> <left> },
            mk_("23","24")  |-> { "24" |-> <left> },
            mk_("24","15")  |-> { "24" |-> <right> ,
                                  "15" |-> <right> },
            mk_("15","24")  |-> { "24" |-> <right> ,
                                  "15" |-> <right> },
            mk_("16","17b") |-> { "16" |-> <right> },
            mk_("17b","16") |-> { "16" |-> <right> },
            mk_("16","19")  |-> { "16" |-> <left> },
            mk_("19","16")  |-> { "16" |-> <left> },
            mk_("16","15")  |-> { "15" |-> <left> },
            mk_("15","16")  |-> { "15" |-> <left> } };

crossingsv = { mk_("17s", "17b")};

signalsv = { mk_("ol","K12") |-> "N",
             mk_("26","K12") |-> "K",
             mk_("22","23")  |-> "E2",
             mk_("21","A12") |-> "D",
             mk_("ol","A12") |-> "A",
             mk_("ol","M12") |-> "M",
             mk_("18","M12") |-> "L",
             mk_("13","14")  |-> "E1",
             mk_("11","C12") |-> "C",
             mk_("ol","C12") |-> "B",
             mk_("27","O12") |-> "O",
             mk_("ol","O12") |-> "R",
             mk_("ol","Q12") |-> "Q",
             mk_("19","Q12") |-> "P" };


trackstatev = { "K12" |-> {|->},
                "26"  |-> {|->},
                "25"  |-> {|->},
                "24"  |-> {|->},
                "23"  |-> {|->},
                "22"  |-> {|->},
                "21"  |-> {|->},
                "A12" |-> {|->},
                "M12" |-> {|->},
                "18"  |-> {"t2" |-> <fixedroute>},
                "17s" |-> {|-> },
                "17b" |-> {|->},
                "16"  |-> {|->},
                "15"  |-> {|->},
                "14"  |-> {|->},
                "13"  |-> {|->},
                "11"  |-> {|->},
                "C12" |-> {|->},
                "O12" |-> {|->},
                "Q12" |-> {|->},
                "19"  |-> {|->},
                "27"  |-> {|->} };

pointstatev = { "25" |-> mk_(<left>,<interlock>),
                "24" |-> mk_(<right>,<interlock>),
                "15" |-> mk_(<left>,<interlock>),
                "16" |-> mk_(<right>,<interlock>) };

signalstatev = { "N"  |-> <stop>,
                 "K"  |-> <stop>,
                 "E2" |-> <stop>,
                 "D"  |-> <stop>,
                 "A"  |-> <stop>,
                 "M"  |-> <stop>,
                 "L"  |-> <stop>,
                 "E1" |-> <stop>,
                 "C"  |-> <stop>,
                 "B"  |-> <stop>, 
                 "O"  |-> <stop>,
                 "R"  |-> <stop>,
                 "P"  |-> <driveaspect>,
                 "Q"  |-> <stop> }
end Test
             
~~~
{% endraw %}

