# Background
The stand allocation for flight arrivals and departures is an important task for airport operators at major airports. For example, as almost all COVID-19-related travel restrictions to Hong Kong has been lifted, it comes to a challenge for the airfield management to decide the minimum number of required parking stands, including frontal stands and remote stands, that can serve all scheduled passenger flights based on a given data of upcoming monthly flight schedule.
In case the number of flights exceeds the capacity of gate/stands parking, i.e., overflow towards remote stand parking, then the airport operators need to evaluate how many additional remote stands, which are not equipped with passenger-boarding bridge”, are required so as to facilitate planning of airside shuttle bus schedule for in advance.

# Model Selection
In this project, the Mixed-Integer Linear Programming (MILP) is applied to solve this problem. In our case, we want the model to find the feasible solution for the objective function subject to a set of constraints. A decision variable is a parameter that the model solver can adjust in its best effort to minimize the cost of objective function. Simply put, we want to run the model to assign stand j for each flight turn i, on the condition that the setting of stand allocation rules can be satisfied. 
Some constraints are so-called “hard-rules” for which that the model must satisfy, whereas some are “soft-rules” such as the preference of assigning flights to frontal stands instead of remote stands as much as possible.
In case there is no feasible solutions, it implies that the resource input of parking stands may not enough to serve the simulated scenario of aircraft operation. At this time, we will adjust input of remote stands and then re-run the model until the optimal solution can be found. 

# Constraints
-	Each parking stand can only handle specific aircraft size

-	Each flight turn must be assigned to a compatible parking stand

-	On grounds of airfield safety operation, at least 25 minutes interval (“time gap”) in-between slot of each aircraft occupied in a parking stand. For each parking stand, therefore, no flight-to-gate assignment exercise shall be made 12.5 minutes before the scheduled time of arrival (STA) of arriving flights until 12.5 minutes after the scheduled time of departure (STD) of departing flights

-	Aircraft with over 6 hours of ground time (i.e. STA - STD) will be towed away from the frontal stand

-	If the pair of flights do not correspond to the same zone (i.e. Green-Orange or Orange-Green), then towing protocol will also be applied

-	For towing plane, add additional 90 mins on top of ATD to cater for boarding, departing the plane and the physical process of towing. Besides, the aircraft needs to tow back to the compatible stand to prepare for embarking 1 hour prior to its STD

-	Mainland China (CN) flight shall be allocated to “Green zone” (exclusively defined by a set of parking stands), where remote stands #N141 - N145 are reserved for handling departure flights in Green Zone)

-	Non-mainland flight shall be allocated to “Orange zone” only

# Assumptions
1) The first arriving passenger flights were assumed to land in an empty airport, i.e., no parking stands were pre-occupied at the first beginning.
2) The flight schedule data is simulated as close as one day in Nov-22 flight schedule of Hong Kong International Airport (HKIA).
3) Our model only serves one objective in this study, despite there may have multiple objectives to consider by Airport Authority during stand allocation, such as assigning flights close to airline service counters, maintaining some degree of stand allocation patterns for regular flights from time to time, etc.

# Terminology
**Turns**: a pair of arrival and departure flights with the same aircraft <br />
**STA**: Scheduled Arrival Time <br />
**STD**: Scheduled Departure Time <br />
**Ground Time**: the planned time period for which an aircraft will occupy a particular stand, i.e. time between STA and STD <br />
**Frontal Stand**: a set of parking stands equipped with “passenger boarding bridge” for connection with the terminal building <br />
**Remote Stand**: an aircraft stand that is not airbridge-served, therefore requires require a shuttle bus service <br />
**Towing Stand**: a set of parking stand for serving towing plane <br />
**Green Zone**: Mainland flight shall be allocated to “Green” zone only <br />
**Orange Zone**: Non-Mainland flight shall be allocated to “Orange” zone only <br />
**Towing**: The turns followed by the towing protocol will be broken up as three turns as each of them is assigned to a different stand for parking: (1) disembarking in a frontal or remote stand; (2) temporary parking in a towing stand; (3) tow back to a frontal or remote stand for embarking 1 hour before STD
 

Model result is presented in Gantt Chart:<br />
https://marco-cheung.github.io/airport-gate-assignment-problem

<br />
*Special thanks to @Ilia Karmanov's repo for coding inspiration. 
