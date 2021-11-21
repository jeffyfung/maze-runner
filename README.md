# maze-runner
In this game, you strive to escape as many mazes as possible!

**Insert gameplay gif**

![image](https://i.postimg.cc/BbxC7JXB/maze.jpg)

# Game Play

## Enter player name and seed for random number generation
Mazes are generated pseudo-randomly (see `#About the components` for details).
Apart from pressing keys ("N", "L", "Q"), player can also click on the boxes of
the relevant options.

![Alt Text](https://media.giphy.com/media/tjbOHwlUnaiWBm9e0r/giphy.gif)

## Basic movement - "WASD" keys

![Alt Text](https://media.giphy.com/media/zc301aOLYLOB1ARMFC/giphy.gif)

## Field of view
The player can only see the tiles near them and hence has a field of view.
The field of view moves along with the player.

![Alt Text](https://media.giphy.com/media/qWRMWVuw1pozyq7K6T/giphy.gif)

Note: you can press "T" to temporarily disable field of view but that is cheating!

## HUD
A HUD is located at the under the generated maze and shows:
* Player's health
* Description of the tile the mouse cursor is currently pointing to 
* Game level
* Current date

![Alt Text](https://media.giphy.com/media/WvJUUxLNcPPc3D6dmR/giphy.gif)

## Special game objects
There are special objects in maze that can boost or hinder your progress, including:
* BREAD : Gives player a +10 health boost.

![Alt Text](https://media.giphy.com/media/vSyOHAdGY4V4rfhq0R/giphy.gif)

* PORTAL : Teleports player to the other side of the portal by pressing "H". Portals are bi-directional.

![Alt Text](https://media.giphy.com/media/QuAbj7VHQXPiim8h0g/giphy.gif)

* TORCH : Lights up the whole maze.

![Alt Text](https://media.giphy.com/media/QXSAoUbjXJeqxrOKUR/giphy.gif)  

* EXIT : Exits the maze and enter a new maze.

## Escaping maze until game over
A new maze is immediately generated after escaping the maze.
Game level is advanced by 1.

![Alt Text](https://media.giphy.com/media/Mm7pWhfLEt8mjEk3P8/giphy.gif)

Each movement consumes 1 health. The game is over when health falls to 0.
Leaderboard is shown when game over and player is prompted is enter if they
want to restart the game.

![Alt Text](https://media.giphy.com/media/AeB8s5mJRNhg4AUQkO/giphy.gif)

# About the components
The game is written in Java v15.0.2. It is based on a course project of
UC Berkeley's [CS61B](https://sp21.datastructur.es/materials/proj/proj3/proj3)
and most dependencies of this project come from there. The game is visualized by
a primitive drawing library, 
[`stdlib.jar`](https://introcs.cs.princeton.edu/java/stdlib/), and may be 
upgraded at a later point.

## Java Modules
Modules used in this game are stored in [`lib/`](./lib).

## To run the game
[`mazeRunner/Core/Main.java`](./mazeRunner/Core) is the entry point of the program,
 where the game can be started. 

There is an option to start non-interactive gameplay by passing `-s KEYSEQUENCES` as
command line arguments, which will call Engine.interactWithInputString(). This option
is only for debugging maze generation and does not contain the full functionality of
the game.

## Maze generation
Each maze is generated pseudo-randomly and based on a user-defined seed. Every
generated map contains rooms that are connected and every space inside the maze
is reachable (i.e. there is no isolated space).

The rooms are generated by sampling partitions of the 2D map space using a 
modified Kd-tree algorithm. 

The rooms are then connected by hallways by running
an approximate SPT algorithm based on Dijkstra's algorithm while a weighted quick
union disjoint set monitors the connectivity of the rooms until they are all
connected. 

Details of the above mechanisms are located inside [`mazeRunner/Core/Engine.java`](./mazeRunner/Core),
[`mazeRunner/Core/KdTree.java`](./mazeRunner/Core), [`mazeRunner/Core/DijkstraUndirMaskedSP.java`](./mazeRunner/Core),
[`mazeRunner/Core/WQUDisjointSet.java`](./mazeRunner/Core).

## Gameplay
Components in the gameplay (game objects) are controlled by
and [`mazeRunner/Core/GameMechanics.java`](./mazeRunner/Core). All game objects
are subclass of `GameObject` from [`mazeRunner/Core/GameObject.java`](./mazeRunner/Core).

## Scores and Leaderboard
The scores attained by a player is the number of rounds they have survived. Only
the top 3 entries are kept track of and displayed in the leaderboard, which is shown
after the game is over. Mechanisms of updating the leaderboard are governed by
[`mazeRunner/Core/Leaderboard.java`](./mazeRunner/Core).
