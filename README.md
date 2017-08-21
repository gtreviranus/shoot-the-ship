# Shoot the Ship  
Alright so this is the current spec I worked out and how I think we should organize the program more or less. The basic idea is that we use a so-called "State Machine" to manage our games various states. A "state" is any condition in which the UI is majorly different from any other. For example, the main menu will likely be one state and the game play (the actual pew-pew-pew) will be another. I'm a little shaky on how exactly we want to organize these states and what states we need to have to make a successful game, but the basic idea is that by hard coding the properties of these states and managing the transitions between these different states we can get a functional user experience. Definitely going to see if I can find any theory on design practices for this stuff.

This game will have a lot of moving parts in it. To summarize, here are some of the tasks to accomplish moving forward.
## Tasks
1. Basic rendering of sprites from sprite sheets to create enemies, bullets, and a PC
2. Menus and game loop with organized hierarchy
3. Responsively handle input and perform satisfactory motion and collision detection (i.e. make a nice-ish physics engine)
4. Score, lives, collision, etc., create a set of general game mechanics

## TODO Next:
* Start working on LazyFoo Tutorials for SDL 2.0. There are some other tutorials out there too (I found one for brick breaker at one point).
* Read some theory on design

# StateMachine.h
The goal of this class is to manage the state the game is in and transition the game from one state to the next. This might be a spurious class with all the main loop. The idea is that the exit code of one state would tell the system to start a new state.

# State.h
The basic idea of this class is to create a series of children that represent all the UI patterns that can come up in the game. By presetting a series of exit codes that the states can leave with, each state will effectively create the graph that maps all the states together. I'm not sure how best to accomplish some of the menu stuff and where to store some of the game data, because we might want one state to effect stuff external to it. This could simply be a matter of using globals (which sounds like a dangerous idea) or passing around arguments. It might also be accomplished with sub-states (that will likely need to be researched for more sophisticated games, but easy to hack together for a simple one).
## Polymorphic Child Classes:
* **Menu**: menu that handles rendering and navigation of the menu. When loading the game, you will actually be allocating and building the menu.
* **Game**: the actual game itself, handling UI, and game objects, and rendering, and everything that makes the game a game.

# GameObject.h
This class will have a high degree of polymorphism (same basic functionality, implemented differently based on the type of object). Everything we want to render on the screen will have a game object wrapper class that will handle it's rules for movement, rendering, and collision detection. Our game is basically having the user shoot bullets (one game object) from a ship (a second game object) at enemy ships (a third game object) that shoot bullets back. Based on the motion of these objects and whether or not they collide, determines whether or not an interaction occurs (i.e. game over or the enemy is destroyed). Anyway here is the basic structure:
## Fields
* **Texture**: the thing that actually gets rendered to the screen at the prescibed position
* **Position**: where to render the object
* **Velocity**: the speed the object is moving at depending on our physics (rotation adds a whole new aspect to this)
* **Acceleration?**: (Maybe depending on our physics)
## Functions
* **Move**: update the position of the object based on the velocity, note velocity can simply be an on/off switch for the player ship and might be set to a constant for bullets and enemies
* **Render**: self-explanatory, draw this object here
* **Collide**: depending on how this is done, we might need to do different pairs of hit box type collisions (e.g. circle to circle, circle to rectangle, rectangle to rectangle, which just two hit box types)
## Polymorphic Child Classes:
* Ship
* Enemy
* Bullet

# Other classes
* **Texture wrapper class**: This will handle all the opengl/sdl stuff for rendering the texture. The idea is that when we render a game object, we just want to call the render method of the exture stored in the object. It might get a little more complicated than that and this might not be the best way to do it. Hurray for game dev theory on some of this stuff.
* **Text wrapper class**: This will handle rendering text to the screen. The idea is that a menu will simply be a collection of these guys with some sort of cursor to indicate where you are.

# Stuff that will need to be hard coded somewhere
* Clipping sprites, i.e. a sprite mapping of some kind. Whether they are individual files or sheets that need to be mapped out.
* Menu lay outs and options
* State transition
* Physics engine stuff
* Text options and game controls

This is all I could think of for now, if you have any questions about any of this we can sit and hash it out. I definitely need to read some more game design theory to really understand how we will want to put together all the pieces to make something functional. I'm not super certain how the back bone of some of these games works, but I'd be excited to fiddle around with it.
