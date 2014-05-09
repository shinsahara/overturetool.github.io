---
layout: default
title: trayallocation
---

~~~
This VDM++ model is made by two students of a sortation system
#******************************************************
~~~
###AllocatorOneTray.vdmpp

{% raw %}
~~~
-- ===============================================================================================================
class AllocatorOneTray is subclass of AllocatorStrategy
	operations
	    -- AllocatorOneTray constructor
		-- Allocates tray if empty at induction offset
        -- Returns true if higher priority inductions in induction group

end AllocatorOneTray
~~~{% endraw %}

###AllocatorStrategy.vdmpp

{% raw %}
~~~
-- ===============================================================================================================
class AllocatorStrategy
	instance variables
	operations
		public AllocateTray: nat ==> set of Tray
		public InductionsWithHigherPriority: InductionController ==> bool
	functions
	    -- Calculate current tray UID at position in front of induction based on position of card reader 
end AllocatorStrategy
~~~{% endraw %}

###AllocatorTwoTray.vdmpp

{% raw %}
~~~
-- ===============================================================================================================
class AllocatorTwoTray is subclass of AllocatorStrategy
	operations
		-- AllocatorTwoTray constructor
		-- Allocates trays if empty at induction offset and offset + 1
        -- Returns true if higher priority inductions in induction group

end AllocatorTwoTray
~~~{% endraw %}

###InductionController.vdmpp

{% raw %}
~~~
-- ===============================================================================================================
class InductionController
	values
	instance variables
		-- If induction is waiting there must be items in sequence
	operations
    -- InductionController constructor
	-- Returns induction controller UID
	-- Returns priority of induction controller	
	-- Returns true if induction is wating with an item
	-- Get size of waiting item in number of trays
    -- Enviroment feeds a new item on induction
    -- Simulate sorter-ring moved one tray step
	-- It any items on induction then induct next item
	functions
	sync
	--thread
	traces
end InductionController
~~~{% endraw %}

###Item.vdmpp

{% raw %}
~~~
-- ===============================================================================================================
class Item
	values
	instance variables
	    sizeOfTrays : ItemTraySize; 	  -- Number of trays item occupies
	    -- If the item is on the sorter ring the size of trays the item occupies 
	operations
    -- InductionController constructor
	-- Return item id
	-- Returns the number of trays the item occupies
	-- Return item size
	-- Creates association between item and tray
	-- Remove item from sorter ring - Implicit operation - not used yet
end Item
~~~{% endraw %}

###ItemLoader.vdmpp

{% raw %}
~~~
-- ===============================================================================================================
class ItemLoader
		inline  =  nat * nat * nat;
	values
	instance variables
		-- Test with mix of 1 and 2 tray items 
	    numTimeSteps : nat := 21;
	operations
	-- Loads test scenario from file
	-- Returns number of time steps to simulate
	-- Returns size of item if found in test scenario
	functions
	sync
	--thread
	traces
end ItemLoader
~~~{% endraw %}

###SC.vdmpp

{% raw %}
~~~
-- ===============================================================================================================
class SC
	values
	instance variables
	operations
    -- SystemController constructor

	-- Notified each time sorter-ring has moved one tray step
	functions
	sync
	--thread
	traces
end SC
~~~{% endraw %}

###SorterEnviroment.vdmpp

{% raw %}
~~~
-- ===============================================================================================================
class SorterEnviroment
	values

	instance variables
	operations
    -- SorterEnviroment constructor
    -- Assigning item loader to SorterEnviroment
    -- Assigning induction group to SorterEnviroment
	-- Used by traces in TestSernarios
	-- Called by world each time sorter ring moves one tray step 
	 	for all i in set {1,...,TrayAllocator`NumOfInductions} 
    	-- Enviroment simulate sorter moved one tray step
 		-- Performs tray step for each induction
	);

	functions
	sync
	--thread
	traces
end SorterEnviroment
~~~{% endraw %}

###String.vdmpp

{% raw %}
~~~
-- ===============================================================================================================
class String
	values
	instance variables
	operations
	static public NatToStr: nat ==> seq1 of char
		if val = 0 then string := "0";
		while x1 > 0 do
		return string;
	functions
end String
~~~{% endraw %}

###TestSenarios.vdmpp

{% raw %}
~~~
-- ===============================================================================================================
class TestTraces
	values
	instance variables								
	    testfile1 : seq1 of char := "\\scenario1.txt";
	    testfile2 : seq1 of char := "\\scenario2.txt";
	operations
	functions
	sync
	--thread
	traces
    -- To run TestSenarious - IO`print has to be commented out
end TestTraces
~~~{% endraw %}

###Tray.vdmpp

{% raw %}
~~~
-- ===============================================================================================================
class Tray
	values
	instance variables
		-- It is allowed for a tray to be <Full> with no item associated 
		-- If an item is associated with a tray the state must be <Full>
		id : UID;									 -- Tray UID
	operations
    -- Tray constructor
 	-- Return tray id
    -- Returns true if tray is empty
    -- Returns true if tray is full
	-- Returns item on tray
	-- Set state of tray	
	-- Returns state of tray ==> <empty> or <full>	
    -- Puts an item on the tray and creates association between tray and item 
		IO`print("-> Item id " ^ String`NatToStr(item.GetId()) ^ 
	functions
	sync
	--thread
	traces
end Tray
~~~{% endraw %}

###TrayAllocator.vdmpp

{% raw %}
~~~
-- ===============================================================================================================
class TrayAllocator
	types
	values
	instance variables
		countTraySteps : nat := 0;  	-- Used for calculation of throughput
		-- Induction group and invariants 
		-- Sorter ring and invariants	
		-- Tray at card reader and invariants
		-- Allocation "strategy pattern" for one and two tray items
	operations
    -- TrayAllocator constructor
        -- Creating strategies for allocation of one and two tray items
    -- Simulate sorter-ring moved one tray step
	    -- Simulate change of Tray state 
	 	-- Count the number of tray steps 
	-- Inducting item on sorter if empty trays and no higher induction priority
		-- Determine the strategy to compute the allocation of trays
		-- Central part of the Tray allocation algorithm 
	-- Assign item on empty trays 
	-- Returns true if sorter is full
	-- Returns calculated throughput of soter capacity for current state of sorter ring
	functions
	-- Calculates the current throughput based on items on sorter ring 

	sync
	--thread
	traces
end TrayAllocator
~~~{% endraw %}

###World.vdmpp

{% raw %}
~~~
-- ===============================================================================================================
class World
	values
	instance variables
		-- Test files that contains test scenarios 
	    testfiles : seq of seq1 of char := [testfile1,
   -- World constructor
    -- Prints configuration and result of tray allocation model simulation
		-- Prints result of simulation
		IO`print("----------------------------------------------\n");
	public Run: () ==> ()
			IO`print("---------------------------------------------\n");
			-- Performs simulation based on time steps	
			PrintSimulationResult();
	functions
	sync
	--thread
	traces
end World
~~~{% endraw %}
