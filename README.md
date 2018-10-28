# Frequency-Measurement-Library  
  
picture  
  
For frequency measurement in the audio or sub audio range the determination of the signal period length delivers the most accurate results. The architecture of the ATMEGA chip provides a special Counter and Capture unit which is designed to  do this job with a high precision. Compared to a Frequency Counter Library published here before, this method delivers a higher accuracy and speed in the range below 20 KHz.  
  
picture  
  
The Measurement

When the Input Signal crosses a threshold determined by the input network a capture event is triggered by the hardware. Counter1 TCNT1 is incremented continuously by the 16MHz clock. The capture event takes a snapshot of Counter1 TCNT1 which is written into the capture register ICR1. The Library takes the difference of ICR1 between two capture events and returns the result as the period length of the input signal. On a Counter1 overflow between two events a constant value of 65536 is added to the result so that the overall precision is enhanced to long integer format. The Analog Comparator triggers the event when the input signal is greater than 100 mV. With a bias resistor on pin 5 the trigger threshold is toggled to get better noise immunity which works similar like a Schmitt-Trigger circuit. The picture shows how a sine wave is detected by the Analog Comparator.  
  
picture  
  
However the best results are obtained when the input signal is a square wave like shown in the serial monitor window. Here the output wobbles in the range of a few millihertz which shows the obtainable resolution.  
  
picture  
  
The library returns the measured period length in 1/16 us steps as a long integer. To get the frequency, the clock frequency has to divided by the returned value. The clock frequency is set in this example to 16000400 to compensate inaccuracies of the Arduino board.  
  
picture  
  
The Library is tested on Atmega168/328  
Download FreqPeriod Library  (updated 1/2012)  
Martin Nawrath / KHM 2010  

