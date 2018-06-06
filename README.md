# Project-Hoppy-Beaver-Extreme

var currentScene = 1;
    
var Beaver = function(x, y) {
    this.x = x;
    this.y = y;
    this.img = getImage("creatures/Hopper-Happy");
    this.sticks = 0;
    this.holes = 0;
    this.npcs = 0;
};

var Stick = function(x, y) {
    this.x = x;
    this.y = y;
};

var Hole = function(x, y) {
    this.x = x;
    this.y = y;
};

var Npc = function(x, y) {
    this.x = x;
    this.y = y;
    this.img = getImage("avatars/duskpin-ultimate");
};

Beaver.prototype.draw = function() {
    this.y = constrain(this.y, 0, height-50);
    image(this.img, this.x, this.y, 40, 40);
};
    
Beaver.prototype.hop = function() {
    this.img = getImage("creatures/Hopper-Jumping");
    this.y -= 5;
};
    
Beaver.prototype.fall = function() {
    this.img = getImage("creatures/Hopper-Happy");
    this.y += 5;
};
    
Beaver.prototype.checkForStickGrab = function(stick) {
    if ((stick.x >= this.x && stick.x <= (this.x + 40)) &&
        (stick.y >= this.y && stick.y <= (this.y + 40))) {
        stick.y = -400;
        this.sticks++;
        }
};
    
Beaver.prototype.checkForHoleAvoid = function(hole) {
    if ((hole.x >= this.x && hole.x <= (this.x + 40)) &&
        (hole.y >= this.y && hole.y <= (this.y + 40))) {
        hole.y = -400;
        this.holes++;
        }
};
    
Beaver.prototype.checkForNpcAvoid = function(npc) {
    if ((npc.x >= this.x && npc.x <= (this.x + 40)) &&
        (npc.y >= this.y && npc.y <= (this.y + 40))) {
        //image(this.img, this.x, this.y, -400, -400);
        this.npcs++;
        }
};
    
Stick.prototype.draw = function() {
    fill(89, 71, 0);
    rectMode(CENTER);
    rect(this.x, this.y, 5, 40);
};
    
Hole.prototype.draw = function() {
    fill(5, 5, 5);
    ellipseMode(CENTER);
    ellipse(this.x, this.y, 10 , 10); 
};

Npc.prototype.draw = function() {
    fill(255, 0, 0);
    image(this.img, this.x, this.y, 40, 40);
};
    
var beaver = new Beaver(200, 300);
/*var stick = new Stick(0, 0, 5, 40);
var hole = new Hole(0, 0, 10, 10);*/
    
var sticks = [];
for (var i = 0; i < 10; i++) {  
    sticks.push(new Stick(i * 40 + 300, random(20, 260)));
    }
    
var holes = [];
for (var i = 0; i < 5; i++) {  
    holes.push(new Hole(i * 40 + 300, random(20, 260)));
    }

var npcs = [];
for (var i = 0; i < 3; i++) {  
    npcs.push(new Npc(i * 40 + 300, random(20, 260)));
    }

var grassXs = [];
for (var i = 0; i < 25; i++) { 
    grassXs.push(i*20);
    }

