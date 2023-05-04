Download Link: https://assignmentchef.com/product/solved-csc-230-lab-4-introduction-to-mega-2560-board-and-i-o-instructions
<br>
In the AVR MEGA 2560 microcontroller, each Input/Output (I/O) port has three associated special purpose registers, the data direction register (DDRx), output register (PORTx), and an input register (PINx). These registers correspond to addresses in the data space accessible by the processor (the first 0x200 bytes in SRAM).

Open Atmel Studio 7 and create a new project named lab3, then build the program. From the menu, click on “Debug” -&gt; “Start Debugging and Break”. On the right-hand side, observe the I/O View. To see the memory address of PORTB and PORTL, click on PORTB or PORTL:

The memory address of PORTL:

Some ports are mapped to an address value that is smaller than 0x40. For example, PORTB is mapped to 0x25. For these ports, you may use IN/OUT instructions to transfer data between the general purpose registers (r0 through r16) and data memory (SRAM). However, some ports, such as PORTL, have a mapping to an address value larger than 0x3F and they cannot be accessed using the IN/OUT instructions. What can we do in this case? We can use LDS and STS instructions instead.

In AVR, ports A to G use Port Mapped I/O (separate addresses from memory) and Ports H to K use Memory Mapped I/O (usage is similar to any memory location). Port Mapped addresses use IN/OUT instructions while Memory Mapped ones use LDS/STS.

<ol>

 <li><strong> Pins and Ports </strong></li>

</ol>

The six LEDs are associated with six pins and the pins are mapped to some bits of two ports, PORTB and PORTL:

In the project named lab4 which you created above, write a small program to turn on a specific LED (the code is provided below):

The memory addresses of DDRL, DDRB, PORTL, and PORTB are defined in a file named

“m2560def.inc”. To see the contents of this file, first build your solution and then go to the “Solution Explorer” and click on “Dependencies” -&gt; “m2560def.inc” (see screenshot below).

Note that the I/O registers – 0x10B and 0x10A – are specified in hexadecimal numbers. The two I/O registers are given names, PORTL and DDRL, by using the .equ directives to represent their memory addresses in data memory, which allows us to refer to the I/O registers by names instead of numbers when we program.

<strong>III. Build and upload the .hex file to the board:</strong>

Build and run (debug) the program above in Atmel Studio. Observe the changes of the data memory and the I/O registers.

Now that you know your program works, you can upload it to the AVR board, but first you need to know which port number Windows assigned to the board. <strong>This port number could be different from time to time and your upload could fail.</strong> To verify which port number is assigned to your AVR board follow the directions below.

Verify the COM port number:

<ol>

 <li>Launch Arduino IDE by clicking the Start button, then Arduino.</li>

 <li>In the Arduino IDE program, from the menu at the top select “Tools”, then “Port”, then see which COM port lists “Arduino/Genuino Mega or Mega 2560” next to it. In the screenshot below, COM4 is the one.</li>

</ol>

Upload the lab4.hex file to the board by typing the following command (as one long line) at the command window:

“C:Program Files (x86)Arduinohardwaretoolsavrbinavrdude.exe” -C “C:Program Files

(x86)Arduinohardwaretoolsavretcavrdude.conf” -p atmega2560 -c wiring -P COM4 -b 115200 -D -F -U flash:w:lab3.hex

Instead of typing such a long command, you can use upload.bat file by double-clicking it. This file contains the abovementioned command. Note that you may need to modify the contents of upload.bat first. The relevant components are highlighted above in red. Additionally, upload.bat file must be located in the same directory (folder) as the .hex file that you wish to upload to the AVR board. To find the location of lab4.hex file, refer to the following screenshot:

<ol>

 <li><strong> Exercises:</strong></li>

 <li>You can use the LED lights to display a binary number. If you tilt your LED lights (or your head) by 90 degrees, each can represent a bit of that binary number (see the picture below). Let pin 52 represent the most significant bit and pin 42 represent the least significant bit.</li>

</ol>

Change the values sent to PORTB and PORTL via r16 to reflect the picture above. Rebuild the program and upload it to the AVR board to verify that the correct LEDs are on. Now display -22 using two’s complement representation.

<ol start="2">

 <li>Modify your code to make one LED light blink by turning it on for a while and then off for a while.</li>

</ol>

Hint: use a nested loop with NOP (no operation) to use up <strong>eight million</strong> cycles, then turn the lights on or off (XOR gate?).

Remember, you can count the exact number of cycles your program requires, refer to the AVR Instruction Set to determine each operation’s cycle count. For instance, DEC and BRNE require 2n+3, where n is the starting number. Adding a NOP will make it 3n+3. Since the ATMEGA2560 processor runs at 16MHz clock speed, if your nested loop uses 8M cycles, then the lights would turn on for half a second, then turn off for half a second. Build and upload your code and verify that it runs correctly.