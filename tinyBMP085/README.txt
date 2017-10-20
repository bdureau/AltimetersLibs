This is a library for the Adafruit BMP085 Barometric Pressure + Temp sensor

Designed specifically to work with the Adafruit BMP085 Breakout 
  ----> https://www.adafruit.com/products/391

These displays use I2C to communicate, 2 pins are required to interface
Adafruit invests time and resources providing this open source code, 
please support Adafruit and open-source hardware by purchasing 
products from Adafruit!

Check out the links above for our tutorials and wiring diagrams 

Adafruit invests time and resources providing this open source code, 
please support Adafruit and open-source hardware by purchasing 
products from Adafruit!

Written by Limor Fried/Ladyada for Adafruit Industries.  
BSD license, all text above must be included in any redistribution

To download. click the DOWNLOADS button in the top right corner, rename the uncompressed folder Adafruit_BMP085. Check that the Adafruit_BMP085 folder contains Adafruit_BMP085.cpp and Adafruit_BMP085.h

-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-
Modified Feb 2012 by John De Cristofaro / johngineer to
work with ATTiny micros which use a USI port for TWI/i2c comm.

This version uses BroHogan's TinyWireM library, which is a
wrapper for Don Blake's usiTWI_Master library.

Modifications emphasize functionality and a smaller RAM and program
memory footprint. Changes have been annotated to provide a clearer
understanding of their purpose. However, some description of the 
underlying hardware is necessary.

The USI port provides a very simple mechanism for implementing TWI/i2c
on some ATTiny microcontrollers. Unlike a 'full' TWI port, USI is very
limited. The only explicit TWI functionality it provides is a simple 
'state machine' for detecting START and STOP conditions on the line. It
also provides an 8-bit data register for sending data in and out. The
DR can signal an interrupt when it is full, thus allowing for 'normal' 
operation while waiting for bits to be clocked in.

This single register allows for half-duplex communication. That is, you
can only send data in one direction at a time. If you write data to the
DR while you are waiting to receive other data, you will set off the
interrupt, and end up reading back the data you just wrote.

Because of the way the TinyWireM library, and the underlying usiTWI_Master
library, is written, the "endTransmission" command writes a byte to this
register as soon as it is called. Meanwhile, the "receive" function merely
sets up the register to read incoming data, and enables the DR interrupt.
It then waits for the DR to signal it is full, at which point the rx'd data
is copied to a software buffer.

If the "endTransmission" function occurs in the code immediately after a
call to "request",  it will write data to the register and set off the
interrupt, thus providing the user with 'bad' data. This can be remedied
by simply commenting out "endTransmission" calls when they occur immediately
after "receive" calls. In the case of the BMP085 library, this is at the end
of the "read8" and "read16" functions.

Other changes include replacing some 'float' vars with 'int32' vars, and the
suggestion to remove (or comment out) the altitude function when not needed,
because it requires the use of the math.h library (by way of the 'pow'
function), which is of considerable program size.




