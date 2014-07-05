Propeller micro to TPIC8101 Interface

/***************************************************************************************************************************/

Setup:

Use simpleide compiler.  Set compiler type to "C", set memory model to "LMM Main Ram", set optimization to 
"-O2Speed".  The last two settings significantly reduce the time needed to send and recieve data via SPI.

/***************************************************************************************************************************/
Connections: Must use current limiting resistors for 5v inputs.  Voltage dividers not needed due to propeller's 
internal over voltage diode.  I repeat must use current limiting resistor with 5v inputs.  4k7 ohm resistor will 
do the job.

Propeller             TPIC8101
1                     SDO (MISO)
5                     SDI (MOSI)
7                     INT/HOLD
9                     SCLK (clock)
11                    CS (slave select)

Propeller             Others
16                    Cam Sensor
17                    Crank Sensor

/***************************************************************************************************************************/

Information about this project:

I am an electrical and computer engineering student that has been developing a knock sensor interface that does 
not need to be disabled at high RPM's.  Generally, an engine management system ignores the knock sensor after a 
specific rpm because, it cannot differentiate between mechanical noise and actual knock at high rpm's.  This 
means you have to tune conservatively, so, you are making less horsepower and torque.

The integrated circuit (IC for short) or chip that is able to do this is Texas Instrument's TPIC8101.  The primary 
advantage of the TPIC8101 is the ability to window your listening period (when valves are not opening or closing etc.) 
to limit false positives due to valve train and other engine noise.  The solutions using the TPIC8101 I have been able 
to find do not vary the time constant based on RPM.  Without getting into the math.  This will cause sensitivity to 
vary with RPM, which causes false positives on one extreme and not picking up actual knock on the other.

My current code varies the time constant with rpm and takes full advantage of all the aspects the TPIC8101 offers, 
even the digital integrator output.  This means an analog to digital converter is not needed.  

Currently my code is configured for a Toyota 4AGE 20v with 24 tooth and one tooth cam wheel.  The code can be 
reconfigured for other applications by simply changing the user configurable variables in the code.  However, I 
have not yet developed an algorithm to deal with different numbers of cylinders.  This means the current code only 
works with four cylinders.

One potentially large hurdle is the requirement of a resonant-type knock sensor.  These have a bandpass filter built 
in.  The TPIC8101 does have an adjustable bandpass filter, however, it is not sufficient.  My testing has indicated a 
non-resonant sensor will need an external (external to the TPIC8101) bandpass filter.

If any of you have programming experience I am certainly open to input/improvements.  My programming skills were 
non-existent before starting this project over seven months ago.  So, please excuse any crudeness.

Please contact me via email if you have any questions.

David R. Spillman
moldspectors@gmail.com
