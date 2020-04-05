# Enes100Fire
#include "Enes100.h"
#include "Tank.h"
#include <math.h>


/* The code inside void setup() runs only once, before the code in void loop(). */
void setup() {
	Enes100.begin("JAWAJJEE", FIRE, 3, 8, 9); // Required before you can use any other Enes100 functions.
	Tank.begin(); // Required before you can use any other Tank functions.
	double a = Enes100.location.y;
	double b = Enes100.destination.y;
	
	if(a < b){ //start of the orientation algorithm, OSV y position is less than Destination y
		setBothMotors(255);
		delay(250);
		Tank.turnOffMotors();
		Enes100.updateLocation();
		double c = Enes100.updateLocation();
		
		if(c < a){ //OSV traveled down
			Tank.setRightMotorPWM(255);
			Tank.setLeftMotorPWM(-255);
			delay(500);
			Tank.turnOffMotors();
			Enes100.updateLocation();
			double d = Enes100.location.y;
			
			while(d < b){ //while loop to make OSV travel to destinaion y positioon
			Enes100.updateLocation();
			d = Enes100.location.y;
			setBothMotors(255);
			}
			
			Tank.turnOffMotors();
		} else if(c > a){ //OSV traveled up
			
			Enes100.updateLocation();
			double d = Enes100.location.y;
			
			while(d < b){ //while loop to make OSV travel to destinaion y positioon
			Enes100.updateLocation();
			d = Enes100.location.y;
			setBothMotors(255);
			}
			
			Tank.turnOffMotors();
		}
	}
	
}

/* The code in void loop() runs repeatedly forever */ 
void loop() { 
	
  	
}

/* This is an example function to make both motors drive
 * at the given power.
 */
void setBothMotors(int speed) {
	Tank.setLeftMotorPWM(speed);
	Tank.setRightMotorPWM(speed);
}



