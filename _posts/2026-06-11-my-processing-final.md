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
excerpt: "This project is a showcase of my thought process when writing my code step-by-step, documenting my development of a 2D pixel survival game."

<!--more-->

# Introduction
   This project is a showcase of my thought process when writing my code step-by-step; this is acheived through organizing my code content chronologically, as well as describing milestones made each day.

# The Main Idea:

   
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

[insert 2 4x4 tables below];
|         |    |      |    
|---------|----|------|----
|         |    |      |
|         |    |      |
|         |    |      |
|         |    |      |
|         |    |      |


# Day [3], Jun 3, 2026: Design Stage; GUI design
    I worked on a mock GUI design on paper to get a sense of how I wanted different screens to look. I wanted everything to be simple, and I added notes to where screens may change or their general functions.
    Below are all the images, they are only rough sketches:
    [image]
    [image]
    [image]


## Day [4], Jun 4, 2026: Coding Stage; Initializing variables and tile function
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
  
  //Keep player centered
  playerX = width / 2;
  playerY = height / 2;
  
  //replace placeholder textures
  tileImages[0] = loadImage("grass.png"); //CHANGE TO WALL1 WHEN YOU HAVE THE TEXTURES
  tileImages[1] = loadImage("water.png"); //CHANGE TO FLOOR1 WHEN YOU HAVE THE TEXTURES

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
    //  image(placeholder);
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

## Day [5], Jun 5, 2026: Coding Stage; Implementing Sprites and Finishing Textures
  i added variables for sprite images and I may change their names later on as more characters/NPCs receive sprites. most of the period was working on art textures, etc. All textures will be done before monday
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



## Day [6], Jun 8, 2026: Coding Stage; ----
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





## Day [7], Jun 9, 2026: Coding Stage; ----
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








# Interpretation:
## Why summary statistics alone are insufficient
Although the four datasets in Anscombe’s Quartet share nearly identical summary statistics — including mean, variance, correlation, regression line, and R² — their scatterplots tell completely different stories.
Dataset I follows a roughly linear pattern. Dataset II is curved (non-linear). Dataset III has one point far from the trend (an outlier), and Dataset IV is dominated by a single leverage point that determines the regression line.
This shows why relying only on numerical summaries is misleading: visualizing the data reveals structure, outliers, and relationships that summary statistics hide.

# Conclusion and next steps
The Anscombe Quartet demonstrates that datasets with identical statistical summaries can differ dramatically in visual form. This reinforces that summary statistics are only the first step of data analysis — visualization is essential for detecting non-linear patterns and outliers.

Next steps: Interactive dashboards (e.g., Plotly or Altair) could help users explore how regression lines change as individual data points move.
