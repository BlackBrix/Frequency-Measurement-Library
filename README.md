# Frequency-Measurement-Library  
  
<a href="raw/master/pics/FreqPeriodTitle.jpg"><img class="alignnone size-large wp-image-2526" title="FreqPeriodTitle" src="raw/master/pics/FreqPeriodTitle-360x259.jpg" alt="FreqPeriodTitle" width="360" height="259"></a>  
  
For frequency measurement in the audio or sub audio range the determination of the signal period length delivers the most accurate results. The architecture of the ATMEGA chip provides a special Counter and Capture unit which is designed to  do this job with a high precision. Compared to a Frequency Counter Library published here before, this method delivers a higher accuracy and speed in the range below 20 KHz.  
  
<a href="http://interface.khm.de/wp-content/uploads/2010/06/FreqPeriodSch05.jpg"><img class="alignnone size-large wp-image-2527" title="FreqPeriodSch05" src="http://interface.khm.de/wp-content/uploads/2010/06/FreqPeriodSch05-360x243.jpg" alt="FreqPeriodSch05" width="360" height="243"></a>  
  
## The Measurement

When the Input Signal crosses a threshold determined by the input network a capture event is triggered by the hardware. Counter1 TCNT1 is incremented continuously by the 16MHz clock. The capture event takes a snapshot of Counter1 TCNT1 which is written into the capture register ICR1. The Library takes the difference of ICR1 between two capture events and returns the result as the period length of the input signal. On a Counter1 overflow between two events a constant value of 65536 is added to the result so that the overall precision is enhanced to long integer format. The Analog Comparator triggers the event when the input signal is greater than 100 mV. With a bias resistor on pin 5 the trigger threshold is toggled to get better noise immunity which works similar like a Schmitt-Trigger circuit. The picture shows how a sine wave is detected by the Analog Comparator.  
  
<a href="http://interface.khm.de/wp-content/uploads/2010/06/FreqPeriodScope.jpg"><img class="alignnone size-full wp-image-2524" title="FreqPeriodScope" src="http://interface.khm.de/wp-content/uploads/2010/06/FreqPeriodScope.jpg" alt="FreqPeriodScope" width="320" height="234"></a>  
  
However the best results are obtained when the input signal is a square wave like shown in the serial monitor window. Here the output wobbles in the range of a few millihertz which shows the obtainable resolution.  
  
<a href="http://interface.khm.de/wp-content/uploads/2010/06/FreqPeriod_wind_sq.png"><img class="alignnone size-large wp-image-2525" title="FreqPeriod_wind_sq" src="http://interface.khm.de/wp-content/uploads/2010/06/FreqPeriod_wind_sq-276x270.png" alt="FreqPeriod_wind_sq" width="276" height="270"></a>    
  
The library returns the measured period length in 1/16 us steps as a long integer. To get the frequency, the clock frequency has to divided by the returned value. The clock frequency is set in this example to 16000400 to compensate inaccuracies of the Arduino board.  
```C++
/* Frequency & Period Measurement for Audio
 * connect pin 5,6,7 to input circuit
 *
 *
 *
 * KHM 2010 /  Martin Nawrath
 * Kunsthochschule fuer Medien Koeln
 * Academy of Media Arts Cologne
 */

#include "FreqPeriod.h"

double lfrq;
long int pp;

void setup() {
  Serial.begin(115200);
  FreqPeriod::begin();
  Serial.println("FreqPeriod Library Test");
}

void loop() {

  pp=FreqPeriod::getPeriod();
  if (pp ){

    Serial.print("period: ");
    Serial.print(pp);
    Serial.print(" 1/16us  /  frequency: ");

    lfrq= 16000400.0 / pp;
    printDouble(lfrq,6);
    Serial.print(" Hz");

    Serial.println("  ");
  }

}

//***************************************************************************
void printDouble( double val, byte precision){
  // prints val with number of decimal places determine by precision
  // precision is a number from 0 to 6 indicating the desired decimial places
  // example: lcdPrintDouble( 3.1415, 2); // prints 3.14 (two decimal places)

  if(val < 0.0){
    Serial.print('-');
    val = -val;
  }

  Serial.print (int(val));  //prints the int part
  if( precision > 0) {
    Serial.print("."); // print the decimal point
    unsigned long frac;
    unsigned long mult = 1;
    byte padding = precision -1;
    while(precision--)
      mult *=10;

    if(val >= 0)
      frac = (val - int(val)) * mult;
    else
      frac = (int(val)- val ) * mult;
    unsigned long frac1 = frac;
    while( frac1 /= 10 )
      padding--;
    while(  padding--)
      Serial.print("0");
    Serial.print(frac,DEC) ;
  }
}
```
  
--------
  

The Library is tested on Atmega168/328  
  
Download FreqPeriod Library  (updated 1/2012)  
  
Martin Nawrath / KHM 2010  


