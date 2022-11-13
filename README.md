# AIRIS - Public Release

AIRIS is an Artificial General Intelligence (AGI) project that combines aspects of Reinforcement Learning (RL) with more traditional symbolic techniques (GOFAI).

[See http://airis-ai.com for videos and additional information.](http://airis-ai.com)

Requires Python 3.x

See "requirements.txt" for additional required packages

By default, the AI is configured to run parallelized on your CPU. You can reconfigure it to run on a CUDA compatible GPU by changing line 123 of other_useful_functions.py to the following:

`@vectorize(['float32(float32, float32)'], target="cuda")`

Regardless of the test environment, AIRIS's learned knowledge is exported as one large python dictionary called "Knowledge.npy" whenever the `save_knowledge()` function is called. If this file exists, it is loaded on the startup of any environment. If this file is deleted or moved from the working directory, AIRIS will "start from scratch" and generate a new one.

###### The Cognitive Architecture of AIRIS 
![AIRIS Cognitive Architecture](https://airisai.files.wordpress.com/2019/01/airis-cognitive-architecture-3.png)

# Example Tests

## puzzle_game_driver.py 
A grid-world puzzle game with various obstacles. The goal is to collect all batteries in a level. There are 13 levels with varying degrees of difficulty and obstacle variety.

By default, this environnment is frame-limited to make it easier for the user to see what is happening. To remove this limitiation and allow the AI to operate at the maximum speed for your hardware, simply comment out line 1293.

`1289: time.sleep(0.10) # control frame rate (in seconds)`

![Puzzle Game Level](https://airisai.files.wordpress.com/2019/01/puzzle-game-level.png)

[Gif of AIRIS Completing the puzzle](https://airisai.files.wordpress.com/2018/03/level.gif)

###### puzzle_game_driver.py additional controls

To enable "view plan" mode, hold the Up Arrow on your keyboard until the console says "Awaiting plan approval..."

You can then use the Left and Right arrow keys to step through each action the AI is planning to perform, and what it expects the result to be.

Press the Space Bar on your keyboard to "approve" the plan and allow the AI to continue.

To disable "view plan" mode, hold the Down Arrow

## puzzle_game_editor.py
This is the level editor for the puzzle game environment. You can use this editor to create your own custom levels for AIRIS to solve. Run puzzle_game_driver_custom.py to use the custom levels.

![Puzzle Game Editor](https://airisai.files.wordpress.com/2019/06/puzzle_game_editor.png)

Left Mouse Button selects an object from the object menu at the top and places a selected object in the level.

Right Mouse Button deletes objects in the level.

The "Clear" button will erase all objects in the level.

A level must have at least 1 robot and 1 battery to unlock the "Play" button. During play mode, arrow keys move and you can either click on the red button at the top or press Spacebar to return to the editor. You will automatically return to the editor when you solve the level.

To unlock "Save" you must play and solve your level.

Levels are saved in \custom_levels in ".csv" format. The name of the level is the level number and is automatically assigned upon opening the editor and is shown in the window title. For example, if there is already "1.csv" and "2.csv" then your new level will be "3.csv".

You can load a level for editing by adding the level number as a command line argument. For example: 

`$ python puzzle_game_editor.py 2` will load level 2 for editing.

You can only save whatever level was created or loaded on startup. To create or load another level, you must exit and restart the editor.

## puzzle_game_driver_custom.py
This is the grid-world puzzle game, but loads the custom levels stored in \custom_levels instead of the original 13.

## puzzle_game_driver_universal.py
This is the grid-world puzzle game with the original 13 levels, but with all AIRIS dependencies stripped out. This environment is for use with any other AI algorithm. 

It is designed to send 2 environmental arrays to an AI: A 20 x 15 grid of the game world filled with the integer IDs that represent the various game objects, and a 1 x 2 grid of integers that represent the number of Keys collected and Fire Extinguishers collected respectively.

    --------------------------------------------
    id   Object
    --------------------------------------------
    0    floor
    1    character
    2    wall
    3    battery
    4    door
    5    key
    6    fire extinguisher
    7    fire
    8    1 way arrow right
    9    1 way arrow left
    10   1 way arrow down
    11   1 way arrow up
    12   open door
    13   player character on top of right arrow
    14   player character on top of left arrow
    15   player character on top of down arrow
    16   player character on top of up arrow
    17   player character in open door
    --------------------------------------------
    (source: game_objects.py)

It expects one of 5 actions to be returned: 'up', 'down', 'left', 'right', or 'nothing'. It then updates the game environnent accordingly, and sends the updated game environment arrays to the AI so that the AI can observe the changes.

By default, it is set to be human controlled with the arrow keys. This can be changed by setting ai_controlled to TRUE on line 1136. Just make sure your AI can get, handle, and return the necessary values (See line 129 and line 157).

## mnist_driver2.py
Number recognition using the MNIST hand-written character dataset

![MNIST Character](https://airisai.files.wordpress.com/2018/03/sprite0_4.png)

## basic_example.py

This is intended to be a simple usage example. It will ask for a word, then ask what the label of that word is. It will try to predict what the label is for new words. Both the word(s) and the label can be anything. In the example below, I used "Name" and "Location" for labels, and some random names and locations I came up with.

```
Enter a word:
    Robert
The AI doesn't yet have knowledge
What is the label for this word?
    Name
This is not the outcome I predicted
Saving new knowledge ...
Enter a word:
    House
The AI thinks the label for this word is:  Name
What is the label for this word?
    Location
This is not the outcome I predicted
Saving new knowledge ...
Enter a word:
    Bill
The AI thinks the label for this word is:  Location
What is the label for this word?
    Name
This is not the outcome I predicted
Saving new knowledge ...
Enter a word:
    Bob
The AI thinks the label for this word is:  Name
What is the label for this word?
    Name
Enter a word:
    Building
The AI thinks the label for this word is:  Name
What is the label for this word?
    Location
This is not the outcome I predicted
Saving new knowledge ...
Enter a word:
    Home
The AI thinks the label for this word is:  Name
What is the label for this word?
    Location
This is not the outcome I predicted
Saving new knowledge ...
Enter a word:
    Housing
The AI thinks the label for this word is:  Location
What is the label for this word?
  Location
Enter a word:
    Apartment
The AI thinks the label for this word is:  Location
What is the label for this word?
    Location
Enter a word:
    John
The AI thinks the label for this word is:  Location
What is the label for this word?
    Name
This is not the outcome I predicted
Saving new knowledge ...
Enter a word:
    Jim
The AI thinks the label for this word is:  Location
What is the label for this word?
    Name
This is not the outcome I predicted
Saving new knowledge ...
Enter a word:
    James
The AI thinks the label for this word is:  Location
What is the label for this word?
    Name
This is not the outcome I predicted
Saving new knowledge ...
Enter a word:
    Joe
The AI thinks the label for this word is:  Name
What is the label for this word?
    Name
Enter a word:
    Studio
The AI thinks the label for this word is:  Name
What is the label for this word?
    Location
This is not the outcome I predicted
Saving new knowledge ...
Enter a word:
    Studio Apartment
The AI thinks the label for this word is:  Location
What is the label for this word?
    Location
Enter a word:
    Office
The AI thinks the label for this word is:  Name
What is the label for this word?
    Location
This is not the outcome I predicted
Saving new knowledge ...
Enter a word:
    Office Building
The AI thinks the label for this word is:  Location
What is the label for this word?
    Location
Enter a word:
```

## Special Thanks to Lucius Dickerson (lucius.dickerson@gmail.com) for helping to teach me Python, fixing my newbie mistakes, and for porting the test environments!
