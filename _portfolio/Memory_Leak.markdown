---
title: "Memory Leak"
order: 3
excerpt: "Memory Leak is a survival tower defense 3D game with roguelite elements made in Unreal Engine"
header:
  overlay_image: /assets/images/MemoryLeakLastLever.gif
  teaser: /assets/images/MemoryLeak.png
  overlay_color: "#000"
  overlay_filter: "0.5"
  actions:
    - label: "Memory Leak on GitHub"
      url: "https://github.com/Helluva-Studio/Project-Factory"
    - label: "Memory Leak on Itch.io"
      url: "https://arakiwi.itch.io/memory-leak"
sidebar:
  - title: "Role"
    text: "Game Programmer"
  - title: "Main responsabilities"
    text: "- General Game and Progression System\n
           - AI Programming\n
           - Enemies System\n 
           - UI Programming\n 
           - Sound Programming"
  - title: "Technologies used:"
    text: "- Blueprint\n
           - Behavior Tree\n
           - State Tree\n
           - Blueprint Widget\n
           - Sound Cue\n
           - Data Table"        
  - title: "Engine: Unreal Engine 5"
    text: ""
  - title: "Team size: 9"
    text: "- 4 Game Programmers\n
           - 5 Game Designers"
  - title: "Time frame: 4 months"
    text: " "
permalink: /MemoryLeak/
gallery:
  - url: /assets/images/MemoryLeak.png
    image_path: /assets/images/MemoryLeak.png
    alt: "Memory Leak Menu"
    title: "Main menu"
gallery2:
  - url: /assets/images/MemoryLeak-Enemies.jpg
    image_path: /assets/images/MemoryLeak-Enemies.jpg
    alt: "Memory Leak UML"
    title: "Enemies Class Diagram"
gallery3:
  - url: /assets/images/StateTree.png
    image_path: /assets/images/StateTree.png
    alt: "Memory Leak State Tree"
    title: "Bossfight State Tree"
gallery4:
  - url: /assets/images/MemoryLeakMenuGif.gif
    image_path: /assets/images/MemoryLeakMenuGif.gif
    alt: "Memory Leak UI"
    title: "Memory Leak Main Screen"
  - url: /assets/images/MemoryLeakLore.gif
    image_path: /assets/images/MemoryLeakLore.gif
    alt: "Memory Leak UI"
    title: "Memory Leak Lore Screen"
gallery5:
  - url: /assets/images/MemoryLeakBossfight.gif
    image_path: /assets/images/MemoryLeakBossfight.gif
    alt: "Memory Leak Bossfight"
    title: "Bossfight"
gallery6:
  - url: /assets/images/MemoryLeakLastLever.gif
    image_path: /assets/images/MemoryLeakLastLever.gif
    alt: "Memory Leak Last Lever"
    title: "Last lever"
gallery7:
  - url: /assets/images/MemoryLeakFlyingPathfinding.gif
    image_path: /assets/images/MemoryLeakFlyingPathfinding.gif
    alt: "Flying Pathfinding"
    title: "Flying Pathfinding"
---
{% include gallery %}
## Overview

Memory Leak is a third-person survival game with roguelite and tower defense elements  where the player controls Roberto, a robot that must survive in an procedurally generated factory by looting resources and by keeping a variety of monsters at bay with the help of its powerful turrets. These will be unlocked throughout each run and will aid Roberto in its escape in many different ways.<br>


## What I have learned
This was the first project I've ever worked on in <strong>Unreal Engine 5</strong> so thanks to it I was able to get accustomed with the Engine, its structure and its gameplay framework.<br>
For this reason and because of the time constraints this project was fully developed using <strong>blueprints</strong>, a tool I became really comfortable with.<br>
My main focus was the enemies and their Artificial Intelligence so I was able to experiment a lot with Unreal Engine <strong>behavior trees, state trees and AI perception</strong> component.<br>
During the development I also had the opportunity to dive into UI Programming using Unreal Engine <strong>widget blueprints</strong> and I handled the game progression system.<br>


## Game Progression System
Each new run is staged in a procedurally generated factory made up of hand-crafted rooms that the player needs to open to proceed in the game. I had the task to come up with a system to handle the general game progression: a simple system that keeps track of the time and manages the probability of the frenzy, an event where all the enemies alive would get stronger and chase the player even if far away from him.<br> 
The probability is handled starting from an initial value and then incrementing each time the method fails, if it succeeds then a frenzy can start and the probability gets reset to an initial value (that also increments during the run).<br>
Also at the end of the dungeon generation the system puts 4 levers in 4 specific rooms: the southernmost one, the northernmost one and so on. Whenever a lever gets pulled a frenzy gets triggered and all enemies level up. When all the 4 levers are pulled then the teleport in the first room of the factory is activated and the player is able to reach the bossfight.
{% include gallery id="gallery6" %}


## Enemies
My main task during this project was to develop the enemies and a general system that manages them. Regarding the latter I came up with a system that creates a pool of possible enemies at the start of each new survival day (8 minutes in-game), each enemy with its own probability to spawn. There are 2 enemies per room and a room gets populated only when the player opens it, whenever an enemy is killed an internal timer starts (each enemy has its own timer) and at the end of it it respawns in the room where the player is or in the adjacent ones.<br>