draw = function() {
    if (currentScene === 1){
        rectMode(CORNER);
        imageMode(CENTER);
        background(0, 221, 255);
        fill(0, 0, 0);
        textSize(20);
        text("Press space to make Hopper move", 10, 30);
        image(getImage("creatures/Hopper-Happy"), 100, 100);
        fill(81, 166, 31);
        rect(200, 200, 100, 25);
        fill(255, 255, 255);
        textSize(16);
        text("Press Button", 205, 220);
        fill(0, 0, 0);
        textSize(20);
        text("Grab sticks and avoid holes!!!", 10, 300);
        if (mouseIsPressed&& mouseX >= 200 && mouseX <=300 && mouseY >=200 && mouseY <=225){
        currentScene = 2;
        }
    }
    
    // LEVEL 1
    else if (currentScene === 2){
        var gameOver = true;
        background(227, 254, 255);
        fill(130, 79, 43);
        rectMode(CORNER);
        rect(0, height*0.90, width, height*0.10);
            
        for (var i = 0; i < grassXs.length; i++) {
            imageMode(CORNER);
            image(getImage("cute/GrassBlock"), grassXs[i], height*0.85, 20, 20);
            grassXs[i] -= 1;
            if (grassXs[i] <= -20) {
                grassXs[i] = width;
            }
        }
        
        for (var i = 0; i < sticks.length; i++) {
            sticks[i].draw();
            beaver.checkForStickGrab(sticks[i]);
            sticks[i].x -= 1;
        }
        
        for (var i = 0; i < holes.length; i++) {
            holes[i].draw();
            beaver.checkForHoleAvoid(holes[i]);
            holes[i].x -= 1;
        }
        
        /*for (var i = 0; i < npcs.length; i++) {
            npcs[i].draw();
            beaver.checkForNpcAvoid(npcs[i]);
            npcs[i].x -= 1;
        }*/
        
        textSize(18);
        text("Score: " + (beaver.sticks - beaver.holes), 20, 30);
        
        textSize(15);
        fill(237, 230, 14);
        text("Score 5 to pass the level", 10, 380);
        
        if (keyIsPressed && keyCode === 0) {
            beaver.hop();
        } else {
            beaver.fall();
        }
        
        if ((beaver.sticks - beaver.holes)/sticks.length >= 0.50) {
                currentScene = 3;
            }
        for(var i = 0; i < sticks.length; i++) {
            if(sticks[i].x>0) {
                gameOver = false;
            }
        }
        
        if (gameOver === true) {
            currentScene = 8;
        }
        
        beaver.draw();
    }
        
    else if (currentScene === 3) {
        rectMode(CORNER);
        imageMode(CENTER);
        background(0, 221, 255);
        fill(0, 0, 0);
        textSize(20);
        text("Continue to level 2", 10, 30);
        image(getImage("creatures/Hopper-Cool"), 100, 100);
        fill(81, 166, 31);
        rect(200, 200, 100, 25);
        fill(255, 255, 255);
        textSize(16);
        text("Press Button", 205, 220);
        if (mouseIsPressed&& mouseX >= 200 && mouseX <=300 && mouseY >=200 && mouseY <=225){
        currentScene = 4;
        }
    }
    
    // LEVEL 2
    else if (currentScene === 4) {
        var gameOver = true;
        var stick = new Stick(0, 0, 5, 40);
        background(227, 254, 255);
        fill(130, 79, 43);
        rectMode(CORNER);
        rect(0, height*0.90, width, height*0.10);
            
        for (var i = 0; i < grassXs.length; i++) {
            imageMode(CORNER);
            image(getImage("cute/GrassBlock"), grassXs[i], height*0.85, 20, 20);
            grassXs[i] -= 1;
            if (grassXs[i] <= -20) {
                grassXs[i] = width;
            }
        }
        
        for (var i = 0; i < sticks.length; i++) {
            sticks[i].draw();
            beaver.checkForStickGrab(sticks[i]);
            sticks[i].x -= 1;
        }
        
        for (var i = 0; i < holes.length; i++) {
            holes[i].draw();
            beaver.checkForHoleAvoid(holes[i]);
            holes[i].x -= 1;
        }
        
        textSize(18);
        text("Score: " + (beaver.sticks - beaver.holes), 20, 30);
        
        textSize(15);
        fill(237, 230, 14);
        text("Score 20 to pass the level", 10, 380);
        
        if (keyIsPressed && keyCode === 0) {
            beaver.hop();
        } else {
            beaver.fall();
        }
        
        if ((beaver.sticks - beaver.holes)/sticks.length >= 0.60) {
            currentScene = 5;
        }
        
        for(var i = 0; i < sticks.length; i++) {
            if(sticks[i].x>0) {
                gameOver = false;
            }
        }
        
        if (gameOver === true) {
            currentScene = 8;
        }
        beaver.draw();
    }
    
    else if (currentScene === 5){
        rectMode(CORNER);
        imageMode(CENTER);
        background(0, 221, 255);
        fill(0, 0, 0);
        textSize(20);
        text("Continue to final level!", 10, 30);
        image(getImage("creatures/Hopper-Jumping"), 100, 100);
        fill(81, 166, 31);
        rect(200, 200, 100, 25);
        fill(255, 255, 255);
        textSize(16);
        text("Press Button", 205, 220);
        fill(0, 0, 0);
        textSize(20);
        text("Avoid creatures!!!!!", 10, 300);
        if (mouseIsPressed&& mouseX >= 200 && mouseX <=300 && mouseY >=200 && mouseY <=225){
        currentScene = 6;
        }
    }
    
    // LEVEL 3
    else if (currentScene === 6) {
        var gameOver = true;
        background(227, 254, 255);
        fill(130, 79, 43);
        rectMode(CORNER);
        rect(0, height*0.90, width, height*0.10);
            
        for (var i = 0; i < grassXs.length; i++) {
            imageMode(CORNER);
            image(getImage("cute/GrassBlock"), grassXs[i], height*0.85, 20, 20);
            grassXs[i] -= 1;
            if (grassXs[i] <= -20) {
                grassXs[i] = width;
            }
        }
        
        for (var i = 0; i < sticks.length; i++) {
            sticks[i].draw();
            beaver.checkForStickGrab(sticks[i]);
            sticks[i].x -= 1;
        }
        
        for (var i = 0; i < holes.length; i++) {
            holes[i].draw();
            beaver.checkForHoleAvoid(holes[i]);
            holes[i].x -= 1;
        }
        
        for (var i = 0; i < npcs.length; i++) {
            npcs[i].draw();
            beaver.checkForNpcAvoid(npcs[i]);
            npcs[i].x -= 1;
        }
        
        textSize(18);
        text("Score: " + (beaver.sticks + beaver.holes - beaver.npcs), 20, 30);
        
        textSize(15);
        fill(237, 230, 14);
        text("Score 30 to pass the level", 10, 380);
        
        if (keyIsPressed && keyCode === 0) {
            beaver.hop();
        } else {
            beaver.fall();
        }
        
        if ((beaver.sticks - beaver.holes - beaver.npcs)/sticks.length >= 0.70) {
            currentScene = 7;
        }
        
        for(var i = 0; i < sticks.length; i++) {
            if(sticks[i].x>0) {
                gameOver = false;
            }
        }
        
        if (gameOver === true) {
            currentScene = 8;
        }
        beaver.draw();
    }
    
    else if (currentScene === 7){
        rectMode(CORNER);
        imageMode(CENTER);
        background(0, 221, 255);
        image(getImage("creatures/Hopper-Happy"), 100, 100);
        fill(81, 166, 31);
        rect(200, 200, 100, 25);
        fill(255, 255, 255);
        textSize(16);
        text("Play again", 205, 220);
        fill(0, 0, 0);
        textSize(20);
        text("YOU WIN!!!!!", 100, 300);
        if (mouseIsPressed&& mouseX >= 200 && mouseX <=300 && mouseY >=200 && mouseY <=225){
        currentScene = 1;
        }
    }
    
    else if (currentScene === 8){
        rectMode(CORNER);
        imageMode(CENTER);
        background(0, 221, 255);
        image(getImage("creatures/OhNoes"), 100, 100);
        fill(81, 166, 31);
        rect(200, 200, 100, 25);
        fill(255, 255, 255);
        textSize(16);
        text("Play again", 205, 220);
        fill(0, 0, 0);
        textSize(20);
        text("YOU LOST!!!", 100, 300);
        if (mouseIsPressed&& mouseX >= 200 && mouseX <=300 && mouseY >=200 && mouseY <=225){
        currentScene = 1;
        }
    }
};
