# 4-
粒子涌现
Mover[] movers = new Mover[1000];
 
void setup() {
  size(1000,1000);
  background(0);
  for (int i = 0; i < movers.length; i++) {
    movers[i] = new Mover();
  }
}
 
 void draw() {
  background(0);
  for (int i = 0; i < movers.length; i++) {
    movers[i].update();
    movers[i].checkEdges();
    movers[i].display();
  }
}
 
class Mover{

  PVector location;
  PVector velocity;
  PVector acceleration;
  PVector followTarget;
  float speedLimit = 10.0f;
  float followStrength = 0.0005f;
  float size = 10.0f;
  boolean followMouse = true;
  
  Mover(){
    acceleration = new PVector(-0.001,0);
    location = new PVector(random(width),random(height));
    velocity = new PVector(random(-2,2)*2,random(-2,2)*2); 
    followTarget = new PVector(width/2f, height/2f);
  }  
  
  void update(){
    acceleration = PVector.random2D();
    acceleration.mult(random(0.01f));
    if(followMouse){
      followTarget = new PVector(mouseX, mouseY);
    }
    PVector dir = PVector.sub(followTarget, location);
    dir.mult(followStrength);
    acceleration = dir;
    velocity.add(acceleration);
    location.add(velocity);
    velocity.limit(speedLimit);
  }
  
  void Follow(PVector target){
    this.followTarget = target;
  }
  
  void UpdateSize(float size){
    this.size = size;
  }
  
  void display(){
  stroke(0,155);
    fill(random(255),random(255),random(255),random(0,255));
    ellipse(location.x,location.y,size,size);
  }
  
  void checkEdges(){
    if(location.x > width)location.x = 0;
    else if (location.x < 0) location.x = width;
    
    if(location.y > height) location.y = 0;
    else if(location.y < 0) location.y = height;
  }
  
  void Apply(){
    this.update();
    //this.checkEdges();
    this.display();
  }
}
