/*
 * 
Reaction Timer for auditory & visual stimuli 
by Märt Reinvee, EMÜ, Estonia.

Code loosely based on:
1) Arduino Reaction Timer using an RGB LCD 16x2 Character Display by Derek Molloy https://www.youtube.com/watch?v=o8V3zl6pmLw
2) Switch Case by Tom Igoe
 
Hardware:

1)Display: UART Serial 16*2 Characters LCD https://www.itead.cc/uart-serial-16-2-characters-lcd.html
!! Note, that the display is controlled via custom commands: http://wiki.iteadstudio.com/UART_SERIAL_16*2_CHARACTERS_LCD !!
2)Controller: ITEADUINO MINI NANO V3.0 ATMEGA328 BOARD FOR ARDUINO ID https://www.itead.cc/development-platform/arduino/iteaduino-nano.html
3)Piezobuzzer
4) 1xLED + resistor
5) 1xpushbutton + resistor
6) 1xtoggle-button + resistor

*
*/

//Hardware setup
int ledPin         =  13;    // Stop LED
int buttonPin      =  7;    // reaction timer button
int choisePin      =  11;    // LED / Buzzer selecting button
int tonePin        =  2;    // Buzzer connection
int choise;

//Phrases for uart-lcd
String greeting1 = ("Alustamiseks"); //Phrase on the first row
String greeting2 = ("vajuta nuppu"); //Phrase on the second row
String alert = ("valmis olla!");     //Seady
String listen1 = ("kuula");          //Listen
String cheat1 = ("Tegemist on");     //meassage for impossibly fast reaction time, row 1
String cheat2 = ("pettusega");       //row 2
String result1 = ("Reageerimisaeg:"); 
String result3 = (" ms");
String start1 = ("<START!>");   //GO

//In order to send a string to the display the command must start with 'ss' and end with ';'
String command1 = String("ss");
String command2 = String(";");

// States of execution
long randomDelayTime;        // holds the random time amount
boolean prepareState = true; // in introduction mode
boolean isTiming = false;    // timing the press state
long timerStartMillis;       // the time when the timer started
long timerEndMillis;         // the time when the timer finished

