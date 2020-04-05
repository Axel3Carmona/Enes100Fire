# Enes100Fire
#include "Enes100.h"
#include "Tank.h"
#include <math.h>

/* The code inside void setup() runs only once, before the code in void loop(). */
void setup() {
	Enes100.begin("Team Name Here", CRASH_SITE, 3, 8, 9); // Required before you can use any other Enes100 functions.
	Tank.begin(); // Required before you can use any other Tank functions.
	//setBothMotors(255); // Set both motors to full power.
	turn(M_PI/2);
	while(Enes100.location.y < 1.5){
		Enes100.updateLocation();
		max();
	}
	zero();
	turn(0);
	while (Enes100.location.x < 2.4 && Tank.readDistanceSensor(1) > .15){
	Enes100.updateLocation();
	setBothMotors(255);
	Enes100.println(Enes100.location.x);
	//Enes100.println(Tank.readDistanceSensor(1));
	}
	zero();
	turn(-M_PI/2);
	while(Enes100.location.y > .4 && Tank.readDistanceSensor(1) > .15 && Enes100.location.x < Enes100.destination.x){
		Enes100.updateLocation();
		max();
		
	}
	zero();
	turn(0);
	while(Enes100.location.x < Enes100.destination.x && Tank.readDistanceSensor(1) > .15){
	Enes100.updateLocation();
		max();
	}
	zero();
	turn(M_PI/2);
	while (Enes100.location.y < 1.5 && Tank.readDistanceSensor(1) > .15 && Tank.readDistanceSensor(11) == 1) {
	Enes100.updateLocation();
		max();
	}
	zero();
	if (Enes100.location.x < Enes100.destination.x && Tank.readDistanceSensor(11) != 1){
		setBothMotors(-255);
		delay(100);
		zero();
	}
	turn(0);
	while( Enes100.location.x < Enes100.destination.x){
	Enes100.updateLocation();
	max();
	}
	zero();
	Enes100.updateLocation();
	float currentY = Enes100.location.y;
	if (currentY < Enes100.destination.y)
	{
		turn(M_PI/2);
		while (Enes100.location.y < Enes100.destination.y){
			Enes100.updateLocation();
			setBothMotors(127);
		}
		setBothMotors(0);
			
	}
	else {
		turn(-M_PI/2);
		while (Enes100.location.y > Enes100.destination.y){
			Enes100.updateLocation();
			setBothMotors(127);
		}
		setBothMotors(0);
	}

}

/* The code in void loop() runs repeatedly forever */ 
void loop(){
}

/* This is an example function to make both motors drive
 * at the given power.
 */
 void max(){
	 setBothMotors(255);
 }
void zero(){
	setBothMotors(0);
}
void setBothMotors(int speed) {
	Tank.setLeftMotorPWM(speed);
	Tank.setRightMotorPWM(speed);
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


/* Another example function that prints pi. */



