# Arduino Binary Clock
Nowdays, reading the time is boring. All one needs to do is to take a glance at their phone or their computer, or even a glance at their desk and all the information they need, will be right there for them. This clock challenges people and changes this by using binary codes to show the time.  

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Aaron Fang | Paly | Undecided | Upcoming Sophomore 

![Headstone Image](https://cdn.discordapp.com/attachments/799773888032014406/857099616511328276/IMG_0419.jpg)(second to last green is really dim)
  
# First Milestone: Learning to read time with Arduino 
My first milestone I made for myself in the process of making the clock was to learn how to track time passing using an Arduino, I had to do this because reading the time is the most important feature of a clock. In order to do this, I started off with the millis(); function to keep track of the time since the Arduino started. Then with this, I could add certain timed events to happen. In this case, I wrote "time to dance".

[![First Milestone](https://cdn.discordapp.com/attachments/799773888032014406/858075178101768232/Screen_Shot_2021-06-25_at_1.01.29_PM.png)](https://youtu.be/JirGGPNNe5g&feature=emb_logo "First Milestone"){:target="_blank" rel="noopener"}

# Second Milestone
My second Milestone was for me to have finished the physical build. I didn't want to worry about the code in that time. So I decided to get the physical build done first. This was done by first wiring up 13 LEDS to a breadboard, therefore making a small prototype for the physical build. Then I decided I wanted to solder the wires into a perf board to make the clock more permanent than if I were to use a breadboard. After soldering the wires into a breadboard, the next logical step was for me to put it into a container which would allow me to show the time how I wanted. 

[![Third Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1612574014/video_to_markdown/images/youtube--y3VAmNlER5Y-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/vvg8xu6B3hI){:target="_blank" rel="noopener"}
# Last Milestone
The last milestone I set for myself was for me to complete the build, physical and the code. Since I already had the physical build done, I needed to get the code done. Originally, I planned on using a few buttons to manually need to change the time. Sort of like a regular clock. Then I shifted my plan to use a RTC chip to be able to track the time. With the RTC in, All I needed to do was convert the RTC time into a way showable by the LED’s in the front. And once that was done. The clock was complete. You could tell what time it was just from the LEDs. 



[![Final Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1612574117/video_to_markdown/images/youtube--CaCazFBhYKs-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=CaCazFBhYKs "First Milestone"){:target="_blank" rel="noopener"}

# CODE
#include <Wire.h>

#include "RTClib.h"

RTC_DS3231 rtc;
 //arduino pins
int oneTenHours = 13; //this is the tens integer for Hours first bit
int twoTenHours = 12;
int oneOneHours = 11; //this is the ones integer for Hours first bit
int twoOneHours = 10;
int fourOneHours = 9;
int eightOneHours = 8;
int oneTenMinutes = 7; //this is the tens integer for minutes first bit
int twoTenMinutes = 6;
int fourTenMinutes = 5;
int oneOneMinutes = 4; //this is the ones integer for minutes first bit
int twoOneMinutes = 3;
int fourOneMinutes = 2;
int eightOneMinutes = A0;

void setup() {
  // put your setup code here, to run once:
  pinMode(oneTenMinutes, OUTPUT);
  pinMode(twoTenMinutes, OUTPUT);
  pinMode(fourTenMinutes, OUTPUT);
  pinMode(oneOneMinutes, OUTPUT);
  pinMode(twoOneMinutes, OUTPUT);
  pinMode(fourOneMinutes, OUTPUT);
  pinMode(eightOneMinutes, OUTPUT);
  pinMode(oneTenHours, OUTPUT);
  pinMode(twoTenHours, OUTPUT);
  pinMode(oneOneHours, OUTPUT);
  pinMode(twoOneHours, OUTPUT);
  pinMode(fourOneHours, OUTPUT);
  pinMode(eightOneHours, OUTPUT);
  
  Serial.begin(9600);
  delay(3000);

  if(! rtc.begin()) {
    Serial.println("Couldn't find RTC");
    while (1);
  }
  if (rtc.lostPower()) {
    Serial.println("RTC lost power, lets set the time!");
    rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
  }
  
}

void loop() {
  // put your main code here, to run repeatedly:
  DateTime now = rtc.now();
  //seperates into four digits
  int hr = now.hour();
  int mn = now.minute();
  Serial.print(hr);
  Serial.print("::");
  Serial.print(mn);
  Serial.println(" ");
  int oneHours = (hr%10);
  int tenHours = ((hr/10)%10);
  int oneMinutes = (mn%10);
  int tenMinutes = (int(mn/10)%10);

  setMinutesOnes(oneMinutes);
  setMinutesTens(tenMinutes);
  setHoursOnes(oneHours);
  setHoursTens(tenHours);
  Serial.print(tenHours);
  Serial.print(oneHours);
  Serial.print(":");
  Serial.print(tenMinutes);
  Serial.print(oneMinutes);
  Serial.println(" ");
  delay(1000);  
}
//LED time keeping
void setMinutesTens(int myMinutes){ //minutes is an integer between 0-5, represented by 3 LEDs (1,2,4)
  if(myMinutes == 0){
    digitalWrite(oneTenMinutes, LOW);
    digitalWrite(twoTenMinutes, LOW);
    digitalWrite(fourTenMinutes, LOW);
  }
  else if(myMinutes == 1){
    digitalWrite(oneTenMinutes, HIGH);
    digitalWrite(twoTenMinutes, LOW);
    digitalWrite(fourTenMinutes, LOW);
  }
  else if(myMinutes == 2){
    digitalWrite(oneTenMinutes, LOW);
    digitalWrite(twoTenMinutes, HIGH);
    digitalWrite(fourTenMinutes, LOW);
  }
  else if(myMinutes == 3){
    digitalWrite(oneTenMinutes, HIGH);
    digitalWrite(twoTenMinutes, HIGH);
    digitalWrite(fourTenMinutes, LOW);
  }
  else if(myMinutes == 4){
    digitalWrite(oneTenMinutes, LOW);
    digitalWrite(twoTenMinutes, LOW);
    digitalWrite(fourTenMinutes, HIGH);
  }
  else if(myMinutes == 5){
    digitalWrite(oneTenMinutes, HIGH);
    digitalWrite(twoTenMinutes, LOW);
    digitalWrite(fourTenMinutes, HIGH);
  }
}

void setMinutesOnes(int mineMinutes){ //minutes is an integer between 0-5, represented by 3 LEDs (1,2,4)
  if(mineMinutes == 0){
    digitalWrite(oneOneMinutes, LOW);
    digitalWrite(twoOneMinutes, LOW);
    digitalWrite(fourOneMinutes, LOW);
    digitalWrite(eightOneMinutes, LOW);
  }
  else if(mineMinutes == 1){
    digitalWrite(oneOneMinutes, HIGH);
    digitalWrite(twoOneMinutes, LOW);
    digitalWrite(fourOneMinutes, LOW);
    digitalWrite(eightOneMinutes, LOW);
  }
  else if(mineMinutes == 2){
    digitalWrite(oneOneMinutes, LOW);
    digitalWrite(twoOneMinutes, HIGH);
    digitalWrite(fourOneMinutes, LOW);
    digitalWrite(eightOneMinutes, LOW);
  }
  else if(mineMinutes == 3){
    digitalWrite(oneOneMinutes, HIGH);
    digitalWrite(twoOneMinutes, HIGH);
    digitalWrite(fourOneMinutes, LOW);
    digitalWrite(eightOneMinutes, LOW);
  }
  else if(mineMinutes == 4){
    digitalWrite(oneOneMinutes, LOW);
    digitalWrite(twoOneMinutes, LOW);
    digitalWrite(fourOneMinutes, HIGH);
    digitalWrite(eightOneMinutes, LOW);
  }
  else if(mineMinutes == 5){
    digitalWrite(oneOneMinutes, HIGH);
    digitalWrite(twoOneMinutes, LOW);
    digitalWrite(fourOneMinutes, HIGH);
    digitalWrite(eightOneMinutes, LOW);
  }
  else if(mineMinutes == 6){
    digitalWrite(oneOneMinutes, LOW);
    digitalWrite(twoOneMinutes, HIGH);
    digitalWrite(fourOneMinutes, HIGH);
    digitalWrite(eightOneMinutes, LOW);
  }
  else if(mineMinutes == 7){
    digitalWrite(oneOneMinutes, HIGH);
    digitalWrite(twoOneMinutes, HIGH);
    digitalWrite(fourOneMinutes, HIGH);
    digitalWrite(eightOneMinutes, LOW);
  }
  else if(mineMinutes == 8){
    digitalWrite(oneOneMinutes, LOW);
    digitalWrite(twoOneMinutes, LOW);
    digitalWrite(fourOneMinutes, LOW);
    digitalWrite(eightOneMinutes, HIGH);
  }
  else if(mineMinutes == 9){
    digitalWrite(oneOneMinutes, HIGH);
    digitalWrite(twoOneMinutes, LOW);
    digitalWrite(fourOneMinutes, LOW);
    digitalWrite(eightOneMinutes, HIGH);
  }
}

void setHoursTens(int Hours){ //Hours is an integer between 0-5, represented by 3 LEDs (1,2,4)
  if(Hours == 0){
    digitalWrite(oneTenHours, LOW);
    digitalWrite(twoTenHours, LOW);
  }
  else if(Hours == 1){
    digitalWrite(oneTenHours, HIGH);
    digitalWrite(twoTenHours, LOW);
  }
  else if(Hours == 2){
    digitalWrite(oneTenHours, LOW);
    digitalWrite(twoTenHours, HIGH);
  }
}

void setHoursOnes(int myHours){ //Hours is an integer between 0-5, represented by 3 LEDs (1,2,4)
  if(myHours == 0){
    digitalWrite(oneOneHours, LOW);
    digitalWrite(twoOneHours, LOW);
    digitalWrite(fourOneHours, LOW);
    digitalWrite(eightOneHours, LOW);
  }
  else if(myHours == 1){
    digitalWrite(oneOneHours, HIGH);
    digitalWrite(twoOneHours, LOW);
    digitalWrite(fourOneHours, LOW);
    digitalWrite(eightOneHours, LOW);
  }
  else if(myHours == 2){
    digitalWrite(oneOneHours, LOW);
    digitalWrite(twoOneHours, HIGH);
    digitalWrite(fourOneHours, LOW);
    digitalWrite(eightOneHours, LOW);
  }
  else if(myHours == 3){
    digitalWrite(oneOneHours, HIGH);
    digitalWrite(twoOneHours, HIGH);
    digitalWrite(fourOneHours, LOW);
    digitalWrite(eightOneHours, LOW);
  }
  else if(myHours == 4){
    digitalWrite(oneOneHours, LOW);
    digitalWrite(twoOneHours, LOW);
    digitalWrite(fourOneHours, HIGH);
    digitalWrite(eightOneHours, LOW);
  }
  else if(myHours == 5){
    digitalWrite(oneOneHours, HIGH);
    digitalWrite(twoOneHours, LOW);
    digitalWrite(fourOneHours, HIGH);
    digitalWrite(eightOneHours, LOW);
  }
  else if(myHours == 6){
    digitalWrite(oneOneHours, LOW);
    digitalWrite(twoOneHours, HIGH);
    digitalWrite(fourOneHours, HIGH);
    digitalWrite(eightOneHours, LOW);
  }
  else if(myHours == 7){
    digitalWrite(oneOneHours, HIGH);
    digitalWrite(twoOneHours, HIGH);
    digitalWrite(fourOneHours, HIGH);
    digitalWrite(eightOneHours, LOW);
  }
  else if(myHours == 8){
    digitalWrite(oneOneHours, LOW);
    digitalWrite(twoOneHours, LOW);
    digitalWrite(fourOneHours, LOW);
    digitalWrite(eightOneHours, HIGH);
  }
  else if(myHours == 9){
    digitalWrite(oneOneHours, HIGH);
    digitalWrite(twoOneHours, LOW);
    digitalWrite(fourOneHours, LOW);
    digitalWrite(eightOneHours, HIGH);
  }
}
