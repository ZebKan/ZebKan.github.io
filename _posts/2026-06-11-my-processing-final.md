---
layout: post
title: Exploring Processing: Creating My First 2D Pixel Survival Game
subtitle: Working Through Trial and Error, and detailing Mistakes made along the way
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [coding, pixel-game, processingIDE]
author: Zebunnisa Kanji
---

# Introduction

This project documents my step-by-step development process for a pixel-based top-down zombie survival game. Instead of only showing the final result, I organized my work chronologically to highlight how the system was built over time, including key milestones, challenges, and design decisions made throughout development.

The goal of this project is not only to build a functional game, but also to demonstrate my learning process in programming, especially in areas such as tile-based world design, player movement, collision detection, and basic game systems.

---

# Main Idea

The game is a **pixel-based top-down zombie survival game** set in a demolished high-tech base of some sort. The player explores a tile-based world, fights zombies, collects resources, and survives increasingly difficult encounters. 

The world is built using a **tile system loaded from a text file**, where each number represents a different type of block (such as floor, walls, or storage objects). These tiles also determine collision boundaries, meaning the player and zombies cannot pass through walls.

The player character is controlled using keyboard input and includes **animated sprite movement**, giving directional feedback (idle, walking animations, etc.). The camera follows the player and ensures only a portion of the world is visible at any time, rather than showing the full map.

---

## Core Features (Planned / Implemented)

- **Tile-based world system**
  - World is generated from a text file
  - Each number represents a different tile type (floor, wall, storage, etc.)
  - Collision system prevents movement through walls

- **Player system**
  - Keyboard movement (WASD controls)
  - Animated sprites for movement and idle states
  - Health system (survival mechanic)

- **Zombie system**
  - Zombies spawn in specific locations or waves
  - Basic AI that follows the player when in range
  - Attack system that reduces player health on contact
  - Health system for zombies (can be defeated)

- **Camera system**
  - Camera follows player movement
  - Prevents full map visibility to create exploration and tension
  - World moves relative to player position

- **Combat system**
  - Player can shoot projectiles toward the mouse direction
  - Bullets can damage zombies
  - Zombies are removed when health reaches zero

- **Resource and progression systems (planned)**
  - Storage objects (boxes/cabinets) that contain randomized items
  - Currency system earned through zombie kills or exploration
  - Crafting system through NPC interaction (“The Builder”)

- **Base system (planned)**
  - A central safe zone that can be upgraded over time
  - Crafting upgrades such as weapons, ammo, and building materials
  - Acts as progression hub between waves

---

## Non-Functional / Optional Components

These features are not essential for the core gameplay loop, but may be added for depth or future expansion:

- Start screen / menu system (not required for gameplay function)
- Tutorial system (instead handled through an in-game NPC)
- Armor system or advanced equipment mechanics
- Multiple wave variations with scaling difficulty (can be simplified to progressive difficulty instead)
- Complex UI systems beyond basic health and inventory display.
   