// Setup function - called only once
void setup() {
  pinMode(ledPin, OUTPUT);       // LED is an output
  pinMode (tonePin, OUTPUT);     //Buzzer is an output
  pinMode (choisePin, INPUT);   // button is an input
  pinMode(buttonPin, INPUT);     // button is an input
  Serial.begin(9600);  
  delay(20);            
  randomSeed(2*analogRead(0));     // use unconnected pin to seed random sequence
  
  Serial.print("sc;");   //clear display 
  delay(20);             //delay for display commands
  Serial.print("sd0,0;"); //fist row, first column
  delay(20);
  Serial.print(command1 + greeting1 + command2); //concatenate the strings in order to conform with command structure
  delay(20);
  Serial.print("sd1,2;"); //second row..
  delay(20); 
  Serial.print(command1 + greeting2 + command2);
  delay(20);
   }

 
 void loop() {

  choise = digitalRead(choisePin); //read choise button 
  switch (choise) {
  case 0:   //Reaction for sound stimulus
  
 if(prepareState){                

    if (digitalRead(buttonPin)==true)  // if the button is pressed
    {
      Serial.print("sc;");                 // clear the display
      delay(20);
      Serial.print("sd0,2;");
      delay(20);
      Serial.print(command1 + alert + command2);
      delay(20);
      randomDelayTime = random(6000);  // this is the random amount to be used 0-1,8 seconds
      while (digitalRead(buttonPin)==true) {} // wait until the button is released
      prepareState = false;       // finished prepare state - lets move on
    }
  }
  else // not in prepare state
  {
     if (!isTiming)                // the timer isn't running, so we are pausing for random amount, ! - tähsitab eitust.
     {
      Serial.print("sc;");                 // clear the display
      delay(20);
      Serial.print("sd0,5;");
      delay(20);
      Serial.print(command1 + listen1 + command2);  
      delay(randomDelayTime);      // delay for the random amount
      timerStartMillis = millis(); // get the current time 
        while (digitalRead(buttonPin)==false) 
  {
           analogWrite(tonePin,208);
           delay(1);
           digitalWrite(tonePin, LOW);
           delay(1);
  }
        isTiming = true;             // now we are ready to start timing reactions
       
     }
     else // now we are timing person's reaction
     {
        if (digitalRead(buttonPin)==true)  // when they press the button
        {
           timerEndMillis = millis(); // get the current time
           digitalWrite(ledPin, LOW); // turn off the red led
           long difference = timerEndMillis - timerStartMillis; // time taken is difference between times
           Serial.print("sc;");                    // clear the LCD
           delay(20);
           Serial.print("sd0,1;");
           delay(20);
           if (difference == 0)            // If the difference is 0 they held the button - or are supermen!
           {
             Serial.print(command1 + cheat1 + command2);
             delay(30);
             Serial.print("sd1,2;");
             delay(30);
             Serial.print(command1 + cheat2 + command2);
             delay(20);
           }
           else  // valid time
           {  
             Serial.print(command1 + result1 + command2);
             delay(30);
             Serial.print("sd1,3;");
             delay(30);
             String result2 = String(difference, DEC);
             Serial.print(command1 + result2 + result3 + command2);
             delay(20);
            }
           delay(1000);
           while (digitalRead(buttonPin)==false) {}                    // leave the message on the screen for 5 seconds
           isTiming = false;               // ready to start timing again
           prepareState = true;            // ready to start all over
         }
     }
  }
  break;
 
  case 1:   //reaction for visual stimulus
 if(prepareState){                // prepare state - give out the instruction to press button
                 
      if (digitalRead(buttonPin)==true)  // if the button is pressed
    {
      Serial.print("sc;");                 // clear the display
      delay(20);
      Serial.print("sd0,2;");
      delay(20);
      Serial.print(command1 + alert + command2);
      delay(20);
      randomDelayTime = random(6000);  // this is the random amount to be used 0-1,8 seconds
      while (digitalRead(buttonPin)==true) {} // wait until the button is released
      prepareState = false;       // finished prepare state - lets move on
    }
  }
  else // not in prepare state
  {
     if (!isTiming)                // the timer isn't running, so we are pausing for random amount, ! - tähsitab eitust.
     {
        delay(randomDelayTime);      // delay for the random amount
        digitalWrite(ledPin, HIGH);  // when finished - set red LED high
        Serial.print("sc;");
        delay(10); 
        Serial.print("sd0,4;");
        delay(20);         // press now message
        Serial.print(command1 + start1 + command2);
        delay(20);
        isTiming = true;             // now we are ready to start timing reactions
        timerStartMillis = millis(); // get the current time 
     }
     else // now we are timing person's reaction
     {
        if (digitalRead(buttonPin)==true)  // when they press the button
        {
           timerEndMillis = millis(); // get the current time
           digitalWrite(ledPin, LOW); // turn off the red led
           long difference = timerEndMillis - timerStartMillis; // time taken is difference between times
           Serial.print("sc;");                    // clear the LCD
           delay(20);
           Serial.print("sd0,1;");
           delay(20);
           if (difference == 0)            // If the difference is 0 they held the button - or are supermen!
           {
            Serial.print(command1 + cheat1 + command2);
            delay(30);
            Serial.print("sd1,2;");
            delay(30);
            Serial.print(command1 + cheat2 + command2);
            delay(20);
           }
           else  // valid time
           {  
             String result2 = String(difference, DEC);
             Serial.print("sd1,3;");
             delay(20);
             Serial.print(command1 + result2 + result3 + command2);
             delay(60);
             Serial.print("sd0,0;");
             delay(60);
             Serial.print(command1 + result1 + command2);
             delay(60);
 
           }
           delay(1000);
           while (digitalRead(buttonPin)==false) {}                    // leave the message on the screen for 5 seconds
           isTiming = false;               // ready to start timing again
           prepareState = true;            // ready to start all over
         }
     }
  }
  break;
  }
 }
