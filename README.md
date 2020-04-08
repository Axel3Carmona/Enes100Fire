# Enes100Fire
#include "Enes100.h"
#include "Tank.h"
#include <math.h>


/* The code inside void setup() runs only once, before the code in void loop(). */
void setup() {
	Enes100.begin("JAWAJJEE", FIRE, 3, 8, 9); // Required before you can use any other Enes100 functions.
	Tank.begin();// Required before you can use any other Tank functions.
	Enes100.updateLocation();
	double a = Enes100.location.y;
	double b = Enes100.destination.y;
	double theta1 = Enes100.location.theta;
	
	if(a < b){ //start of the orientation algorithm, OSV y position is less than Destination y
		if(theta1 != M_PI/2){
			turn(M_PI/2);
			Tank.turnOffMotors();
		}
		Enes100.updateLocation();
		double c = Enes100.location.y;
		while(c < b){
			Enes100.updateLocation();
			c = Enes100.location.y;
			setBothMotors(255);
		}
		Tank.turnOffMotors();
	}else if(a > b){
		if(theta1 != -M_PI/2){
			turn(-M_PI/2);
			Tank.turnOffMotors();
		}
		Enes100.updateLocation();
		double c = Enes100.location.y;
		while(c > b){
			Enes100.updateLocation();
			c = Enes100.location.y;
			setBothMotors(255);
		}
		Tank.turnOffMotors();
	}
	turn(0);
	Enes100.updateLocation();
	double d = Enes100.location.x;
	double e = Enes100.location.y;
	
	while(d < 1.45){
		Enes100.updateLocation();
		d = Enes100.location.x;
		setBothMotors(255);
	}
	Tank.turnOffMotors();
	
	Enes100.updateLocation(); //update location
	double firstObstacle = Tank.readDistanceSensor(1); //sense for first obstacle
	double theta2 = Enes100.location.theta; //update theta
	double f = Enes100.location.x;
	Enes100.println(firstObstacle);
	Enes100.print(e);
	
	if(firstObstacle > 0.4){ //No first obstacle detected
		if((e < 4/3 && e > 2/3) || e > 4/3){
			secondObstacle(0);	//run second obstacle algorithm
		}
		if(e < 2/3){
			secondObstacle(3);
		}
	}else if((Tank.readDistanceSensor(1) < 0.4) && (e < 4/3) && (e > 2/3)){ //Middle Lane Scenario if first obstacle detected
		turn(-M_PI/2);
		Enes100.updateLocation();
		double g = Enes100.location.y;
		
		while(g > 1.5/3){ //drive down to avoid obstacle
			Enes100.updateLocation();
			g = Enes100.location.y;
			setBothMotors(255);
		}
		
		Tank.turnOffMotors();
		turn(0);
		secondObstacle(1); //run second obstacle algorithm
		
	}else if(Tank.readDistanceSensor(1) < 0.4 && e > 4/3){ //Top lane scenario if first obstacle detected
			turn(-M_PI/2);
			Enes100.updateLocation();
			double g = Enes100.location.y;
		
			while(g > 1){ //drive down to avoid obstacle
				Enes100.updateLocation();
				g = Enes100.location.y;
				setBothMotors(255);
			}
			
			Tank.turnOffMotors();
			turn(0);
			secondObstacle(1);	//run second obstacle algorithm
	}else if(Tank.readDistanceSensor(1) < 0.4 && e < 2/3){ //Bottom lane scenario if first obstacle detected
		turn(M_PI/2);
		Enes100.updateLocation();
		double g = Enes100.location.y;
		
		while (g < 1.5/3){
			Enes100.updateLocation();
			g = Enes100.location.y;
			setBothMotors(255);
		}
		Tank.turnOffMotors();
		turn(0);
		secondObstacle(2);
	}
}

/* The code in void loop() runs repeatedly forever */ 
void loop() { 
	
  	
}

/* This is an example function to make both motors drive
 * at the given power.
 */