### An Object Oriented Approach
Once the design team was done ideating the enemies’ archetypes the very first thing I did was a class diagram to help me define a good object oriented structure in order to assure <strong>flexibility, maintainability, to avoid code repetition and most important to allow me to further expand and/or add new enemies or behaviors</strong>.<br>
{% include gallery id="gallery2" %}

### A Data Driven Approach
To aid the design team in the definition of the enemies’ stats I decided to adopt a <strong>data driven approach</strong>: I created a <strong>data table asset</strong> for each enemy, these were composed of 5 rows, the possible levels, and various entries, the specific enemy statistics. Whenever an enemy spawns he reads and sets all its values from his data table according to the level he is spawning.<br>
Thanks to this approach the design team was able to implement more accurate balance changes faster since they were able to see the big picture altogether.<br>

### Artificial Intelligence Implementation
Each enemy is controlled by a BasicEnemyController who handles the entering/exiting from the frenzy state and manages all the senses, in particular the <strong>sight one, hearing one, touch one and prediction one</strong>. For each sense I implemented a series of checks to make sure that the enemy is actually sensing the player and that the perception is successful, after these checks, the controller communicates with the blackboard so that this new information can be used in the decision making done by the <strong>behavior tree</strong>.<br>
The basic behavior tree has 4 main branches:
- <strong>Patrol</strong> (choose a random point in a given range and reach it)
- <strong>Investigate</strong> (goes toward the source of a noise)
- <strong>Player detected</strong>:
    - Chase player
    - Attack player
- <strong>Turret detected</strong> (only if the player has not been detected or if it is too far away)
    - Reach turret
    - Attack turret

The flow of the behavior tree is regulated by the conjunction use of <strong>decorators and services</strong>, both premade and custom made.<br>
For example the decision of shooting the player made by a ranged enemy is made only if the player is in sight (checked thanks to a decorator), if it has been inside its range long enough (checked thanks to a service) and if the tag cooldown on that task is expired (checked thanks to another decorator).<br>

#### Flying Pathfinding Problem
A curious problem I had to deal with while working with the Artificial Intelligence was on how to make a pathfinding for the flying enemies since the solution Unreal Engine offers does not include this feature.<br>
Since making our own 3D pathfinding algorithm was completely out of scope, the “creative” solution we came up with was to make <strong>an invisible flying floor where only the flying enemies could walk</strong>.<br> 
This gives a good illusion of flying although there were some care had to be taken with the level design.<br>
However, this was better than the more classic solution of having the enemy walk on the ground but with its collider and mesh in the air because this method has a major flaw in my opinion: the flying enemy would have of course avoided all obstacles, also those lower than him.<br>
{% include gallery id="gallery7" %}

### Bossfight
#### Why the state tree
Designing and developing the AI for the bossfight was the thing I enjoyed the most during this project. For it I decided to use the <strong>state trees</strong> since already during the development of the normal enemies’ AI I realized that the behavior trees were not the best choice for me for two main reasons:
- while designing the overall architecture I always had a state-like structure in mind and make that work in behavior trees resulted in trees a little too cluttered;
- managing transitions from a branch to another one wasn’t that much clean.

Given these reasons I understood that behavior trees were not the correct choice and that I was using them the wrong way, like a state machine.<br>
Switching to state trees made the design and implementation of the bossfight AI much easier and readable thanks also to the possibility of managing the various transitions on a case-by-case basis.<br>

#### The state tree implementation
The boss’s state trees has a static hierarchy of priorities, from top to bottom:
1. He will try to <strong>build a turret</strong> of its own (only if there are construction points available and if he has not yet reached its build limit);
2. He will try to <strong>attack the player</strong> (only if it is inside a given range)
3. He will try to <strong>attack one of the turrets built by the player</strong> (if there are any, if there are not then he will chase the player down regardless of its distance)

The boss also holds a special state called “Desperation Move” triggered only when the damage received within a specific time frame exceeds a threshold causing him to release it in the form of an explosion that deals damage to everything in its path.<br>
{% include gallery id="gallery3" %}
{% include gallery id="gallery5" %}

A possible improvement to the boss’s AI would be changing its static hierarchy of priorities into a dynamic one, a <strong>Utility System</strong>, where according to the specific moment a score is calculated for each possible behavior (or state) and the higher one gets chosen and performed. In my opinion this would make for a better AI and overall a more challenging bossfight experience.<br>


## User Interface
Another aspect of the development that I really enjoyed was programming the various <strong>menus and submenus of the game and their opening/closing animations</strong>.<br>
In particular I took care of:
- the main menu 
- the win/game over screen 
- the turret selection screen (the screen before the start of a new game that let the player select which turret he would like to start the game with)
- the dialogue screen (implemented at the beginning and end of the game to show the plot)

For the latter I developed a simple “tool” meaning that the designers could input inside a data table all the information about the text they wanted to show (like the actual text, fontsize, color and so on) and the widget would then adapt to the design team's liking.<br>
{% include gallery id="gallery4" %}
