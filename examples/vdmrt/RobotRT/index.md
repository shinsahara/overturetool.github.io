---
layout: default
title: Robot
---

~~~
This example was produced by Lasse Lorentzen and Kenneth Lausdahl
~~~
###DataReader.vdmrt

{% raw %}
~~~
class DataReader
operations
 public Read : () ==> ()
end DataReader
class DataReaderTest is subclass of TestCase
operations
 protected RunTest: () ==> ()
  protected TearDown: () ==> ()
end DataReaderTest
~~~{% endraw %}

###Enviroment.vdmrt

{% raw %}
~~~
class Enviroment
instance variables
 public GetPointAvalibility: Grid`Point ==> Grid`PointAvalibility
public handleEvent : Grid * seq of SteeringController`Route * 
public isFinished : () ==> ()
thread
   completeGrid := file.Load("testmap.map");
   busy := false;
 )
sync
per isFinished => not busy;

end Enviroment

class EnviromentTest is subclass of TestCase
operations
 protected RunTest: () ==> ()
  protected TearDown: () ==> ()
end EnviromentTest
~~~{% endraw %}

###Grid.vdmrt

{% raw %}
~~~
class Grid
types
instance variables
private maxPoint: Point := mk_Point(10E6,10E6);
inv forall p in set dom points & IsValidGridPoint(p) 
operations
 public Grid : Point * Point ==> Grid
 public GetPointAvalibility : int * int ==> PointAvalibility
  pre IsValidGridPoint(mk_Point(x,y));
 public SetPointMP : map Point to PointAvalibility ==> ()

 public IsValidGridPoint : Point ==> bool
end Grid

class GridTest is subclass of TestCase
operations
 public GridTest : seq of char ==> GridTest
 protected RunTest: () ==> ()

  protected TearDown: () ==> ()
end GridTest
~~~{% endraw %}

###MovingObstacle.vdmrt

{% raw %}
~~~
class MovingObstacle
 private Step : () ==> ()
        if direction = <South> then
        if direction = <East> then
        if direction = <West> then
      );
 );
 public GetPos: () ==> Grid`Point
 public Stop: () ==> ()
sync
mutex(SetPos, GetPos);

end MovingObstacle


class MovingObstacleTest is subclass of TestCase
operations
 protected RunTest: () ==> ()
  protected TearDown: () ==> ()
end MovingObstacleTest
~~~{% endraw %}

###NextMoveController.vdmrt

{% raw %}
~~~
class NextMoveController
instance variables
sync
-- no simultanious calls
mutex (LocateMovingObstacles);
operations
 public NextMoveController: () ==> NextMoveController
public AddMovingObsticle : MovingObstacle ==> ()
private LocateMovingObstacles : () ==>()
    let m = { mo.GetPos()|-><Occupied> | mo in set mobs} in
private SetObs: map Grid`Point to Grid`PointAvalibility ==> ()
private WaitForAvalibility: Grid`Point ==> ()
     skip;
private IsPointBlocked: Grid`Point ==> bool

          Util`PrintDebug("Requesting Pos");
if p in set dom obs then
   return p in set dom obs;
 public GetNextPoint : set of Grid`Point * Grid`Point ==> [Grid`Point]
          WaitForAvalibility(p);


           return p
functions
  Distance: Grid`Point * Grid`Point -> nat
end NextMoveController


class NextMoveControllerTest is subclass of TestCase
operations
 protected RunTest: () ==> ()
  protected TearDown: () ==> ()
end NextMoveControllerTest
~~~{% endraw %}

###ObstacleSensor.vdmrt

{% raw %}
~~~
class ObstacleSensor
instance variables
operations
 public GetPointAvalibility : Grid`Point ==> Grid`PointAvalibility
  public GetDirection : () ==> SensorDirection
end ObstacleSensor


class ObstacleSensorTest is subclass of TestCase
operations
 protected RunTest: () ==> ()
   );

  protected TearDown: () ==> ()
end ObstacleSensorTest
~~~{% endraw %}

###Robot.vdmrt

{% raw %}
~~~
system Robot
instance variables
-- BUS speed does only have effect if large amount of data 

 private name : set of char;

 public static dataReader : DataReader := new DataReader();
 public static mobs1 : MovingObstacle 
 public static nmc : NextMoveController := new NextMoveController();
   cpu5.deploy(dataReader);
   cpu1.deploy(steering);
   cpu3.deploy(mobs1);
   cpu4.deploy(mobs3);
   cpu2.deploy(nmc);
 );