# Day [1], Jun 1, 2026: Design Stage; Pseudocode
  This Monday is the first day of my design stage, where I will be finishing my plan for all the features in my zombie game, etc. Below is my pseudocode:
  1. Initialize Variables
  a) PImage variables, playerX, playerY, playerSpeed, boolean variables, etc.
  2. Begin Setup()
  a) Load assets: player character sprites, zombie sprites, tile textures, weapon textures, NPC textures, etc. 
  b) Add BufferedReader and other functions + open txt file.
  3. Main draw() loop
  a) Update game systems through calling methods: zombieFunction(), playerMovement(), healthSystem(), checkCollision(), etc.
  b) Draw game objects/map system: read txt array in a loop by each column and row to generate map textures, etc.
  c) User interface: using text(); draw coin system, buttons, inventory, time until next wave, etc.
  4. Custom methods
  a) Have functions in place for all methods in the method table and more.
  5. Load endScreen()
  a) The game ends with wave 1, giving the player the option to replay the game (replay() - //executed if user wants to replay; endGame () - //executed if user doesn’t want to replay).

# Day [2], Jun 2, 2026: Design Stage; IPO Chart and Method descriptions

## IPO Chart and Method Descriptions

| Method Name        | Description                                                                 | Function |
|--------------------|-----------------------------------------------------------------------------|----------|
| checkCollision(fx, fy, sx, sy…) | Checks whether the player can collide with wall tiles, NPCs, storage, etc. Returns true or false depending on if the collision condition is met. | Prevents movement through solid objects |
| coinSystem()       | Increases coins when zombies are killed or when coin items are collected from storage systems. If zombieDeathStat = true, coins increase by X amount. Also updates UI display. | Handles coin collection and scoring |
| tileRead()         | Loads the map using a grid of numbers representing tile types (floor, wall, etc). Uses BufferedReader to open and read map.txt with loops. | Generates game world from text file |
| zombieFunction()   | Controls zombie AI including movement, attack behavior, and health. Zombies detect player within a radius and become aggressive. Dead zombies are removed. | Manages zombie behavior and interactions |
| playerMovement()   | Handles WASD movement, sprite direction changes, and movement animation. Uses keyPressed for input detection. Updates player speed and direction. | Controls player movement and animation |
| weaponAttack()     | Called via mousePressed. Handles shooting system where bullets follow mouse direction and deal damage on collision with zombies. | Handles shooting and combat damage |

# Day [3], Jun 3, 2026: Design Stage; GUI design
I worked on a mock GUI design on paper to get a sense of how I wanted different screens to look. I wanted everything to be simple, and I added notes to where screens may change or their general functions.
    Below are all the images, they are only rough sketches.
<p align="center">
  <img src="/assets/img/gui-sample-1.png" width="350">
  <img src="/assets/img/gui-sample-2.png" width="350">
</p>

<p align="center">
  <img src="/assets/img/gui-sample-3.png" width="350">
  <img src="/assets/img/gui-sample-4.png" width="350">
</p>

<p align="center">
  <img src="/assets/img/gui-sample-5.png" width="350">
  <img src="/assets/img/gui-sample-6.png" width="350">
</p>


## Day [4], June 4, 2026: Coding Stage — Initializing Variables and Tile Function

Today marked the transition from planning into actual implementation. I began constructing the foundation of the game by setting up the core variables and creating the tile system responsible for generating the world. Instead of manually placing each object, the map is loaded from a text file where different numbers represent different tile types. This approach makes the world much easier to modify and expand in the future, as changing the map only requires editing the text file rather than rewriting code.

I also implemented the camera and movement system. Rather than moving the player across the screen, the player remains centered while the camera shifts the visible portion of the world around them. Although placeholder textures and a simple circle were used at this stage, this established the framework upon which future systems such as collisions, sprites, and zombie AI would be built.

```java
///Game map variables
PImage[] tileImages = new PImage[2];
int[][] map;
int tileSize = 128;
int cols;
int rows;

// Camera variables
float camX = 0;
float camY = 0;

// Player variables
int playerX;
int playerY;
int playerSpeed = 6;

boolean up, down, left, right;

void setup() {
  size(1000, 800);

  // Keep player centered
  playerX = width / 2;
  playerY = height / 2;

  // Replace placeholder textures
  tileImages[0] = loadImage("grass.png");
  tileImages[1] = loadImage("water.png");

  String[] lines = loadStrings("map.txt");

  rows = lines.length;

  String[] firstRow = split(trim(lines[0]), " ");
  cols = firstRow.length;

  map = new int[rows][cols];

  // Nested FOR loop to identify number of rows and columns
  for (int y = 0; y < rows; y++) {
    String[] values = split(trim(lines[y]), " ");

    for (int x = 0; x < cols; x++) {
      map[y][x] = int(values[x]);
    }
  }
}
void draw() {
  background(0);
  updateCamera();

  // Load tiles
  for (int y = 0; y < rows; y++) {
    for (int x = 0; x < cols; x++) {

      int tileType = map[y][x];
      // image(placeholder);
    }
  }

  // Placeholder player (will have sprites)
  fill(255);
  noStroke();
  ellipse(playerX, playerY, 40, 40);
}
void updateCamera() {
  // Camera function shifts visible tiles shown
  if (up) camY -= playerSpeed;
  if (down) camY += playerSpeed;
  if (left) camX -= playerSpeed;
  if (right) camX += playerSpeed;
}
void keyPressed() {
  // Include upper and lowercase to avoid error
  if (key == 'w' || key == 'W') up = true;
  if (key == 's' || key == 'S') down = true;
  if (key == 'a' || key == 'A') left = true;
  if (key == 'd' || key == 'D') right = true;
}

void keyReleased() {
  // Include upper and lowercase to avoid error
  if (key == 'w' || key == 'W') up = false;
  if (key == 's' || key == 'S') down = false;
  if (key == 'a' || key == 'A') left = false;
  if (key == 'd' || key == 'D') right = false;
}
```

## Day [5], Jun 5, 2026: Coding Stage; Implementing Sprites and Finishing Textures
  I added variables for sprite images and I may change their names later on as more characters/NPCs receive sprites. Most of the period was working on art textures, etc. All textures were completed on the weekend and monday.
```java
//Game map variables
PImage[] tileImages = new PImage[2];
int[][] map;
int tileSize = 128;
int cols;
int rows;

//PLAYER SPRITE VARIABLES
PImage upIdle, upWalk1, upWalk2;
PImage downIdle, downWalk1, downWalk2;
PImage leftIdle, leftWalk1, leftWalk2;
PImage rightIdle, rightWalk1, rightWalk2;
PImage currentSprite;

// Camera variables
float camX = 0;
float camY = 0;

// Player variables
int playerX;
int playerY;
int playerSpeed = 6;

boolean up, down, left, right;

void setup() {
  size(1000, 800);
  
  //Keep player centered
  playerX = width / 2;
  playerY = height / 2;
  
  //replace placeholder textures
  tileImages[0] = loadImage("grass.png"); //CHANGE TO WALL1 WHEN YOU HAVE THE TEXTURES
  tileImages[1] = loadImage("water.png"); //CHANGE TO FLOOR1 WHEN YOU HAVE THE TEXTURES
  //SPRITE IMAGES
  upIdle = loadImage("up_idle.png");
  upWalk1 = loadImage("up_walk1.png");
  upWalk2 = loadImage("up_walk2.png");
  
  downIdle = loadImage("down_idle.png");
  downWalk1 = loadImage("down_walk1.png");
  downWalk2 = loadImage("down_walk2.png");
  
  leftIdle = loadImage("left_idle.png");
  leftWalk1 = loadImage("left_walk1.png");
  leftWalk2 = loadImage("left_walk2.png");
  
  rightIdle = loadImage("right_idle.png");
  rightWalk1 = loadImage("right_walk1.png");
  rightWalk2 = loadImage("right_walk2.png");
  
  currentSprite = downIdle;
  String[] lines = loadStrings("map.txt");
  rows = lines.length;
  String[] firstRow = split(trim(lines[0]), " ");
  cols = firstRow.length;
  map = new int[rows][cols];
  
  //nested FOR loop to identify number of rows and columns
  for (int y = 0; y < rows; y++) {
    String[] values = split(trim(lines[y]), " ");

    for (int x = 0; x < cols; x++) {
      map[y][x] = int(values[x]);
    }
  }
}
void draw() {
  background(0);
  updateCamera();

  // Load tiles
  for (int y = 0; y < rows; y++) {
    for (int x = 0; x < cols; x++) {

      int tileType = map[y][x];
      image(tileImages[tileType], x * tileSize - camX, y * tileSize - camY, tileSize, tileSize);
    }
  }
  // Placeholder player(will have sprites)
  fill(255);
  noStroke();
  ellipse(playerX, playerY, 40, 40);
}

void updateCamera() { //camera functions shifts visible tiles shown 
  if (up) camY -= playerSpeed;
  if (down) camY += playerSpeed;
  if (left) camX -= playerSpeed;
  if (right) camX += playerSpeed;
}

void keyPressed() { //include upper and lowercase to avoid error
  if (key == 'w' || key == 'W') up = true;
  if (key == 's' || key == 'S') down = true;
  if (key == 'a' || key == 'A') left = true;
  if (key == 'd' || key == 'D') right = true;
}

void keyReleased() { //include upper and lowercase to avoid error
  if (key == 'w' || key == 'W') up = false;
  if (key == 's' || key == 'S') down = false;
  if (key == 'a' || key == 'A') left = false;
  if (key == 'd' || key == 'D') right = false;
}
```


## Day [6], Jun 8, 2026: Coding Stage; Implementing Player Sprites and Movement
  On this day, I began replacing the placeholder ellipse with actual sprite animations. I loaded idle and walking sprites for different directions and started building a basic animation system that changes the player's appearance depending on movement inputs. I also connected the movement logic with the camera system so that the world/tiles move around the player while keeping them centered on the screen. However, the animation system was still incomplete and used placeholder logic in some areas.

```java
///Game map variables
PImage[] tileImages = new PImage[2];
int[][] map;
int tileSize = 128;
int cols;
int rows;

//PLAYER SPRITE VARIABLES
PImage upIdle, upW1, upW2;
PImage downIdle, downW1, downW2;
PImage currentSprite;

// Camera variables
float camX = 0;
float camY = 0;

// Player variables
int playerX;
int playerY;
int playerSpeed = 6;

//Movement variables 
int animTimer; //switches between sprites during movement
boolean up, down, left, right;
boolean move;

void setup() {
  size(1000, 800);
  
  //Keep player centered
  playerX = width / 2;
  playerY = height / 2;
  
  //replace placeholder textures
  tileImages[0] = loadImage("grass.png"); //CHANGE TO WALL1 WHEN YOU HAVE THE TEXTURES
  tileImages[1] = loadImage("water.png"); //CHANGE TO FLOOR1 WHEN YOU HAVE THE TEXTURES
  //SPRITE IMAGES
  upIdle = loadImage("up_idle.png");
  upW1 = loadImage("up_walk1.png");
  upW2 = loadImage("up_walk2.png");
  
  downIdle = loadImage("down_idle.png");
  downW1 = loadImage("down_walk1.png");
  downW2 = loadImage("down_walk2.png");
  currentSprite = downIdle;
  
  String[] lines = loadStrings("map.txt");
  rows = lines.length;
  String[] firstRow = split(trim(lines[0]), " ");
  cols = firstRow.length;
  map = new int[rows][cols];
  
  //nested FOR loop to identify number of rows and columns
  for (int y = 0; y < rows; y++) {
    String[] values = split(trim(lines[y]), " ");
    for (int x = 0; x < cols; x++) {
      map[y][x] = int(values[x]);
    }
  }
}

void draw() {
  background(0);
  updateCamera();
  playerMovement();
  
  // Load tiles
  for (int y = 0; y < rows; y++) {
    for (int x = 0; x < cols; x++) {
      int tileType = map[y][x];
      image(tileImages[tileType], x * tileSize - camX, y * tileSize - camY, tileSize, tileSize);
    }
  }

  // Placeholder player(will have sprites)
  fill(255);
  noStroke();
  ellipse(playerX, playerY, 40, 40);
}

void updateCamera() { //camera functions shifts visible tiles shown 
  if (up) camY -= playerSpeed;
  if (down) camY += playerSpeed;
  if (left) camX -= playerSpeed;
  if (right) camX += playerSpeed;
}

void keyPressed() { //include upper and lowercase to avoid error
  if (key == 'w' || key == 'W') up = true;
  if (key == 's' || key == 'S') down = true;
  if (key == 'a' || key == 'A') left = true;
  if (key == 'd' || key == 'D') right = true;
}

void keyReleased() { //include upper and lowercase to avoid error
  if (key == 'w' || key == 'W') up = false;
  if (key == 's' || key == 'S') down = false;
  if (key == 'a' || key == 'A') left = false;
  if (key == 'd' || key == 'D') right = false;
}

//Sprite changes for player character
void playerMovement(){
  if(move != true){
  downIdle(playerX, playerY, 40, 40);
  }
  if (up == true){
    move = true;
  }
}
```




## Day [7], Jun 9, 2026: Coding Stage; Expanding Core Game Systems
  On this day, I expanded the project beyond basic movement and map rendering by beginning work on several of the game's major systems. I implemented additional tile textures, improved the player's walking animation, and created the first prototype of zombie behaviour using an aggro radius and placeholder pursuit logic. I also planned out future gameplay mechanics that unfortunately never made their way to the deadline.
  ```java
///Game map variables
PImage[] tileImages = new PImage[6];
int[][] map1;
int tileSize = 105;
int cols;
int rows;

//SPRITE VARIABLES
PImage downIdle, downW1, downW2;
PImage currentSprite;
int animationTimer = 0;
PImage currentZombieSprite;
PImage zombieR, zombieL; //create array for zombies later on
PImage NPCIdle;


// Camera variables
float camX = 0;
float camY = 0;

// Player variables
int playerX;
int playerY;
int playerSpeed = 6;

boolean up, down, left, right;

void setup() {
  size(1000, 800);
  
  //Keep player centered
  playerX = width / 2;
  playerY = height / 2;
  
  //load tile textures
  tileImages[0] = loadImage("floor1.png");
  tileImages[1] = loadImage("floor2.png");
  tileImages[2] = loadImage("wall.png");
  tileImages[3] = loadImage("wallShade.png");
  tileImages[4] = loadImage("storage.png");
  tileImages[5] = loadImage("blank.png");
  //SPRITE IMAGES: Player, zombie, NPC
  downIdle = loadImage("downIdle.png");
  downW1 = loadImage("downW1.png");
  downW2 = loadImage("downW2.png");
  currentSprite = downIdle;
  currentZombieSprite = zombieR;
  zombieR = loadImage("zombieR.png");
  zombieL = loadImage("zombieL.png");
  NPCIdle = loadImage("NPCIdle.png");
  
  String[] lines = loadStrings("map1.txt.txt");

  rows = lines.length;

  String[] firstRow = split(trim(lines[0]), " ");
  cols = firstRow.length;

  map1 = new int[rows][cols];
  
  //nested FOR loop to identify number of rows and columns
  for (int y = 0; y < rows; y++) {
    String[] values = split(trim(lines[y]), " ");

    for (int x = 0; x < cols; x++) {
      map1[y][x] = int(values[x]);
    }
  }
}

void draw() {
  background(0);
  updateCamera();
  zomebieFunction();
  // Load tiles
  for (int y = 0; y < rows; y++) {
    for (int x = 0; x < cols; x++) {

      int tileType = map1[y][x];

      if (tileType >= 0) {
        image(
          tileImages[tileType],
          x * tileSize - camX,
          y * tileSize - camY,
          tileSize,
          tileSize);
      }
    }
  }
   // player(will have sprites)
  image(currentSprite, playerX-80, playerY-80,160,160); //size him bigger
}

void updateCamera() { //camera functions shifts visible tiles shown 
  if (up) camY -= playerSpeed;
  if (down) camY += playerSpeed;
  if (left) camX -= playerSpeed;
  if (right) camX += playerSpeed;
  boolean moving = up || down || left || right;
  if (moving) {
    animationTimer++;
    if ((animationTimer / 10) % 2 == 0) { //switches between 2 sprites during movement
      currentSprite = downW1;
    } else {
      currentSprite = downW2;
    }
  } else {
    currentSprite = downIdle;
  }
}

void keyPressed() { //include upper and lowercase to avoid error
  if (key == 'w' || key == 'W') up = true;
  if (key == 's' || key == 'S') down = true;
  if (key == 'a' || key == 'A') left = true;
  if (key == 'd' || key == 'D') right = true;
}

void keyReleased() { //include upper and lowercase to avoid error
  if (key == 'w' || key == 'W') up = false;
  if (key == 's' || key == 'S') down = false;
  if (key == 'a' || key == 'A') left = false;
  if (key == 'd' || key == 'D') right = false;
}

void zomebieFunction(){
  float worldX = tileSize + playerX;
  float worldY = tileSize + playerX;
  int zombieX = 500; 
  int zombieY = 400;
  int zombieSpeed = 2;
  image(zombieR, zombieX, zombieY);
  float distance = dist(zombieX, zombieY, worldX, worldY);
  int aggroRadius = 400;
  if (distance > aggroRadius){
    //PLACEHOLDERS:
     //check direction of player to zombie using playerX/Y - zombie X/Y
     //update zombie speed in that direction
     //damage dealt is 0.5 health per hit
  }
}

//*boolean checkCollision(float X, float Y){
//NPC that explains tutorial and runs the shop
void theBuilder(){
  image(NPCIdle, 100, 100, 160,160); //placeholder size/location
  shopSystem();
}

void shopSystem(){
  //rect and text function to open shop menu
}

void inventorySystem(){
    if (key == 'i' || key == 'I') {
      //open inventory menu with texts displaying inventory items.
    } 
}
void storageSystem(){
  //check tile if it equals storage unit [5]
  //randomize items in storage unts
}
```
## Day [8], Jun 10, 2026: Final Coding Day
  On this final coding day, I integrated most of the remaining core gameplay systems to make my game somewhat of a playable experience. I implemented a zombie combat system where each zombie has individual health and speed values and can be defeated using projectiles fired by the player. I also added a player health system where zombies deal periodic damage upon contact (1 attack per 60 frames), requiring the player to avoid close-range encounters.
Finally, I introduced a game over state that triggers when the player's health reaches zero.

```java
///Game map variables
PImage[] tileImages = new PImage[6];
int[][] map1;
int tileSize = 105;
int cols;
int rows;

//SPRITE VARIABLES
PImage downIdle, downW1, downW2;
PImage currentSprite;
int animationTimer = 0;
PImage zombieR, zombieL; //create array for zombies later on
PImage currentZombieSprite;

//zombie variables
float[] zombieX;
float[] zombieY;
boolean[] zombieAggro;
PImage[] zombieSprite;
int[] zombieHealth;
int zombieCount = 10; //represents the number in each array
float[] zombieSpeed;
int aggroRadius = 300;
float zombieDamage = 0.5;

// Camera variables
float camX = 0;
float camY = 0;

// Player variables
int playerX;
int playerY;
int playerSpeed = 6;
float playerHealth = 3;

boolean up, down, left, right;
int damageTimer = 0;
float bulletX, bulletY;
float bulletSpeed = 10;
boolean shooting = false;
boolean gameOver;
float dirX, dirY;


void setup() {
  size(1000, 800);
  PFont pixelF;
  pixelF = createFont("Pixellettersfull-BnJ5.ttf", 200);
  textFont(pixelF);
  //Keep player centered
  playerX = width / 2;
  playerY = height / 2;
  
  //zombie array values
  zombieR = loadImage("zombieR.png");
  zombieL = loadImage("zombieL.png");
  currentZombieSprite = zombieR;
  zombieX = new float[zombieCount];
  zombieY = new float[zombieCount];
  zombieAggro = new boolean[zombieCount];
  zombieSprite = new PImage[zombieCount];
  zombieSpeed = new float[zombieCount];
  
  //zombie spawn positions
  zombieX[0] = 1450;
  zombieY[0] = 700;
  
  zombieX[1] = 1450;
  zombieY[1] = 800;
  
  zombieX[2] = 2000;
  zombieY[2] = 767;
  
  zombieX[3] = 2200;
  zombieY[3] = 1500;
  
  zombieX[4] = 1000;
  zombieY[4] = 2000;
  
  zombieX[5] = 750;
  zombieY[5] = 2000;
  
  zombieX[6] = 750;
  zombieY[6] = 3000;
  
  zombieX[7] = 3300;
  zombieY[7] = 700;
  
  zombieX[8] = 3000;
  zombieY[8] = 3000;
  
  zombieX[9] = 2800;
  zombieY[9] = 2500;
  
  // default sprites
  zombieHealth = new int[zombieCount];
  for (int i = 0; i < zombieCount; i++) {
    zombieSprite[i] = zombieR;
    zombieHealth[i] = 5;
    zombieSpeed[i] = random (2,5);
  }
    
  
  //load tile textures
  tileImages[0] = loadImage("floor1.png");
  tileImages[1] = loadImage("floor2.png");
  tileImages[2] = loadImage("wall.png");
  tileImages[3] = loadImage("wallShade.png");
  tileImages[4] = loadImage("storage.png");
  tileImages[5] = loadImage("blank.png");
  //Sprite images: Player, zombie, NPC
  downIdle = loadImage("downIdle.png");
  downW1 = loadImage("downW1.png");
  downW2 = loadImage("downW2.png");
  currentSprite = downIdle;
  
  String[] lines = loadStrings("map1.txt.txt");
  rows = lines.length;
  String[] firstRow = split(trim(lines[0]), " ");
  cols = firstRow.length;
  map1 = new int[rows][cols];
  //nested FOR loop to identify number of rows and columns
  for (int y = 0; y < rows; y++) {
    String[] values = split(trim(lines[y]), " ");

    for (int x = 0; x < cols; x++) {
      map1[y][x] = int(values[x]);
    }
  }
}

//Draw function
void draw() {
  //gameover
  if (gameOver==true) {
    text("Game Over", width/2, 120);
    fill(0);
    rect(400, 400, 200, 80); 
    fill(255);
    text("Replay?", 450, 450);
    if (mouseX > 400 && mouseX < 600 && mouseY > 400 && mouseY < 480) {
      if (mousePressed == true) {
        resetGame();
      }
     }
      return;
  }
  //general
  damageTimer++;
  background(0);
  updateCamera();
  // Load tiles
  for (int y = 0; y < rows; y++) {
    for (int x = 0; x < cols; x++) {
      int tileType = map1[y][x];
      if (tileType >= 0) {
        image(
          tileImages[tileType],
          x * tileSize - camX,
          y * tileSize - camY,
          tileSize,
          tileSize);
      }
    }
  }
   zomebieFunction();
   // player(will have sprites)
  image(currentSprite, playerX-80, playerY-80,160,160); //size him bigger
  updateBullet();
  fill(255);
  textSize(30);
  text("Health: " + playerHealth, width - 180, 40);
}

void updateCamera() { //camera functions shifts visible tiles shown 
  boolean moving = up || down || left || right;
   if (up) {
    if (!checkCollision(camX + playerX, camY + playerY - playerSpeed)) {
      camY -= playerSpeed;
      moving = true;
    }
  }
  if (down) {
    if (!checkCollision(camX + playerX, camY + playerY + playerSpeed)) {
      camY += playerSpeed;
      moving = true;
    }
  }
  if (left) {
    if (!checkCollision(camX + playerX - playerSpeed, camY + playerY)) {
      camX -= playerSpeed;
      moving = true;
    }
  }
  if (right) {
    if (!checkCollision(camX + playerX + playerSpeed, camY + playerY)) {
      camX += playerSpeed;
      moving = true;
    }
  }

  if (moving) {
    animationTimer++;
    if ((animationTimer / 10) % 2 == 0) { //switches between 2 sprites during movement
      currentSprite = downW1;
    } else {
      currentSprite = downW2;
    }
  } else {
    currentSprite = downIdle;
  }
}

void keyPressed() { //include upper and lowercase to avoid error
  if (key == 'w' || key == 'W') up = true;
  if (key == 's' || key == 'S') down = true;
  if (key == 'a' || key == 'A') left = true;
  if (key == 'd' || key == 'D') right = true;
}

void keyReleased() { //include upper and lowercase to avoid error
  if (key == 'w' || key == 'W') up = false;
  if (key == 's' || key == 'S') down = false;
  if (key == 'a' || key == 'A') left = false;
  if (key == 'd' || key == 'D') right = false;
}

void zomebieFunction() {

  float playerWorldX = camX + playerX;
  float playerWorldY = camY + playerY;  
  for (int i = 0; i < zombieCount; i++) {
    float distance = dist(
      zombieX[i], zombieY[i],
      playerWorldX, playerWorldY);
      
    if (distance < aggroRadius) {
      zombieAggro[i] = true;
      if (distance < 40 && damageTimer >= 60) {
      playerHealth -= zombieDamage;
      damageTimer = 0;
      if (playerHealth <= 0) {
        gameOver = true;
      }
      break;
    }
   }

    if (zombieAggro[i]) {
      if (playerWorldX > zombieX[i]) {
        zombieX[i] += zombieSpeed[i];
        zombieSprite[i] = zombieR;
      }

      if (playerWorldX < zombieX[i]) {
        zombieX[i] -= zombieSpeed[i];
        zombieSprite[i] = zombieL;
      }

      if (playerWorldY > zombieY[i]) {
        zombieY[i] += zombieSpeed[i];
      }

      if (playerWorldY < zombieY[i]) {
        zombieY[i] -= zombieSpeed[i];
      }
    }

    image(zombieSprite[i],zombieX[i] - camX,zombieY[i] - camY,160,160);
  }
}

boolean checkCollision(float playerWorldX, float playerWorldY) {
  int tileX = int(playerWorldX / tileSize);
  int tileY = int(playerWorldY / tileSize);

  if (tileX < 0 || tileY < 0 || tileX >= cols || tileY >= rows) {
    return true;
  }
  int tile = map1[tileY][tileX];
  return (tile == 2 || tile == 3); //2 and 3 are wall tiles.
}

void mousePressed() {
  if (gameOver) {
    text("Game Over", width/2, 120);
    text("Replay?", 400, 400);
    if (mouseX > 400 && mouseX < 600 && mouseY > 400 && mouseY < 480) {
      resetGame();
    } return;
  }
  bulletX = playerX;
  bulletY = playerY;
  float dx = mouseX - playerX;
  float dy = mouseY - playerY;
  dirX = dx / 50;
  dirY = dy / 50;
  shooting = true;
}
//Projectile Function
void updateBullet() {

  if (shooting) {
    bulletX += dirX * bulletSpeed;
    bulletY += dirY * bulletSpeed;

    ellipse(bulletX, bulletY, 20, 20);
    float bulletWorldX = bulletX + camX;
    float bulletWorldY = bulletY + camY;
    
    for (int i = 0; i < zombieCount; i++) {
      float distance = dist(
        bulletWorldX,
        bulletWorldY,
        zombieX[i] + 80,
        zombieY[i] + 80  );

      if (distance < 40) {
        zombieHealth[i] -= 1;
        if (zombieHealth[i] <= 0) {
          zombieX[i] = -1000;
          zombieY[i] = -1000;
        }
        shooting = false;
        bulletX = -1000;
        bulletY = -1000;
        return;
      }
    }
  }
}

//Replay method
void resetGame() {

  playerHealth = 3;
  gameOver = false;
  camX = 0;
  camY = 0;
  shooting = false;
  //zombie placements
  zombieX[0] = 1450;
  zombieY[0] = 700;
  
  zombieX[1] = 1450;
  zombieY[1] = 800;
  
  zombieX[2] = 2000;
  zombieY[2] = 767;
  
  zombieX[3] = 2200;
  zombieY[3] = 1500;
  
  zombieX[4] = 1000;
  zombieY[4] = 2000;
  
  zombieX[5] = 750;
  zombieY[5] = 2000;
  
  zombieX[6] = 750;
  zombieY[6] = 3000;
  
  zombieX[7] = 3300;
  zombieY[7] = 700;
  
  zombieX[8] = 3000;
  zombieY[8] = 3000;
  
  zombieX[9] = 2800;
  zombieY[9] = 2500;

  for (int i = 0; i < zombieCount; i++) {
    zombieHealth[i] = 5;
    zombieAggro[i] = false;
    zombieSprite[i] = zombieR;
  }
}
```

## Reflection

When the main idea of my game came to me , it was far more detailed than what I have now. The true playable experience I wanted to acheive was to allow players to progress by collecting items found in the map (using brown shelf systems and inventory systems), killing zombies and earning coins, and using shop systems; the player should have been able to reach a final wave stage where thet defend against zombies and repair parts of the base to block or trap them. Unfortuantely, due to a combination of poor time management, lack of experience (i.e, a lot of debugging) and a group consisting of just myself, I was unable to acheive this vision given the time frame. A large chunk of my time was also spent on figuring out how to properly implement the tile system, and so if I had spent more time practicing at home, I feel as though I could've been perfectly capable of attaining all my goals. Overall, I am proud of the work I've done.
