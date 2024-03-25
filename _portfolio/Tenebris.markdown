---
title: "Tenebris"
order: 2
excerpt: "Tenebris is an hypercasual endless-runner 2D game made with Unity"
header:
  overlay_image: /assets/images/TenebrisOverlay.gif
  teaser: /assets/images/TenebrisMainMenu.png
  overlay_color: "#000"
  overlay_filter: "0.5"
  actions:
    - label: "Tenebris on GitHub"
      url: "https://github.com/TeamNameStudios/Tenebris"
    - label: "Tenebris on Itch.io"
      url: "https://jackalone.itch.io/tenebris"
sidebar:
  - title: "Role"
    text: "Game Programmer"
  - title: "Main responsabilities"
    text: "- General Game and Progression System\n
           - Procedural Content Generation\n
           - Enemies System\n 
           - Tools Programming\n 
           - Key mapping system"
  - title: "Technologies used:"
    text: "- C#\n
           - WFC algorithm \n
           - Scriptable Objects\n
           - Prefabs"        
  - title: "Engine: Unity"
    text: ""
  - title: "Team size: 6"
    text: "- 2 Game Programmers\n
           - 4 Game Designers"
  - title: "Time frame: 4 months"
    text: " "
permalink: /Tenebris/

gallery:
  - url: /assets/images/TenebrisMainMenu.png
    image_path: /assets/images/TenebrisMainMenu.png
    alt: "Tenebris Menu"
    title: "Main menu"
procedural:
  - url: /assets/images/TenebrisChunkMovement.gif
    image_path: /assets/images/TenebrisChunkMovement.gif
    alt: "Tenebris chunk movement"
    title: "Chunck movement"
keymap:
  - url: /assets/images/TenebrisRebindKeys.gif
    image_path: /assets/images/TenebrisRebindKeys.gif
    alt: "Tenebris rebind keys"
    title: "Rebind keys"
runner:
  - url: /assets/images/TenebrisRunner.gif
    image_path: /assets/images/TenebrisRunner.gif
    alt: "Tenebris Runner"
    title: "Runner"
lurker:
  - url: /assets/images/TenebrisLurker.gif
    image_path: /assets/images/TenebrisLurker.gif
    alt: "Tenebris Lurker"
    title: "Lurker"
chaser:
  - url: /assets/images/TenebrisChaser.gif
    image_path: /assets/images/TenebrisChaser.gif
    alt: "Tenebris Chaser"
    title: "Chaser"
dialogue:
  - url: /assets/images/TenebrisDialogue.gif
    image_path: /assets/images/TenebrisDialogue.gif
    alt: "Tenebris Dialogue"
    title: "Dialogue Box"
---

## Overview

Tenebris is a 2D platform endless-runner (procedurally generated) in which a cultist fails an invocation ritual and is now chased by Tenebris, a giant entity with the only purpose of killing him.<br>
The gameplay features classic systems and mechanics of its genre, a player in an endless run for its life while avoiding obstacles using its abilities, the Dash and the Grapple.<br>


## What I have learned

Tenebris was my first big project using Unity and it’s the one where I had the opportunity to really hone my skills about <strong>C#</strong> and <strong>knowledge about the engine</strong> by the application of everything I learned, in particular:
- Game design patterns:
    - <strong>Singleton</strong> pattern;
    - <strong>Factory</strong> pattern;
    - <strong>Object Pool</strong> pattern;
    - <strong>Publisher/Subscriber</strong> pattern;
- Procedural Content Generation - <strong>WFC Algorithm</strong>


## Event Driven Approach - Event Manager