void secondObstacle(int scenario){
	Enes100.updateLocation();
	double h = Enes100.location.x;
	while(h < 2.2){ //drive to x location of second obstacle
		Enes100.updateLocation();
		h = Enes100.location.x;
		setBothMotors(255);
	}
	Tank.turnOffMotors();
	
	Enes100.updateLocation();
	double secondObs = Tank.readDistanceSensor(1); //sense for obstacle
	
	if(secondObs < 0.4){ //Second obstacle detected, drive around obstacle
		if(scenario == 0){
			turn(-M_PI/2);
			Enes100.updateLocation();
			double l = Enes100.location.y;
			
			while(l > 1.5/3){
				Enes100.updateLocation();
				l = Enes100.location.y;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
			turn(0);
			Enes100.updateLocation();
			double m = Enes100.location.x;
			
			while(m < Enes100.destination.x){
				Enes100.updateLocation();
				m = Enes100.location.x;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
			turn(M_PI/2);
			Enes100.updateLocation();
			double n = Enes100.location.y;
			
			while(n < Enes100.destination.y){
				Enes100.updateLocation();
				n = Enes100.location.y;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
			
		}else if(scenario == 1){//Second obstacle detected scenario for top and middle lane	
			turn(M_PI/2);
			Enes100.updateLocation();
			double j = Enes100.location.y;
		
			while(j < Enes100.destination.y){
				Enes100.updateLocation();
				j = Enes100.location.y;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
			turn(0);
			Enes100.updateLocation();
			double k = Enes100.location.x;
		
			while(k < Enes100.destination.x){
				Enes100.updateLocation();
				k = Enes100.location.x;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
		}else if(scenario == 2){ //Second obstacle detected scenario for bottom lane
			turn(M_PI/2);
			Enes100.updateLocation();
			double j = Enes100.location.y;
			
			while(j < 1){
				Enes100.updateLocation();
				j = Enes100.location.y;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
			turn(0);
			Enes100.updateLocation();
			double k = Enes100.location.x;
			
			while(k < Enes100.destination.x){
				Enes100.updateLocation();
				k = Enes100.location.x;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
			
			turn(-M_PI/2);
			Enes100.updateLocation();
			double l = Enes100.location.y;
			
			while(l > Enes100.destination.y){
				Enes100.updateLocation();
				l = Enes100.location.y;
				setBothMotors(255);
			}
			
			Tank.turnOffMotors();
		}else if(scenario == 3){
			turn(M_PI/2);
			Enes100.updateLocation();
			double l = Enes100.location.y;
			
			while(l < 1){
				Enes100.updateLocation();
				l = Enes100.location.y;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
			turn(0);
			Enes100.updateLocation();
			double m = Enes100.location.x;
			
			while(m < Enes100.destination.x){
				Enes100.updateLocation();
				m = Enes100.location.x;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
			turn(-M_PI/2);
			Enes100.updateLocation();
			double n = Enes100.location.y;
			
			while(n > Enes100.destination.y){
				Enes100.updateLocation();
				n = Enes100.location.y;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
		}
		
	}else if(secondObs > 0.4){ 
		if(scenario == 0){
			Enes100.updateLocation();
			double l = Enes100.location.x;
			
			while(l < Enes100.destination.x){
				Enes100.updateLocation();
				l = Enes100.location.x;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
		}else if(scenario == 1){ //No second obstacle detected scenario for top and midle lane, drive destination x position, then to destination y 		position
			Enes100.updateLocation();
			double j = Enes100.location.x;
		
			while(j < Enes100.destination.x){
				Enes100.updateLocation();
				j = Enes100.location.x;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
			turn(M_PI/2);
		
			Enes100.updateLocation();
			double k = Enes100.location.y;
		
			while(k < Enes100.destination.y){
				Enes100.updateLocation();
				k = Enes100.location.y;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
		}else if(scenario == 2){ //No second obstacle detected for bottom lane
			Enes100.updateLocation();
			double j = Enes100.location.x;
		
			while(j < Enes100.destination.x){
				Enes100.updateLocation();
				j = Enes100.location.x;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
			turn(-M_PI/2);
			
			Enes100.updateLocation();
			double k = Enes100.location.y;
		
			while(k > Enes100.destination.y){
				Enes100.updateLocation();
				k = Enes100.location.y;
				setBothMotors(255);
			}
			Tank.turnOffMotors();
		}else if(scenario == 3){
			Enes100.updateLocation();
			double l = Enes100.location.x;
			
			while(l < Enes100.destination.x){
				Enes100.updateLocation();
				l = Enes100.location.x;
			}
		}
	}
}

void turn(float angle){
	Enes100.updateLocation();
	float currentposition = Enes100.location.theta;
	if ( currentposition < angle) {
		while (Enes100.location.theta < angle){
			Enes100.updateLocation();
			Tank.setLeftMotorPWM(-255);
			Tank.setRightMotorPWM(255);
		}
	setBothMotors(0);
	while (Enes100.location.theta >angle){
		Enes100.updateLocation();
		Tank.setLeftMotorPWM(20);
		Tank.setRightMotorPWM(-20);
	}
	setBothMotors(0);
	} else {
		while (Enes100.location.theta > angle){
			Enes100.updateLocation();
			Tank.setLeftMotorPWM(255);
			Tank.setRightMotorPWM(-255);
		}
		setBothMotors(0);
		while (Enes100.location.theta <angle ){
			Enes100.updateLocation();
			Tank.setLeftMotorPWM(-20);
			Tank.setRightMotorPWM(20);
		}
		setBothMotors(0);
	}	
}

void setBothMotors(int speed) {
	Tank.setLeftMotorPWM(speed);
	Tank.setRightMotorPWM(speed);
}




