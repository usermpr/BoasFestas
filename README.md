# BoasFestas
ArrayList<NATAL> systems;
PFont sourceLight;
float max_distance;

void setup() {
  size(1290, 720);
 //sourceLight = createFont("infini-gras.otf", 150);
  //textFont(sourceLight);
  systems = new ArrayList<NATAL>();
  max_distance = dist(0, 0, width, height);
}

void draw() {
 
  background(00);
  for (NATAL ps: systems) {
    ps.run();
    ps.addParticle();
  }
  
  
  if (systems.isEmpty()) {
    
   fill(250);
    textAlign(CENTER);
    //text("BOAS FESTAS!", 570, 400);
    //text("BOM ANO!", 800, 600);\
     sourceLight = createFont("infini-gras.otf", 60);
       textFont(sourceLight);
    text("CLICA NA DESCOBERTA!", 800, 320);
    fill(0, 0, 0);
    for(int i = 0; i <= width; i += 30) {
    for(int j = 0; j <= height; j += 20) {
      float size = dist(mouseX, mouseY, i, j);
      size = size/max_distance * 100;
   ellipse(i, j, size, size);
    fill(0, 0, 0);
 //float diam=abs(pmouseX-mouseX);
 //ellipse(mouseX,mouseY,diam,diam); 
  {
      
 }
    }
  }
    
  }
}

void mousePressed() {
  systems.add(new NATAL(1, new PVector(mouseX, mouseY)));
}





class NATAL {

  ArrayList<Particle> particles;    
  PVector origin;                   

  NATAL(int num, PVector v) {
    particles = new ArrayList<Particle>();   
    origin = v.get();                       
    for (int i = 30; i < num; i++) {
      particles.add(new Particle(origin));    
    }
  }


  void run() {
    for (int i = particles.size()-1; i >= 0; i--) {
      Particle p = particles.get(i);
      p.run();
      if (p.isDead()) {
        particles.remove(i);
      }
    }
  }

  void addParticle() {
    Particle p;
    if (int(random(0, 20)) == 0) {
      p = new Particle(origin);
    } 
    else {
      p = new CrazyParticle(origin);
    }
    particles.add(p);
  }

  void addParticle(Particle p) {
    particles.add(p);
  }

  boolean dead() {
    return particles.isEmpty();
  }
}


class CrazyParticle extends Particle {
  float theta;

  CrazyParticle(PVector l) {
    super(l);
    theta = 50.0;
  }



  void update() {
    super.update();

    float theta_vel = (velocity.x * velocity.mag()) / 15.0f;
    theta += theta_vel;
  }

  void display() {
    super.display();


  pushMatrix();
    translate(location.x,location.y);
    rotate(theta);
    stroke(205,lifespan);
    line(0,0,200,0);
    popMatrix();
  }

}


class Particle {
  PVector location;
  PVector velocity;
  PVector acceleration;
  float lifespan;

  Particle(PVector l) {
    acceleration = new PVector(0,0.05);
    velocity = new PVector(random(-1,1),random(-1,0));
    location = l.get();
    lifespan = 200.0;
     sourceLight = createFont("infini-gras.otf", 150);
       textFont(sourceLight);
      text("BOAS FESTAS!", 570, 400);
    text("BOM ANO!", 800, 600);
  }

  void run() {
    update();
    display();
  }

  void update() {
    velocity.add(acceleration);
    location.add(velocity);
    lifespan -= 2.0;
  }
  
  void display() {
    stroke(200,lifespan);
    fill(0,lifespan);
    ellipse(location.x,location.y,200,8);
  }

  boolean isDead() {
    return (lifespan < 30.0);
  }

}
