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
	
	while(d < 1.4){
		Enes100.updateLocation();
		d = Enes100.location.x;
		setBothMotors(255);
	}
	Tank.turnOffMotors();
	
	Enes100.updateLocation(); //update location
	double firstObstacle = Tank.readDistanceSensor(1); //sense for obstacle
	double theta2 = Enes100.location.theta; //update theta
	double f = Enes100.location.x;
	Enes100.println(firstObstacle);
	Enes100.print(e);
	
	if(firstObstacle == 1){ //No obstacle detected
		
		while(d < Enes100.destination.x){
			Enes100.updateLocation();
			d = Enes100.location.x;
			setBothMotors(255);
		}
		Tank.turnOffMotors();
		
	}
	
	if((Tank.readDistanceSensor(1) < 0.4) && (e < 4/3) && (e > 2/3)){ //Middle Lane, 1st obstacle
		turn(-M_PI/2);
		Enes100.updateLocation();
		double g = Enes100.location.y;
		
		while(g > 1.5/3){
			Enes100.updateLocation();
			g = Enes100.location.y;
			setBothMotors(255);
		}
		
		Tank.turnOffMotors();
		turn(0);
		
	}
}

/* The code in void loop() runs repeatedly forever */ 
void loop() { 
	
  	
}

/* This is an example function to make both motors drive
 * at the given power.
 */
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



