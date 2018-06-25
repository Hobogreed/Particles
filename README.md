# Particles

Particles[] p=new Particles[0x0000ffff];
float cX,cY;

void setup(){
  size(500,500);
  frameRate(4500);
  noSmooth();
  background(#000000);
  for(int i=0;i<p.length;i++){
   p[i]=new Particles(i);
  }
}

void draw(){
  //cX=mouseX;cY=mouseY;

  borders();
  run();
  //noLoop();
}

void run(){
   for(int i=0;i<p.length;i++){
    p[i].check();
   }
}

void borders(){
  //  vertical
  color n=0xfeffffff;
  for(int y=0;y<499;y++){
    set(0,y,n);
    set(499,y,n);
  }
  // horizontal
  color m=0xfdffffff;
  for(int x=0;x<499;x++){
    set(x,0,m);
    set(x,499,m);
  }
  
}

void mousePressed(){
  loop();
}

class Particles{
  float X,Y,vX,vY;
  float dX,dY;
  float XG=.0,YG=.0,E=-.89;
  int cID;
  
  Particles(int i){  
    cID=0xffffffff-i;
    //X=250;Y=250;
    X=random(2,498);Y=random(2,498);
    cX=10;cY=10;
    //vX=random(-1,1);vY=random(-1,1);
    set(int(X),int(Y),cID);
  }
  
  void check(){
    //println("check");
    //  get direction
    getDir();
    //cX=mouseX;cY=mouseY;
    //vX+=dX;vY+=dY;
    
    //  get destination color
    float destX=X+dX,destY=Y+dY;
    
    //println(X,Y,destX,destY);
    //if(destX>1){destX=1;}if(destY>1){destY=1;}
    //if(destX<-1){destX=-1;}if(destY<-1){destY=-1;}
    //dX=X+dX;dY=Y+dY;
    int dC=get(int(destX),int(destY));
    //println(X,Y,destX,destY,hex(dC));
    //  itself
    if (dC==cID){X+=dX;Y+=dY;return;}
    //  empty space
    if (dC==#000000){
             //println("empty space");
             set(int(X),int(Y),#000000);
             X=destX;Y=destY;
             set(int(X),int(Y),cID);
             //println(X,Y);
             return;}
    //  border         
    if (dC==0xfeffffff){vX*=E;return;}
    if (dC==0xfdffffff){vY*=E;return;}
    //  another particle
    int n=abs(0xffffffff-dC);
    float vXT=p[n].vX,vYT=p[n].vY;
    p[n].vX=vX;p[n].vY=vY;
    vX=vXT;vY=vYT;
  }
  
  void getDir(){
    //println("getDir");
    cX=mouseX;cY=mouseY;
    //  println(cX,cY);
    dX=cX-X;dY=cY-Y;
    
    float m=sqrt(sq(dX)+sq(dY));
    dX/=m;dY/=m;
    dX/=4;dY/=4;
    //println(dX,dY);
  }
  
  void update(){
    vX+=XG;vY+=YG;
    if(vY>1){vY=1;}
    if(vX>1){vX=1;}
    if(vY<-1){vY=-1;}
    if(vX<-1){vX=-1;}
    X+=vX;Y+=vY;
  }
  
}//end of class  
