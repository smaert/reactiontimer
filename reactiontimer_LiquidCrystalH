/*
 * 
 * !NOTICE This code does not represent the figure in the github repositry readme!
 * 
 * 
 *  Reaction Timer for auditory & visual stimuli 
 *  by Märt Reinvee, EMÜ, Estonia.
 *
 *  Code loosely based on:
 *    1) Arduino Reaction Timer using an RGB LCD 16x2 Character Display by Derek Molloy: https://www.youtube.com/watch?v=o8V3zl6pmLw
 *    2) Switch Case by Tom Igoe
 *
 *  Hardware:

 *    1)A LCD Display that is compatible with the Hitachi HD44780 driver (LiquidCrystal.h library)
 *    2)Piezobuzzer
 *    3) 1xLED + resistor
 *    4) 1xpushbutton + resistor
 *    5) 1xtoggle-button + resistor
 *
 *
 */


#include <LiquidCrystal.h>  // include LCD library

// Set up the display with 4 bits - R/W tied to GND
LiquidCrystal lcd(9, 8, 7, 6, 5, 4); // RS, E, D4-D7
int ledPin         =  11;    // red stop LED
int buttonPin      =  3;    // reaction timer button
int choisePin      =  13;    // LED / Buzzer selecting button
int tonePin        =  2;    // Buzzer connection
int choise;

// States of execution
long randomDelayTime;        // holds the random time amount
boolean prepareState = true; // in introduction mode
boolean isTiming = false;    // timing the press state
long timerStartMillis;       // the time when the timer started
long timerEndMillis;         // the time when the timer finished

// Setup function - called only once
void setup() {
  pinMode(ledPin, OUTPUT);       // LED is an output
  pinMode (tonePin, OUTPUT);
  pinMode (choisePin, INPUT);
  pinMode(buttonPin, INPUT);     // button is an input
  lcd.begin(16, 2);              // 16 Columns by 2 Rows
  randomSeed(2*analogRead(0));     // use unconnected pin to seed random sequence
}

void loop() {
  choise = digitalRead(choisePin);
  switch (choise) {
  case 0:
 if(prepareState){                // prepare state - give out the instruction to press button
    lcd.setCursor(0,0);            // set cursor to 0,0 (top left)  
    lcd.print("  Alustamiseks"); // string on the top row
    lcd.setCursor(0,1);            // next row
    lcd.print("  vajuta nuppu"); // string on the next row
    if (digitalRead(buttonPin)==true)  // if the button is pressed
    {
      lcd.clear();                 // clear the display
      lcd.setCursor(0,0);
      lcd.print("  valmis olla!");
      randomDelayTime = random(6000);  // this is the random amount to be used 0-1,8 seconds
      while (digitalRead(buttonPin)==true) {} // wait until the button is released
      prepareState = false;       // finished prepare state - lets move on
    }
  }
  else // not in prepare state
  {
     if (!isTiming)                // the timer isn't running, so we are pausing for random amount, ! - tähsitab eitust.
     {
        lcd.clear();
        lcd.setCursor(5,0);          
        lcd.print("Kuula");       
        delay(randomDelayTime);      // delay for the random amount
        timerStartMillis = millis(); // get the current time 
        while (digitalRead(buttonPin)==false) 
  {
           analogWrite(2,208);
           delay(1);
           digitalWrite(2, LOW);
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
           lcd.clear();                    // clear the LCD
           lcd.setCursor(0,0);
           if (difference == 0)            // If the difference is 0 they held the button - or are supermen!
           {
             lcd.print(" Tegemist on");
             lcd.setCursor(0,1);
             lcd.print("  pettusega!");
           }
           else  // valid time
           {  
             lcd.print("Reageerimisaeg:");
             lcd.setCursor(3,1);
             lcd.print(difference);
             lcd.print(" ms");              // milliseconds
           }
           delay(1000);
           while (digitalRead(buttonPin)==false) {}                    // leave the message on the screen
           isTiming = false;               // ready to start timing again
           prepareState = true;            // ready to start all over
         }
     }
  }
    break;
 
    case 1:
  if(prepareState){                // prepare state - give out the instruction to press button
    lcd.setCursor(0,0);            // set cursor to 0,0 (top left)  
    lcd.print("  Alustamiseks"); // string on the top row
    lcd.setCursor(0,1);            // next row
    lcd.print("  vajuta nuppu"); // string on the next row
    if (digitalRead(buttonPin)==true)  // if the button is pressed
    {
      lcd.clear();                 // clear the display
      lcd.setCursor(0,0);
      lcd.print("  valmis olla!");
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
        lcd.setCursor(0,0);          // press now message
        lcd.print("    <START!>   "); 
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
           lcd.clear();                    // clear the LCD
           lcd.setCursor(0,0);
           if (difference == 0)            // If the difference is 0 they held the button - or are supermen!
           {
             lcd.print(" Tegemist on");
             lcd.setCursor(0,1);
             lcd.print("  pettusega!");
           }
           else  // valid time
           {  
             lcd.print("Reageerimisaeg:");
             lcd.setCursor(3,1);
             lcd.print(difference);
             lcd.print(" ms");              // milliseconds
           }
           delay(1000);
           while (digitalRead(buttonPin)==false) {}                    // leave the message on the screen
           isTiming = false;               // ready to start timing again
           prepareState = true;            // ready to start all over
         }
     }
  }
     break;
 }

}   
