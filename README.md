# Dynamic Scoping In Pearl: Maze Race
## CSCI3180 – Principles of Programming Languages – Spring 2019

```
Assignment 3 — Perl and Dynamic Scoping
```
# 1 Introduction

The purpose of this assignment is to offer you the first experience with Perl, which supports both
dynamic and static scoping. Our main focus is on dynamic scoping.

The assignment consists of two parts. First, you need to implement a game called “Maze Race”
in Perl with dynamic scoping. Then, you are required to re-implement this task using Python with
static scoping. The detailed object-oriented design for the Perl implementation is given, and you are
asked to design a similar framework in the Python realization. In the process, you will experience
both the programming flexibility and readability with dynamic scoping. Your implementation in
Perl should be able to run with Perl v5.14.2. Besides, you are required to add “use warnings;” at
the start of your program. For the Python part, Python 3.6 is demanded. Good coding styles are
expected.
IMPORTANT:All your codes will be graded on the Linux machines in the Department.
You are welcome to write your codes on your own machines, but please test them on the given
Departments machines (the perl version in linux1 is 5.14.2) before your submission.
NO PLAGIARISM!You are free to devise and implement the algorithms for the given tasks,
but you must not “steal/copy” codes from your classmates/friends. If you use some code snippets
from public sources or your classmates/friends, ensure you cite them in the comments of your code.
Failure to comply will be considered as plagiarism.

# 2 Task: Maze Race

In this task, you need to implement a maze race game with Perl. Note that dynamic scoping is
beneficial and required here, and you need to explain why your implementation using dynamic
scoping (specifically, with thelocalkeyword in Perl) is more convenient.
With dynamic scoping, you can 1) implicitly affect the behavior of functions in function calling
sequences without passing extra parameters, and 2) temporarily mask out some existing variables.
Full marks on this task demands you to show both of these two properties with your code and
explanations.
You shouldstrictly follow our OO design in you implementation. You have to follow the
prototypes of the given classes exactly. You can choose to add new member variables and functions
if necessary.

## 2.1 The Story

In ancient Greece, there was a mysterious creature called Minotaur, with a bull-like head and tail,
and a humanoid body. The surrounding villagers had been suffering from the monster’s bullying
for a long time. Barely a man could match Minotaur’s mighty power and agility, let along killing it.
To seek peaceful living, Greek people came up with a temporary solution to isolate the monster.
The architect Daedalus and his son Icarus designed and built the Labyrinth (a Greek term for
maze). Then they seduced Minotaur to dwell at the center of the Labyrinth to prevent it from
engaging with humans. But still, nobody could deliver death to Minotaur even it was trapped in
the building. Years later, the mighty Greek hero Theseus rose and took the mission of terminating
Minotaur. He received the blueprint of the Labyrinth from Daedalus and found a feasible path to
kill Minotaur.
Thousands years after the battle between Theseus and Minotaur, rumors are spread across
the world that Theseus left some treasures in that Labyrinth. You, a promising and gifted Greek
explorer, make up your mind to retrieve these precious items for your homeland. Meanwhile, a
heritage smuggling company employs a treasure hunter to loot for Theseus’s legacy. So please be
cool and quick, draw your strategies now.


## 2.2 Task Description

You are required to realize a treasure hunting game that involves two players: Player 1 and Player

2. Both players are located in a different entrance of a maze respectively. The maze is organized
into a 2-dimensional array of cells, which can be empty or occupied by a solid wall, a player, or
the treasure. Besides, they both know where the treasure is butinitiallyknow nothing about the
maze itself except their own locations and the dimensions of the maze (height and width). The
initial positions of the two players are of the same Manhattan distance to the treasure. Normally,
a player can attempt to move one step to an adjacent empty cell in the north (N), south (S), east
(E), and west (W) directions. A player cannot move out of the maze. A wall always block a move,
except when a special move is employed. Initially, all cells are unknown, except those containing
the two players and the treasure. As a player attempts to move to the next cell, the new cell will
be revealed to all players.
    This game is turned-based, meaning the two players make move alternately. Player 1 moves
first. The player whoreachesthe treasure first will win this game. Note thatreachingthe treasure
means the playerhas just moved intothe cell containing the treasure.

