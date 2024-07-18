#include "LedControl.h" //  need the library
LedControl lc=LedControl(0,1,2,1); // LedControl(DIN,CLK,CS,n);
const int trigPin = 3;
const int echoPin = 4;
int NumberDots = 0;
// Arduino pin 12 is connected to DIN (MAX7219 pin 1)
// Arduino pin 11 is connected to CLK (MAX7219 pin 13)
// Arduino pin 10 is connected to CS  (MAX7219 pin 12)
// 1 as we are only using 1 MAX7219

void setup()
{
  // the zero refers to the MAX7219 number, it is zero for 1 chip
  lc.shutdown(0,false);       // turn off power saving, enables display
  lc.setIntensity(0,3);       // sets brightness (0~15 possible values)
  lc.clearDisplay(0);         // clear screen

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  digitalWrite(trigPin, LOW);
}

void loop()
{
  long duration, inches, cm;
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);  // Lenght in microseconds
  cm = microsecondsToCentimeters(duration);

  if (cm < 11)
  {
    if (cm > 1)
    {
      NumberDots = map(cm, 1, 10, 0, 8);
      for (int i=8; i>=NumberDots; i--)
      {
        int col = i;
        int row = 0;
        lc.setLed(0,col,row,true);    // addr,row,col,state (false=off)
        for (int i=0; i<=8; i++)
        {
          int row = i;
          lc.setLed(0,col,row,true);  // turns off LED at col, row        
        }
      }
    }

    NumberDots = map(cm, 1, 10, 0, 8);
    for (int i=0; i<NumberDots; i++)
    {
      int col = i;
      int row = 0;
      lc.setLed(0,col,row,false);    // addr,row,col,state (true=on)
      for (int i=0; i<8; i++)
      {
        int row = i;
        lc.setLed(0,col,row,false);  // turns on LED at col, row        
      }
    }
  }

  if (cm >= 11)
  {
    lc.clearDisplay(0);
  }
  delay(50);   // Wait 50ms until next measure (minimun time recommended!)
}

// Calcula la distancia en cm
long microsecondsToCentimeters(long microseconds)
{
  return microseconds /58;
}
