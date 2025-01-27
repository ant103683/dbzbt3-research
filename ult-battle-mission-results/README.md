# Ultimate Battle Mission Results
## Introduction
When completing any mission in Sim Dragon, Mission 100, Survival or Disc Fusion, 
the player is given points, based on fulfilled conditions/criteria, which are
then used to determine their rank for that mission.

Interestingly enough, Ultimate Battle and Dragon History share the same condition system,
except in the latter, it is only used to trigger new scenes, voice lines or other changes.

As a result, the game does not care if the player has done e.g. multiple Blast 2's.
Each condition is its own bit, therefore quantity is the least of the game's concerns.
## Main Criteria
Aside from the conditions above, the game also scores the following:
* Health Percentage;
* Highest Number of Hits in a Combo;
* Highest Amount of Damage.

As for how they are converted to points:
* ``pts += hp * 1000``;
* ``pts += maxHits * 4000``;
* ``pts += maxDmg * 5``;

In Survival, there is an additional condition: the number of opponents defeated.
* ``pts += oppsDefeated * 2000``;

NOTE: Before assigning the points, the ``maxDmg`` variable is rounded to the nearest multiple of 1000.
## Condition System
There is a total of 48 conditions used for Ultimate Battle. However, 14 of them go unused:
* Underwater Win;
* Player Hit More Than Opponent;
* Aerial Rush (referring to Air Combos?);
* Lift Opponent Into Air (referring to Lift Strikes);
* Knock Opponent Down;
* Dragon Heavy Hits;
* Aerial Combos;
* Blast Combo Hits;
* Ki Attacks Only (referring to Ki Blasts AND Blast 2's maybe?)
* Rushing Ki Attack Hits (referring to Ki Blasts);
* Blast Attacks Only;
* Throw Attacks Only;
* Successful Guards;
* Counter Opponent.
As for the rest, click here to check their names, comments, and points acquired from each one.
## Ranking System
Pretty straightforward, although it changes depending on the selected game mode.
### Normal Rankings
* Rank C: 000001-159900; 
* Rank B: 159901-219900; 
* Rank A: 219901-279900;
* Rank Z: 280001 or higher.
### Survival Rankings
* Rank C: 000001-209900; 
* Rank B: 209901-249900; 
* Rank A: 249901-319900;
* Rank Z: 319901 or higher.