end Robot
~~~{% endraw %}

###RobotTest.vdmrt

{% raw %}
~~~
class RobotTest
operations
end RobotTest
~~~{% endraw %}

###SteeringController.vdmrt

{% raw %}
~~~
class SteeringController
MAX_POINT : Grid`Point = mk_Grid`Point(100,100);
instance variables
operations
 public SteeringController : () ==> SteeringController
-- GET
 private GetPointDirection : Grid`Point ==> ObstacleSensor`SensorDirection
   );
 private GetBatUsage: () ==> nat
 private GetPos : () ==> Grid`Point
 private GetRoutes : () ==> seq of Route
 private GetNeighbourPoints : () ==> set of Grid`Point
  post RESULT =

  private GetNextMove : set of Grid`Point ==> [Grid`Point]
 );
 private IsDestination : Grid`Point ==> bool
 private DoesRouteHaveMoreOptions : () ==> bool
 private IsInRoute : Grid`Point ==> bool

-- operations moving or changing the location
 private StartNewRoute : () ==> ()
  public ReturnToBase : () ==> ()
 private Move : Grid`Point ==> ()
  pre GetBatUsage()*2 <= batCap and batCap > 1 and 


 private RestartNewRoute : () ==> ()
 private FindRoute : () ==> Grid * seq of Route * Grid`Point * bool
    if FindRouteToDestination() then
   Util`PrintDebug("The End");


  private FindRouteToDestination : () ==> bool
 );


 private DiscoverUnknownNeighbourPoints : set of Grid`Point ==> ()
 )
 public SetDiscoverInfo: Grid`Point * Grid`Point * int ==> ()
 public AddObstacleSensor: ObstacleSensor ==> ()


 public isFinished : () ==> ()
  sync
thread
sync
per FindRoute => #fin(SetDiscoverInfo) > #fin(FindRoute);
mutex (FindRoute);
mutex (SetDiscoverInfo, FindRoute);
functions
ValidatePoint: Grid`Point * Grid`Point -> bool


class SteeringControllerTest is subclass of TestCase
operations
 protected RunTest: () ==> ()

  protected TearDown: () ==> ()
end SteeringControllerTest
~~~{% endraw %}

###Storage.vdmrt

{% raw %}
~~~
class Storage
types
values
inv startIndex > 0 and destIndex > 0 and batCapIndex > 0;
 public Storage : () ==> Storage

  public Load : seq of char ==> Grid
    return SetData(inData);
 private SetData : seq of inDataType ==> Grid
 )
 public Save : Grid * seq of SteeringController`Route * Grid`Point * bool ==> ()
private WriteMap: Grid`Point * Grid`PointAvalibility ==> ()

private WriteRoute: SteeringController`Route ==> ()
 );
private PrintInt: nat ==> ()
private PrintLine: seq of char  ==> ()
end Storage



class StorageTest is subclass of TestCase
instance variables
values
operations
 public StorageTest : seq of char ==> StorageTest
 protected SetUp: () ==> ()
 protected RunTest: () ==> ()
      AssertTrue(<Occupied> = completeGrid.GetPointAvalibility(1,1));

  protected TearDown: () ==> ()
end StorageTest
~~~{% endraw %}

###Util.vdmrt

{% raw %}
~~~
class Util

operations
 public static PrintValue: Grid`Point ==> ()

 public static PrintInt: int ==> ()


end Util
~~~{% endraw %}

###VDMUnit Framework.vdmrt

{% raw %}
~~~
class Test
operations
end Test

class TestCase
instance variables
operations
 public GetName: () ==> seq of char
 protected AssertTrue: bool ==> ()
 protected AssertFalse: bool ==> ()
 public Run: TestResult ==> ()
 protected SetUp: () ==> ()
 protected RunTest: () ==> ()
 protected TearDown: () ==> ()
end TestCase

class TestSuite
instance variables
operations
 public Run: TestResult ==> ()
 public AddTest: Test ==> ()
end TestSuite

class TestResult
instance variables
operations
 public Print: seq of char ==> ()
  public Show: () ==> ()
end TestResult
~~~{% endraw %}

###World.vdmrt

{% raw %}
~~~
class World
instance variables
public static env : [Enviroment] :=nil;
operations
   Robot`steering.AddObstacleSensor(Robot`obsSensorNorth);

 );
 public Run : () ==> ()
 );

end World
~~~{% endraw %}