During the development of this project I made extensive use of the <strong>Publisher/Subscriber design pattern</strong>.<br> 
For this I implemented a GameObject called <strong>EventManager</strong> [(click here to see the script)](https://github.com/TeamNameStudios/Tenebris/blob/master/ProjectProject/Assets/_Scripts/Utilities/EventManager.cs) as the broker. This holds a dictionary of string (eventNames as keys) and Action (the actual delegate); through it objects can subscribe their callbacks methods to it using the correct event name string.<br>
I found myself really comfortable using this event driven approach because it made the development of the project much faster and I achieved a <strong>good level of decoupling</strong>, making the whole architecture more <strong>modular</strong> and <strong>maintainable</strong>.


## Procedural Content Generation
### Simple implementation of the Wave Function Collapse algorithm

After studying about the WFC algorithm I wanted to try and make something with it so I decided to take care of the PCG during this project.<br>
Unfortunately, due to the simple nature of the game, in the end I opted for a really simple implementation of it, that still follows the general idea of the WFC algorithm, but works only in the right direction.<br>
First some facts about the game,the player does not move but we give the illusion of movement by moving the background and the “chunks”.<br> 
{% include gallery id="procedural" %}
This is a container in which the <strong>LevelAssembler</strong> (the script that handles the procedural generation), places the level asset chosen by the algorithm [(click here to see the script)](https://github.com/TeamNameStudios/Tenebris/blob/master/ProjectProject/Assets/_Scripts/Controllers/LevelAssembler.cs).<br>
Starting from the initial level asset, the algorithm takes all the possible level assets (possible neighbors) that this level can be connected to and picks one according to its probability to spawn.<br>
I then reset the probability of the chosen level asset and increase the probability of all the level assets that were not chosen; this allows a good distribution of all the level assets among each run.<br>
The chosen one is then spawned beside the previous one and the process repeats itself starting from all the possible neighbors this newly created level has.

### Level Scriptable Object

A level asset is a scriptable object with which the design team was able to create new levels in a quick and easy way; in particular what they had to do was create a new asset and specify a LevelID (an enum that held every level created so far), the level prefab, its intended difficulty and probability and a list of all the possible level it could connect to.

## Game Systems
### Game Progression System

The game progression system is strictly related to both the procedural content generation and the enemies since those are the two areas of the game that changes over time:
- Every X seconds all the enemies “level up”, becoming faster at falling or at chasing the player;
- Every Y minutes, the base probability of all levels of easy and medium difficulty decreases and the probability of all levels of hard and insane difficulty increases.

Both increase and decrease until a cap value and all these values are <strong>easily customizable by the design team</strong>.

### Corruption System

The corruption is the counterpart of health for the player, it is represented by a bar that fills a bit each time the player gets hit by an enemy or gets too close to the shadow that is chasing it but also each time the player uses one of his abilities.<br>
The value of corruption slowly decreases if it has not incremented for some time but only if it did not reach its max value, when this happens the player’s abilities are less effective and most important another hit from an enemy could kill it, causing game over. The corruption value stays at its max for some time before resetting to zero.

### Key Mapping System

During the final stages of the development I wanted to challenge myself and I started researching a way to implement a system to let the user remap the keys at will, since Unity does not have a native one. It requires:
- an enum for each “action type” (ex. “jump”, “dash”),
- a dictionary of action types and key codes
- a Coroutine that, when the rebinding starts for a specific action type, waits for the key code input by the user and exchanges that in the dictionary. Before ending the coroutine the input controller gets updated with the new key code for that action type.

I decided to go store everything into a dictionary to always have an updated reference on what key code is bound to an action type, since during the tutorial the text needs to show the correct key when explaining to the player how to perform a specific action.<br>
[If you want to take a look at the code you can find it here!](https://github.com/TeamNameStudios/Tenebris/blob/master/ProjectProject/Assets/_Scripts/Controllers/KeymapController.cs)
{% include gallery id="keymap" %}

## Enemies

After finishing the PCG I decided to focus on the enemies of the game and in particular on their class architecture, how to spawn them and their behaviors.<br>
Since the scope of the project was small I decided to just make an abstract class containing the basics:
- the logic to apply the damage to the player;
- the self-destruct coroutine.

I developed three kinds of enemies all with simple but entertaining behaviors:
- The [Runner](https://github.com/TeamNameStudios/Tenebris/blob/master/ProjectProject/Assets/_Scripts/Entity/Manifestations/Runner.cs), that runs towards the player copying its movements from the right side of the screen;
{% include gallery id="runner" %}
- The [Chaser](https://github.com/TeamNameStudios/Tenebris/blob/master/ProjectProject/Assets/_Scripts/Entity/Manifestations/Chaser.cs), that chases the player from the left side of the screen slowly accelerating until it outruns it;
{% include gallery id="chaser" %}
- The [Lurker](https://github.com/TeamNameStudios/Tenebris/blob/master/ProjectProject/Assets/_Scripts/Entity/Manifestations/Lurker.cs), that lurks over the player's head before crashing to the ground.
{% include gallery id="lurker" %}

### Factory and Object Pool Game design pattern

To optimize the spawn and destruction of the enemies I used the Factory game design pattern in conjunction with the Object Pool pattern.<br>
I created an object called <strong>ManifestationFactory</strong> that at the start of the game creates three object pools, one for each enemy type and handles the activation/deactivation of the enemy gameobject when requested.<br>
The spawn request is made by the spawn of a specific trigger that the design team placed in the level during the creation of the prefab, this allowed them complete and autonomous control over where each enemy should be spawned.<br>
You can find the link for my implementation of these patterns here for the [Factory](https://github.com/TeamNameStudios/Tenebris/blob/master/ProjectProject/Assets/_Scripts/Controllers/ManifestationsFactory.cs) one and here for the [Pool](https://github.com/TeamNameStudios/Tenebris/blob/master/ProjectProject/Assets/_Scripts/Utilities/Helper.cs) one (line 10 - 95).


## Dialogue Tool

I also took care of most of the UI in the game (main menu, pause and gameover screen) in regards to transitions, sounds and logic.<br>
In particular I developed a dialogue box that, given a list of lines to show, cycles through each line and prints them letter by letter.<br>
To aid the design team in the creation of the tutorial dialogues I prepared two objects to use in conjunction: 
- A GameObject with a simple trigger and a “tag” string;
- A ScriptableObject where the design team should input;
    - A “tag” (that should match the trigger tag)
    - A position on where to spawn the dialogue box
    - A list of all the lines that dialogue box should show
{% include gallery id="dialogue" %}
