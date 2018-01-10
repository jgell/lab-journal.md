# Building a DC motor

DC MOTOR
Table of contents

Describe the basic design of each component
The overall system and how it performs
Arduino
Code
Math

Describe the basic design of each component



For this motor we both decided to opt away from 3d printing and to work with what ever we could find.
	
	- Armature
The armature was made from a balloon pump which has 8 coils (45 loops 0.5mm diameter enamelled copper wire), which are wound on screws and stretch around the armature 180 degrees. The ballon pump has a small piece of tuppleware on the end so that the armature can be opened up so that you can access the inside of the balloon pump. On each side there frictionless bearings to help with rotation.










	

-Positive  commutator
This motor has two commutators a negative and a positive. The postive commutator copper take place on top of the coils with NO splits. The brush on this commutator never loses connection as there is no need. Their is concern that the high ampage could melt the enamel below and essentially cut loops out of coil by shorting itself out. To help avoid this we are using electrical tape and making sure the copper tape stay in good condition. If the copper tape goes bad then reapply new copper tape over the old rather than removing for an extra layer of protection.

	

	-Negative commutator
The negative commutator was also made with copper tape but with the splits between the sections it wore out within a couple of minutes due to a high amount of amps. Therefore we a had to switch over to copper plates on both the brush an the commutator. The circumference of the armature was smaller than we like so we used a piece of a Nutri bullet blender with a much larger circumference. This is held in place with cigarette filters soaked in supper glue which creates a very strong support. 























	- Brushes
For the brushes we used zip ties as they a have a constant springyness so that it will return back to the commutator when bounced off. With a wire running down to deliver power from the supply, the point to point contact for the brush to the commutator is copper tape for the positive commutator and a copper plate for the negative.


	-Point of rotation
We had some frictionless bearings taken from a project last year. To ensure the bearing holes were the correct size in the balloon pump and the Tupperware we stripped a bearing down so that only the outer ring was remaining. Then with a  pair of Leatherman pliers held over the stove until extremely hot and then pushed into the plastic to make perfect bearing size holes. When the bearings where placed into their holes we then secured it with super glue to ensure stability.













	-Coils
Their are 8 coils each with 45 loops this was the most amount possible using the screws that we had to wind around them. The enamelled copper was 0.5mm in dimension (a lot thicker than the copper we were supplied with hence less windings for each coil).

	-Housing
The housing was laser cut on the ground floor of Smeaton building. Initially chose acrylic so that what was going on could be visible from more angles, to help with working in awkward places (such as below the armature). Also visibility would be mater the motor more presentable, although I  decided I did not like the look as it seemed tacky. So I decided to wrap in electrical tape. The box has two drill holes in either side for the shaft to go through and be held secure.





	-Shaft
The motor spins on the shaft but the shaft itself does not spin. It is a made of two long bolts held together with tape and two 15cm copper plates acting as a split to hold it straight. Along the bolts nuts which are spaced out evenly to create a flat surface for the “splint” and the magnets to rest on a flat surface.

	











	-32 bit rotary encoder
this was 3d printed and glued onto the end of the armature I chose 32 bits over 1 bit as it will give a more accurate reading of rpm. Once rpm is calculated you can then multiply by 6 and then you have degrees per second (angular velocity). The binary encoder is placed in the middle of an infra red circuit. 






































	-Infra red circuit
the infra red part of the circuit is very basic consisting of 2 resistors (10k, 1k) in parallel, these resistor run in series with a photo transistor and a infra red LED respectively  before heading to ground. In between the photo transistor and the resistor is a point to sense voltage. The sensing of voltage at this point is important in fact that when the LED opens the base of the transistor current is allowed to flow. When current flows this will feedback information to the Arduino saying that it has completed 1/32 of a revolution for every time that the led switches. The transistor placed in a hole within the housing of the motor which was made by pushing a soldering iron through the acrylic. The LED goes over the top of the encoder and then down to the same level as the transistor. The LED is attached to an allenkey which is cello taped to a cable clip which is cello taped to the housing of the motor.










	















	-Magnets
None of the magnets are visible as they are all held within the armature itself, attached to the shaft. Their are 6 neodymium magnets each with 12kg of pull. Note this is the reason the shaft does not spin, as the magnets are not allowed to spin.



The overall system and how It performs

The motor runs well the fact that there is a constant contact to positive means that there is no issues with how the positive and negative connections are aligned. A big issue is that the amount of current and that needs to run through it, generally the motor can keep under 2 amps but I have notice spikes reaching to 8 amps (although rare). The reason this is a big issue is that the Arduino can not ground anywhere near this amount and is running far to much for a Arduino motor shield. Therefore we will need to create a new circuit. Using a mosfet we can still control the duty from a Arduino PWM pin. The mosfet has a built in diode but I would like to put a fly back diode closer to the motor itself (in parallel). I have chosen to be what I a believe to be a capable mosfet being.



This Mosfet was chosen for reasons being that it had a low internal resistance (as power = I^2 * R the smaller the multiplier of are the less power dissipation meaning less heat).





 Circuit diagram for the motor portion of the circuit.






















	Demonstrating the large amount of current drawn from the power supply
























Arduino

I took the example code from the dle and although I understood it I would always rather create code from scratch so that I feel more comfortable when it come to debugging. I also decided that since I also own a LCD screen I thought to add extra functionality to the project with little cost.


Arduino pin setup for IR circuit


















Pin setup for the LCD screen although I had to change pin 2 over to pin seven as it was already in use for the IR circuit. With this set up the LCD screen is working a off a nibble so that there are plenty of pins left on the arduino for other things I may want to use. 

Asides from these 2 images the only other thing coming from the Arduino is the PWM leading the the mosfet to control the duty to the motor.

CODE















this code is still not working totally and when solved will change from RPM to angular velocity. The reason being is that millis() can overflow quite quickly and if multiplied by six I could be making the issue worse before fixing other problems with the code.

Duty control is still to be finished.
