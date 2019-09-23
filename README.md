# processing-snowmen-code
int numSnowmens = 40;  //amount of snowmen
float spring = 0.40;  //2-4 controls movement of snowmen
float gravity = .0;
float friction = -0.9;
float banana;  //tells you that banana is a code word
Ball[] balls = new Ball[numSnowmens];

void setup() {
  size(1000, 800); //size of the background
  for (int i = 0; i < numSnowmens; i++) {
    balls[i] = new Ball(random(width), random(height), random(30, 70), i, balls);
  }
  noStroke();
  fill(255, 204);
}

void draw() {
  background(0);  //background color
  for (Ball ball : balls) {
    ball.collide();
    ball.move();
    ball.display();  
  }
}

class Ball {
  
  float x, y;
  float size;
  float vx = 0;
  float vy = 0;
  int id;
  Ball[] others;
 
  Ball(float xin, float yin, float din, int idin, Ball[] oin) {
    x = xin;
    y = yin;
    size = din;
    id = idin;
    others = oin;
  } 
  
  void collide() {
    for (int i = id + 1; i < numSnowmens; i++) {
      float dx = others[i].x - x;
      float dy = others[i].y - y;
      float distance = sqrt(dx*dx + dy*dy);
      float minDist = others[i].size/2 + size/2;
      if (distance < minDist) { 
        float angle = atan2(dy, dx);
        float targetX = x + cos(angle) * minDist;
        float targetY = y + sin(angle) * minDist;
        float ax = (targetX - others[i].x) * spring;
        float ay = (targetY - others[i].y) * spring;
        vx -= ax;
        vy -= ay;
        others[i].vx += ax;
        others[i].vy += ay;
      }
    }   
  }
  
  void move() {
    vy += gravity;
    x += vx;
    y += vy;
    if (x + size/2 > width) {
      x = width - size/2;
      vx *= friction; 
    }
    else if (x - size/2 < 0) {
      x = size/2;
      vx *= friction;
    }
    if (y + size/2 > height) {
      y = height - size/2;
      vy *= friction; 
    } 
    else if (y - size/2 < 0) {
      y = size/2;
      vy *= friction;
    }
  }
  
  void display() {
     float banana=dist(mouseX,mouseY,x,y);
    if (mousePressed) {
     
      if (banana<30) { //what lets you click the snowmen
        x=mouseX;y=mouseY;
          if (mousePressed)  {
            fill(211,46,41);  //changes color when clicked
  }
      }
    }
ellipse(x, y, size, size);  //96-101 make up the snowmen
ellipse(x, y-size/2-size/2*.8, size*.8, size*.8);
ellipse(x, y-size/2-size*.8-size/2*.5, size*.5, size*.5);
fill(234,232,223);  //color of snowmen
ellipse(x, y-size/2-size*.4-size/2*.5, size*.1, size*.1);
ellipse(x, y-size/2-size*.1-size/2*.5, size*.1, size*.1);
  }
}