![](https://github.com/Huzo/Dynamic-Scoping-in-Perl-Maze-Race/blob/master/figure1.png)
Figure 1: The neighborhood of Player 1 usingmove. The green cell is feasible while red cells are
blocked. Best viewed on screen.

Players move in the maze alternately. In addition to anormal move, a player can make three
more special moves:rush,walk through a blocked cell, andteleport.

- Amoveenables a player to move from his/her current location to a neighboring (the 4 com-
    pass direction: N, W, S, E, like where the arrows point to in Figure 1) andfeasible/available
    (emptyoronly containing the treasure) cell. If the neighboring cell of the current player
    is a wall or occupied by the other player, then the current player remains in the same
    cell after attempting the move. The detailed logic ofmoveis given in the skeleton code
    (maze_race_components.pm) within classPlayer.
- Players canrushthrough a clean (no obstacles) vertical or horizontal path until right before
    an edge of the maze, or a cell that is occupied by a wall or player. Check out Figure 2 for
    illustrations. All cells along the path of a rush will be revealed.

![](https://github.com/Huzo/Dynamic-Scoping-in-Perl-Maze-Race/blob/master/figure2.png)
Figure 2: The neighborhood of Player 1 usingrush. The green cells are feasible while red cell is
blocked. Best viewed on screen.

- If a player is next to a wall or the other player. This player can choose towalk throughthe
    blocked cell (by the wall/player) to the next empty cell. This special move cannot go through
    more than one level of blocked cell though. On the other hand, if there is no wall/player in
    front of the player, thenwalk through a blocked celldegenerates to a normalmove.

![](https://github.com/Huzo/Dynamic-Scoping-in-Perl-Maze-Race/blob/master/figure3.png)
Figure 3: The neighborhood of Player 1 usingwalk through a blocked cell. The green cells are
feasible while red cell is blocked. Best viewed on screen.

- Influenced by the magic in the maze, players have a choice to be teleported to arandomcell
    within the maze. If the target cell is feasible then this teleportation succeeds. Otherwise, this
    teleportation fails and the player remains in the original cell, but the target cell is revealed
    on the map.

Note Each player can only make special moves at most four times. Moreover, failed move
attempts would not change players’ locations, but they would reveal the cell contents (feasible or


not) on the map if that cell has not been explored yet.

Hint Implement special moves via dynamic scoping.
The basic logic of the game is presented in “mazerace.pl”, which has been provided to you.
Please check the file for more details.

## 2.3 Perl Classes

Please follow the classesMazeRace,Player,Maze,Cell, andPositiondefined below in your Perl
implementation. You are free to add other variables, methods or classes, but cannot modify/delete
ours. Please stick to our naming convention and getter/setter style too. We would start the
program by running:perl maze_race.pl.

```
1.ClassMazeRace
This class realizes a simple game board for hosting the competition between two players.
You have to implement it with the following components:
```
- Instance Variable(s)
    maze
    - A maze instance of classMazedefined below, maintaining maze related informa-
tion. In the following specification, all variables named asmazehave the same meaning
as here unless otherwise specified.
player
- A player instance of classPlayer(defined below) standing for player 1.
player
- A player instance of classPlayer(defined below) standing for player 2.
- Instance Method(s)
    new(file)
    - Instantiate an object of ClassMazeRace, initialize its maze and players’ information
(the name of player 1 is ‘E’ and the name of player 2 is ‘H’) fromfile, and return this
object.fileis a string storing the path of the test file. In the following specification,
all variables named asfilehave the same meaning as here unless otherwise specified.
start()
- Start the maze race.

```
2.ClassPlayer
A class representing a player in the game. You have to implement it with the following
components:
```
- Instance Variable(s)
    name
    - The name of the player.
    curPos
    - The current position of the player. It is an instance of classPosition.
    specialMovesLeft
    - How many move times that the current player can make a special move (no matter
whether this move fails or succeeds). It should be initialized to 4.
rshift
- A one-dimensional array describing the offsets related to this position in row to
stand for a normal move to the S, E, N and W directions,e.g. our $rshift = (1, 0, -1,
0);
cshift


- A one-dimensional array describing the offsets related to this position in column
to stand for a normal move to the S, E, N and W directions,e.g.our $cshift = (0, 1, 0,
-1);
- Instance Method(s)
new(name)
- Instantiate an object of ClassPlayerwith its name, and return this object.
setName(name)
- The setter of this player’s name.
getName()
- The getter of this player’s name.
getPos()
- The getter of this player’s position. It returns an object of classPosition.
occupy(maze)
- Update the cell content as the player moves into the current location in themaze.
leave(maze)
- Update the cell content as the player leaves the current location in themaze.
move(pointTo, maze)
- The player moves to a new location (one step away) according to the givendirection
indicated bypointToin thatmaze. If he/she is blocked on the way, he/she would
remain still. In our game setting,pointTois an integer from 0 to 3 indicating the
compass direction (0: south, 1: east, 2: north, 3: west). In the following specification, all
variables named aspointTohave the same meaning as here unless otherwise specified.
next(pointTo)
- Return the position of the neighbor of the player in the directionpointTo.
rush(pointTo, maze)
- The player rushes to a new location based on the given direction (pointTo) in
thatmaze. The player would stop at where he/she is hindered on the way.
throughBlocked(pointTo, maze)
- The player can walk through one blocked cell when he/she is facing a wall or the
other player and the cell behind the wall/player is available.
teleport(maze)
- The player will be teleported to a random cell in the maze. Note that the target
cell could be in any position of the maze, including this player’s current location, other
player’s location, infeasible cells blocked by walls, available cells, and the location of the
treasure.
makeMove()
- Make the current player move to a position based on the instruction of the human
user. The interaction can be invoked by standard I/O.

3.ClassMaze

```
A class representing a maze in the game. You have to implement it with the following
components:
```
- Instance Variable(s)
    map
    - A two-dimensional array of object of classCellrecording maze information. Ini-
tially, all cells are not explored.
height
- The height of the maze.
width


- The width of the maze.
destPos
- The position of the destination (where we can find the treasure). It is an instance
of classPosition.
- Instance Method(s)
new()
- Instantiate an object of ClassMaze, initialize its instance variables, and return
this object.
getHeight()
- The getter of the height of the maze.
getWidth()
- The getter of the width of the maze.
getCell(pos)
- The getter of the cell of the maze in the given position (pos).posis an instance
of classPosition. In the following specification, all variables named asposhave the
same meaning as here unless otherwise specified.
setCell(pos, cell)
- The setter of the cell of the maze in the given position (pos).cellis an instance
of classCell.
getCellContent(pos)
- The getter of the cell content of the maze in the given position (pos).
setCellContent(pos, value)
- The setter of the cell content of the maze in the given position (pos).
isAvailable(pos)
- Check whether the cell in the given position (pos) is feasible or not. It will return
a -1/0/1 value (-1: infeasible, 0: out of range, 1: feasible).
explore(pos)
- Mark the cell at the given position (pos) as explored.
reachDest(pos)
- Return whether we can find the treasures in the given position (pos) as a 0/
value (0: not found, 1: found).

![](https://github.com/Huzo/Dynamic-Scoping-in-Perl-Maze-Race/blob/master/figure4.png)

```
Figure 4: A visual example about the maze information.
```
```
displayMaze()
```
- Print the maze information to a standard output device (screen). Specifically,
players are represented by the initials of their names (like ‘E’ and ‘H’ by default), and


```
the treasure is denoted by ‘O’. For known cells, ‘’ (one blank) denotes empty cell and
‘#’ denotes wall. For unknown cells, they are all shown as ‘?’. An example is given in
Figure 4.
loadMaze(file)
```
- Load the maze related information from a givenfile(to be explained in further
details).

4.ClassCell

```
A class representing a cell in the maze. You have to implement it with the following compo-
nents:
```
- Instance Variable(s)
    pos
    - The position of this cell. Hereposis an instance of classPositiondefined below.
    content
    - A character representing the content of this cell. ‘#’ denotes wall, and ‘*’ denotes
an empty cell, ‘E’ denotes Player 1, ‘H’ denotes Player 2, and ‘O’ denotes the treasure.
explored
- A 0/1 value indicating whether this cell is explored (1) or not (0).
- Instance Method(s)
    new()
    - Instantiate an object of ClassCell, initialize its instance variables, and return
this object.
setExplored(value)
- The setter of the variableexplored.
getExplored()
- The getter of the variableexplored.
setPos(pos)
- The setter of the variableposwhich is an object of classPosition.
getPos()
- The getter of the variablepos.
setContent(value)
- The setter of the variablecontent.
getContent()
- The getter of the variablecontent.
isAvailable()
- Return whether this cell is available (1) or not (0) as a 0/1 value.

5.ClassPosition

```
A class representing a position in the maze. You have to implement it with the following
components:
```
- Instance Variable(s)
    r
    - The row number of this position.
    c
    - The column number of this position.


- Instance Method(s)
    new()
    - Instantiate an object of ClassPlayer, initialize its instance variables, and return
this object.
getC()
- The getter of the column numberc.
getR()
- The getter of the row numberr.
setC(value)
- The setter of the column numberc.
setR(value)
- The setter of the row numberr.

## 2.4 Execution Context

We provide you with the skeleton code in “mazerace.pl” and “mazeracecomponents.pm”. You
are not allowed to change our code, but can only insert your own code. Your implementation
should be able to run with the main program in “mazerace.pl”.

![](https://github.com/Huzo/Dynamic-Scoping-in-Perl-Maze-Race/blob/master/figure5.png)

```
Figure 5: The input of Player 1.
```
## 2.5 Input/Output Specification

All possible mazes with the positions of the treasures and two players are given in the specification
file. The data format is given below.

- Input

```
H W
#1###2#
```

![](https://github.com/Huzo/Dynamic-Scoping-in-Perl-Maze-Race/blob/master/figure6.png)

```
Figure 6: Check the input for Player 2 and request the input again if that is invalid.
```
```
****#**
##*###*
#******
#*#O##*
```
The first line gives the height (H, where 2≤H≤70) and width (W, where 2≤W≤70) of
the maze. The remaining H lines give the maze information. The detailed maze description is
given in a 2D matrix form, where ‘#’ denotes wall and ‘*’ denotes an empty cell, ‘O’ denotes
treasure, ‘1’ and ‘2’ denote players 1 and 2 respectively. Note ‘O’, ‘1’ and ‘2’ only show up
once in the maze information.

Note each line in the given test file is ended with ‘\n’. Once again, we use ‘*’ to denote an
empty cell in the input file while ‘’ (a blank) is used to show an empty cell on the screen
(Figure 4).

You can assume the input file to be error free.

How to control a Player’s Move Players require user-inputs to determine their next
moves. When it is one player’s turn during the game, your program should request for the
type and direction of the next move. The input should be integers from 0 to 2 for the type
and from 0 to 3 for the direction. When inputting the moving type, hitting ENTER directly
without giving any number will control the current player to make a normal move.

If the target cell is available, the player will move to that cell. If not, the information of the
target cell will be uncovered. An example is presented in Figure 5. After this, the system
switches to the other player’s turn automatically.

Moreover, any invalid input should be forbidden and the user is requested for input again until
a valid move is given. See Figure 6. Note we will also display the number of remaining special
moves that a player can make in each turn. When a player runs out of all his special moves,

![](https://github.com/Huzo/Dynamic-Scoping-in-Perl-Maze-Race/blob/master/figure7.png)

Figure 7: Player 2 used up all his/her special moves, and he/she is defaulted to make a normal
move from now on.

```
this player needs no more user-input for choosing the moving type (the moving direction still
requires interaction) as shown in Figure 7.
If you choose 3 (teleport), then there is no need for direction input.
Your implementation should output exactly the same message as shown in the examples in
the figures.
```
- Output
    Figures 8 and 9 show partial output during the game. Specifically, at the beginning of the
    game, only the locations of Player 1 and 2 and the treasures are visible, and the remaining
    cells are invisible, denoted by ‘?’. No matter whether the movement of a player succeeds or
    fails, the information (replacing ‘?’ with ‘#’, ‘’, ‘E’, or ‘H’) of the target cell will be revealed
    permanently.
    When a player reaches the treasure first, the game will output the information like Figure
    10 and then end.
    Note all the input and output statements are provided to you in the skeleton code. Do not
    modify them in any way.
