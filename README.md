# Zenith-Z148-Clock
Files for the PZT148 real time clock

I failed to find the driver software for this card by Premier Technologies Ltd - so I made a work arround using GWBASIC.
At the heart of the clock circuit it uses an MSM58321RS 4BIT real time Clock Module so this approach may be valid for other RTC units.
By port sniffing with GWBASIC OUT and INP commands, trail and error and plain dumb luck,
I found the ports to communicate with the hardware.  The clock enable jumper caused differences to show when reading the port, so that helped.      
Decimal port 615 (control instructions) and port 616 (4 bit data in/out) are the ones that work with this RTC. 
for example sending OUT 615,233 sets pin 9 (ADDRESS WRITE LATCH) to High, and this showed up on my logic probe.  

CLK.BAS
CLK.BAS reads the current hardware real time clock registers, then it manipulates the data into a format that can be used to set the dos system TIME and DATE. 
The program then exits GWBASIC to the system.  Now DOS knows the date and time until the next reboot.  
CLK.BAS can be used in the autoexec.bat by adding a line: 

GWBASIC CLK.BAS  

adding this to AUTOEXE.BAT will run the CLK.BAS at boot time and sync the software clock to the hardware clock (within a 1 second accuracy).  
You may need to give a path if the GWBASIC and CLK.BAS files are not in the root. 

CLOCK.BAS 
This is a program to clear and set the hardware clock (RTC), then read the clock, and set the system clock. 
setting the clock is done register by register from shortest to longest minutes to decades.  
I skipped seconds S0 and S1, so aim for the next minute when setting.
The order is :
minutes, 1's    e.g. for 12:30 enter 0
minutes, 10's   e.g. for 12:30 enter 3
hours, 1's    e.g. for 12:30 enter 2
hours 10's   ' this is a tricky one ! add 8 to make it 24hr clock.  i.e. if 2pm, 14:00:00 you enter 1+8 =9, bit 4 is used for 12/24 hr clock.
Day of the week, 0 = sunday, 1 = monday etc.    
day of month, 1's
day of month 10's
month of year, 1's  e.g. 2 for december 
month of year, 10's e.g. 1 for october - November -  december
2 digit year, 1s    e.g. 3 for 2023
2 digit year, 10's  e.g. 2 for 2023 decade
the program will set the hardware clock based on these inputs.  
if the display is wrong, try again - Dont set an impossible date or time ! 

Have fun playing with your Clock, 

MAS 12/16/2022 
