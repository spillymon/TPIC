Propeller micro to TPIC8101 Interface

/***************************************************************************************************************************/

Setup:

Use simpleide compiler.  Set compiler type to "C", set memory model to "LMM Main Ram", set optimization to 
"-O2Speed".  The last two settings significantly reduce the time needed to send and recieve data via SPI.

/***************************************************************************************************************************/

Connections: 

Must use current limiting resistors for 5v inputs.  Voltage dividers not needed due to propeller's 
internal over voltage diode.  I repeat must use current limiting resistor with 5v inputs.  4k7 ohm resistor will 
do the job.

Propeller to TPIC8101
1 to SDO (MISO)
5 to SDI (MOSI)
7 to INT/HOLD
9 to SCLK (clock)
11 to CS (slave select)

Propeller to engine sensors
16 to Cam Sensor
17 to Crank Sensor

/***************************************************************************************************************************/

Information about this project:

I am an electrical and computer engineering student (summer/third semester) that has been developing a knock sensor interface that does not need to be disabled at high RPM's.  Generally, an engine management system ignores the knock sensor after a specific rpm because, it cannot differentiate between mechanical noise and actual knock at high rpm's.  This means you have to tune conservatively, so, you are making less horsepower and torque.  I started this project do to the lack of sophisticated aftermarket knock detection systems.

The integrated circuit (IC for short) or chip that is able to do this is Texas Instrument's TPIC8101.  The primary advantage of the TPIC8101 is the ability to window your listening period (when valves are not opening or closing etc.) to limit false positives due to valve train and other engine noise.  The solutions using the TPIC8101 I have been able to find do not vary the time constant based on RPM.  This will cause sensitivity to vary with RPM, which causes false positives on one extreme (low rpm)  and not picking up actual knock on the other (high rpm).

Integration is simply determining the area under the curve.  The IC is integrating the sine wave output from the knock sensor.  The area under the curve depends on the voltage and amount of time.  The longer the listening window, the more sine waves the IC receives, which results in more area under the curve.  This is why the time constant is so important. 

/***************************************************************************************************************************/

The code:

My current code varies the time constant with rpm (check out the “changTc” function in the source code for equation) and
takes full advantage of all the aspects the TPIC8101 offers, even the digital integrator output.  This means an analog to
digital converter is not needed.  The microcontroller sends a simple pulsed output when a configurable output level is
exceeded, which indicates knock.  This output can be sent to an engine management system and/or an indicator light.

When updating the integrator time constant and using the digital integrator output, up to four SPI data transfers occur
after each crank sensor input.  Each data transfer takes approximately 10.5 micro-seconds (0.0000105 seconds).  This
leaves plenty of time between inputs or ignition events even at high rpms.

The program is able estimate engine position with an algorithm that calculates the amount of time required to reach that
position from the last crank input pulse.  This allows a greater selection of engine positions than the number of teeth on
the crank position sensor would allow.  Ex: 12 tooth crank wheel gives engine position in 30 degree increments.  Currently
the program is able to do one degree increments with about 94% accuracy.

/***************************************************************************************************************************/


Limitations:

The current program requires both a crank and cam sensor input in order to function properly.

Currently my code is configured for a Toyota 4AGE 20v with booth a 24 tooth and one tooth cam wheel.  The code can be
reconfigured for other applications by simply changing the user configurable variables in the code.  However, I have not
yet developed an algorithm to deal with different numbers of cylinders.  This means the current code only works with four
cylinders.

An issue for some is the requirement of a resonant-type knock sensor.  These have a bandpass filter built in.  The
TPIC8101 does have an adjustable bandpass filter, however, it is not sufficient for use with a non-resonant type sensor
(non-resonant means no built in bandpass filter).  My testing has indicated the TPIC8101’s bandpass filter has bandwidth
of approximately 3khz when using a center frequency of 6.9khz.  This means a non-resonant sensor will need additional
filtering.

/***************************************************************************************************************************/


Please contact me via email if you have any input/questions.

David R. Spillman
moldspectors@gmail.com
