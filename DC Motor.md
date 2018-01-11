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















this code is still not working totally and when solved will change from RPM to angular velocity. The reason being is that millis() can overflow quite quickly and if multiplied by six I could be making the issue worse before fixing other problems with the code#include <IRremote.h>

''''c<b>
#include <IRremoteInt.h>
#include <LiquidCrystal.h>

int RECV_PIN = 8; //pin assigned to ir reciever
IRrecv irrecv(RECV_PIN);
decode_results results; //store remotes value

LiquidCrystal lcd(12, 11, 5, 4, 10, 7);  //defining lcd pins

const int dataIN = 2; //IR sensor INPUT for speed dection
int gate = 9; // pin assigned to open mesfet gate
int rpm; // RPM value
int duty;  // stores a value for PWM

unsigned long prevmillis; // To store time
unsigned long duration; // To store time difference
unsigned long lcdrefresh; // To store time for lcd to refresh

boolean currentstate; // Current state of IR input scan
boolean prevstate; // State of IR sensor in previous scan

void setup()
{
  irrecv.enableIRIn(); // Start the receiver
  pinMode(gate,OUTPUT);
  pinMode(dataIN,INPUT);
  lcd.begin(16,2);     
  prevmillis = 0;
  prevstate = LOW;  
  analogWrite(gate, 255);
}

void loop()
{
  angular_velocity();
  speedcontrol();

}

int speedcontrol(){
 
  if (irrecv.decode (& results))  
  { 
    
    long IRRemote = 0;
    long speed1 = 0xff30cf;
    long speed2 = 0xff18e7;
    long speed3 = 0xff7a85;
    
    IRRemote = (results.value); 

    lcd.clear();
    lcd.setCursor(0,0);

    if(IRRemote  == speed1){
      duty = duty - 10; 
      analogWrite(gate, duty);      
    }
          
    if(IRRemote == speed2){
      duty = duty + 10;
      analogWrite(gate, duty);
    }

    lcd.print("PWM = ");
    lcd.print(duty);  
  }

irrecv.resume(); // Receive the next value
analogWrite(gate, duty);
return duty;
}
 
void angular_velocity(){
 // degrees per second Measurement
 currentstate = digitalRead(dataIN); // Read IR sensor state
  
 if( prevstate != currentstate) // If there is change in input
   {
     if( currentstate == HIGH ) // If input only changes from LOW to HIGH
       {
         duration = ( micros() - prevmillis ); // Time difference between revolution in microsecond
         rpm = (11250000/duration); // 11250000 = (1/ time millis)*(1000*1000*60*6)/32 = degrees per second
         prevmillis = micros(); // store time for next revolution calculation
       }
   }
  prevstate = currentstate; // store this scan (prev scan) data for next scan
  
  // LCD Display
  if( ( millis()-lcdrefresh ) >= 1000 )
    {
      lcd.clear();
      lcd.setCursor(0,1);
      lcd.print("RPM = ");
      lcd.print(rpm);         
      lcdrefresh = millis();   
    }

}
.
</b>'''
<b>'''c

duty control by infrared remote to choose 3 speed setting on pulse width.

#include <IRremote.h>
#include <IRremoteInt.h>
#include <LiquidCrystal.h>

int RECV_PIN = 8; //pin assigned to ir reciever
IRrecv irrecv(RECV_PIN);
decode_results results; //store remotes value

LiquidCrystal lcd(12, 11, 5, 4, 10, 7);  //defining lcd pins

const int dataIN = 2; //IR sensor INPUT for speed dection
int gate = 9; // pin assigned to open mesfet gate
int rpm; // RPM value
int duty;  // stores a value for PWM

unsigned long prevmillis; // To store time
unsigned long duration; // To store time difference
unsigned long lcdrefresh; // To store time for lcd to refresh

boolean currentstate; // Current state of IR input scan
boolean prevstate; // State of IR sensor in previous scan

void setup()
{
  irrecv.enableIRIn(); // Start the receiver
  pinMode(gate,OUTPUT);
  pinMode(dataIN,INPUT);
  lcd.begin(16,2);     
  prevmillis = 0;
  prevstate = LOW;  
  analogWrite(gate, 255);
}

void loop()
{
  angular_velocity();
  speedcontrol();

}

int speedcontrol(){
 
  if (irrecv.decode (& results))  
  { 
    
    long IRRemote = 0;
    long speed1 = 0xff30cf;
    long speed2 = 0xff18e7;
    long speed3 = 0xff7a85;
    
    IRRemote = (results.value); 

    lcd.clear();
    lcd.setCursor(0,0);

    if(IRRemote  == speed1){
      duty = duty - 10; 
      analogWrite(gate, duty);      
    }
          
    if(IRRemote == speed2){
      duty = duty + 10;
      analogWrite(gate, duty);
    }

    lcd.print("PWM = ");
    lcd.print(duty);  
  }

irrecv.resume(); // Receive the next value
analogWrite(gate, duty);
return duty;
}
 
void angular_velocity(){
 // degrees per second Measurement
 currentstate = digitalRead(dataIN); // Read IR sensor state
  
 if( prevstate != currentstate) // If there is change in input
   {
     if( currentstate == HIGH ) // If input only changes from LOW to HIGH
       {
         duration = ( micros() - prevmillis ); // Time difference between revolution in microsecond
         rpm = (11250000/duration); // 11250000 = (1/ time millis)*(1000*1000*60*6)/32 = degrees per second
         prevmillis = micros(); // store time for next revolution calculation
       }
   }
  prevstate = currentstate; // store this scan (prev scan) data for next scan
  
  // LCD Display
  if( ( millis()-lcdrefresh ) >= 1000 )
    {
      lcd.clear();
      lcd.setCursor(0,1);
      lcd.print("RPM = ");
      lcd.print(rpm);         
      lcdrefresh = millis();   
    }

}
</b>'''

