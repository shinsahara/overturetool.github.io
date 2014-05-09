---
layout: default
title: ConwayGameLife
---

~~~
Conway's Game of Life
The initial pattern constitutes the seed of the system. The first generation is created by 
~~~
###Conway.vdmsl

{% raw %}
~~~
/**
module Conway 
definitions
values
types
	Population = set of Point;

functions
	-- Count the number of live cells around a given point 
	-- Generate the set of empty cells that will become live
	-- Generate the set of cells to die
	-- Perform one generation
	-- Generate a sequence of N generations 
	measureGenerations: nat1 * Population -> nat
    -- Generate an offset of a Population (for testing gliders)
	-- Test whether two Populations are within an offset of each other
	-- Test whether a game is N-periodic
	-- Test whether a game disappears after N generations
	-- Test whether a game is N-gliding within max cells
 	-- Versions of the three tests that check that N is the least value
	disappearNP: Population * nat1 -> bool
	gliderNP: Population * nat1 * nat1 -> bool

-- Test games from http://en.wikipedia.org/wiki/Conway%27s_Game_of_Life
	BLINKER = { mk_Point(-1,0), mk_Point(0,0), mk_Point(1,0) };
	TOAD = BLINKER union { mk_Point(0,-1), mk_Point(-1,-1), mk_Point(-2,-1) };
	BEACON = { mk_Point(-2,0), mk_Point(-2,1), mk_Point(-1,1), mk_Point(0,-2),  
	PULSAR = let quadrant = { mk_Point(2,1), mk_Point(3,1), mk_Point(3,2),
  DIEHARD = {mk_Point(0,1),mk_Point(1,1),mk_Point(1,0),
  GLIDER = { mk_Point(1,0), mk_Point(2,0), mk_Point(3,0), mk_Point(3,1), mk_Point(2,2) };      
  GOSPER_GLIDER_GUN = { mk_Point(2,0), mk_Point(2,1), mk_Point(2,2), mk_Point(3,0), mk_Point(3,1),
functions

end Conway

~~~{% endraw %}

###Graphics.vdmsl

{% raw %}
~~~
module gui_Graphics 
imports from Conway all
exports all
types
functions
	animate_step : seq of Conway`Population -> seq of Conway`Population
	measure_animate_step: seq of Conway`Population -> nat
	initialise: nat1 * nat1 -> int
  newLivingCell:  int * int -> int
  newRound: () -> int
values
	BLINKER = mk_Configuration(5, 500, Conway`BLINKER);
	TOAD = mk_Configuration(6, 500, Conway`TOAD );
	BEACON = mk_Configuration(8, 500, Conway`BEACON);
	PULSAR =  mk_Configuration(17, 1000, Conway`PULSAR);
  DIEHARD = mk_Configuration(33, 300, Conway`DIEHARD);
  GLIDER = mk_Configuration(40, 100, Conway`GLIDER);      
  GOSPER_GLIDER_GUN = mk_Configuration(40, 50, Conway`GOSPER_GLIDER_GUN);
end gui_Graphics
~~~{% endraw %}