https://github.com/jgell/lab-journal.md/blob/master/DC%20Motor.md
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26169818_504132126668416_6095534043517008697_n.jpg?oh=da56e278f5e0ec2f801b169fe6895ff9&oe=5AB36DB8
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26239167_504132043335091_2983679147367181504_n.jpg?oh=80fafe177a528fa3101745e42fab3b2d&oe=5AF440A6
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26219440_504131846668444_1126382931795985420_n.jpg?oh=08cf9a800cefc53bb739b4038943feb7&oe=5AB5D005
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26196174_504132170001745_1955822201881655201_n.jpg?oh=d77febb0ec0c474ed2e2eb8aa260c232&oe=5AB2B53A
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26230341_504131873335108_1908618700162135486_n.jpg?oh=6c1f11b60b364f268aa3f01a9542f3d9&oe=5AB46A36
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26231776_504131876668441_3068762678981181975_n.jpg?oh=c71048e97a19e39edf64e6ce504cfeda&oe=5AF8A7AA
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26239548_504132110001751_2957293107887016430_n.jpg?oh=e62e9e2608bde21810715d5d198a626a&oe=5AE6C68C
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26734448_504132183335077_8172108596097874500_n.jpg?oh=d5c8d66d4daa8b244e4652c06d22048f&oe=5AB4AE4B
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26219691_504131743335121_2425890310900180844_n.jpg?oh=c2ecd1fa74d780c7238d39ac051d0ddd&oe=5AB3F512
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26733472_504131793335116_7556836880360623652_n.jpg?oh=75069a6f3c4c2159609f792115a42fd2&oe=5AEC6103
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26196279_504132056668423_431839627080528724_n.jpg?oh=4f99238f161fabbb0ed6fe8c5cbd2c35&oe=5AFECFA6
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26219405_504132130001749_3762926161465669312_n.jpg?oh=35aa9f45d516740e12169abb137971c4&oe=5AB125CE
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26229914_504131993335096_8915955637555267803_n.jpg?oh=bec6df8b7f3b2eaabc04acfac285c17d&oe=5AB36795
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26219738_504155803332715_5368656751235109704_n.jpg?oh=917e7bc19ef0ea9151dc0a993bab4e60&oe=5AFE63D7
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26229824_504155829999379_7257578027988944398_n.jpg?oh=18f27c22c67b8094c7a632c3b741b1b8&oe=5AE7CCDB
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26231241_504155859999376_432024468047462088_n.jpg?oh=b10a381d19c2d3dc2a43ecc6b06f8adc&oe=5AF34D3F
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26219693_504155879999374_5042234702698361400_n.jpg?oh=dba28640511199d479f6be4ee7622a7d&oe=5AFA96D1
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26196026_504155899999372_104087161731913060_n.jpg?oh=0b094862106f7f1caf95bbe167148268&oe=5AB6D2FC
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26230158_504155929999369_1337004874302325606_n.jpg?oh=2997252555c7d913731be1f0f91a01a5&oe=5AFB56EA
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26219796_504155969999365_9053811824678892838_n.jpg?oh=65981a90f5717fdf88401c2ffe1a8813&oe=5AF9F79C
https://www.facebook.com/jack.gell.39/videos/504156353332660/
https://www.facebook.com/jack.gell.39/videos/504156233332672/
https://www.facebook.com/jack.gell.39/videos/504156506665978/
https://www.facebook.com/jack.gell.39/videos/504156556665973/
https://www.facebook.com/jack.gell.39/videos/504156656665963/
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26219809_504156023332693_7520718065614911702_n.jpg?oh=3573c0f3c1da3b93ab0b33c3253b7d7e&oe=5AB419C4
https://www.facebook.com/jack.gell.39/videos/504156733332622/
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26230045_504156083332687_1686835564241953209_n.jpg?oh=c41618bb1f2c59aa6b53221a38ca9487&oe=5AEC55E6
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26231671_504156123332683_4081584507467518834_n.jpg?oh=8f7c86bc0387e3e3337a65eb78c7fe93&oe=5AB675E3
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26230536_504156149999347_1372605246873979524_n.jpg?oh=c23b7ebe4d55f0efb3c751d040732153&oe=5AB9790F
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26219931_504161243332171_4867539354303015142_n.jpg?oh=beea1e92fadf2beffddbc90049371946&oe=5AFBB466
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26239463_504161379998824_4041412746738721406_n.jpg?oh=39aa2f0b80215ce9b9db3ee6d32212c1&oe=5AE4DB12
