# ZX81 Hardware

## Basic Motherboards

Issue #1. Made with curved traces and handmade. The board has the “solder mask” on the solder side in green. Most plates are green on the component side as well, although some plates can be found to be the color of the raw material on the plate.

![Issue 1](_zx81/_images/ZX81_issue1.jpg?raw=true "Issue 1")

![Issue 1_yellow](_zx81/_images/ZX81_issue1_amarillo.jpg?raw=true "Issue 1 Yellow")

![Issue 1_green](_zx81/_images/ZX81_issue1_verde.jpg?raw=true "Issue 1 Green")



Issue #2. It is a fairly difficult prototype to find. It is a unit that was made as a test for issue 3.


Issue #3. Made with CAD design and straight lines. The board has the solder mask on the solder side in red.

![Issue 3](_zx81/_images/ZX81_issue3%20copia.jpg?raw=true "Issue 3")

![Issue 3_red](_zx81/_images/ZX81_issue3_rojo.jpg?raw=true "Issue 3 Red")


## CPUs

The CPU models that may appear in the ZX81 are:

* ZILOG Z80A
* NEC (uPD780C-1)
* MOSTEK (MK3880)

The pin layout or “pinout” is shown in the following figure:

![cpu_pinout](_zx81/_images/Z80_CPU_pin-out.png?raw=true "CPU Pinout")


## ROM

The ROM models that can be found in a ZX81 are:

* 2364 (8kB x 8 bits) 24-pin, on 28-pin socket
* Compatible Motorola ZCM38818C or 68764
* Compatible Mostek MK36809. This model can be found welded directly to the plate, without a base

The ROM firmware versions are:

* The “Standard” ROM. It is the original and the first buggy version as programmed by Sinclair.
* The “Improved” ROM. It is the second version programmed by Sinclair, and refined by Dr. I. Logan and Dr. Frank O'Hara. Some boards mounted a small circuit on top of the CPU with two SN74LS27N chips (“piggyback fix”).
* The “Shoulders of Giants” ROM. It is a debugged version programmed by Geoff Wearmouth.

The "piggyback fix" added to the "standard ROM" fixes the arithmetic errors but does not fix another bug involving the PAUSE function.

The error due to PAUSE –always with old ROMs-, causes the computer to crash or delete the program it has in memory by interrupting it with BREAK (“white-out effect”), as long as it is working in FAST mode. To avoid this, put POKE 16437,255; before the PAUSE statement.

The definitive test is to type PRINT PEEK 54, if the value received is 136, the ROM is correct.

To calculate the ROM checksum, enter the following code:

```
10 FAST
20 LET A=0
30 FOR B=0 TO 8191
40 LET A=A+PEEK B
50 NEXT B
60 PRINT A
```

|              |  Standard ROM  |  Standard ROM + Piggyback Fix  |  Improved ROM  |
|--------------|----------------|--------------------------------|----------------|
|  Checksum:  |  854885  |  854885  |  855106  |
|  PRINT PEEK 3823  |  33  |  33  |  205  |
|  PRINT SQR 0.25  |  1.3591409  |  0.5  |  0.5  |
|  PRINT 0.25**2  |  3.142384  |  0.0625  |  0.0625  |
|  PRINT 4-0.0000000001  |  12  |  4  |  4  |
|  PRINT SQR 0.0625  |  1.847264  |  0.25  |  0.25  |

The ROM pinout is shown in the following figure:

![rom_pinout](_zx81/_images/ROM.jpg?raw=true "ROM Pinout")


## RAM

The boards can have two different models of RAM mounted:

* Two 18-pin ICs of type 2114 (1kB x 4 bits).
* A 24-pin IC of type 4118 (1kB x 8 bits) or 4816 (2kB x 8 bits). Type 4801 (1kB x 8 bits) is compatible with 4118.

|         |  PRINT PEEK 16388 + 256*PEEK 16389  |  PRINT PEEK 16389  |
|---------|-------------------------------------|--------------------|
|  RAM 1kB:  |  17408  |  68  |
|  RAM 16kB:  |  32768  |  128  |

The pinouts of the different options are shown in the following figures:

![Ram_2114](_zx81/_images/RAM%202114.jpg?raw=true "RAM 2114")

![Ram_4118_4801](_zx81/_images/RAM%204118_4801.jpg?raw=true "RAM 4118/4801")

![Ram_4816](_zx81/_images/RAM%204816.jpg?raw=true "RAM 4816")


## ULA

There are three ULA models, both based on the Ferranti 2C000 prototype.

* 1st version: 2C158E. The reference to the version is placed on the rear face of the chip.
Only the year and week of production is written on the face when mounted on
the plinth This version does not generate the “back porch” of the image. The first versions
of this ULA, which were mounted on the issue #1 plate, are soldered directly
to plate without plinth.

* 2nd version: 2C184E. The video signal does not produce the "back porch" so on modern TVs you can see no picture or a very dark picture.

* 3rd version: 2C210E. It is the most modern version. Fix generation problem
from the back porch.

The pinout is common to all versions of ULA and can be seen in the figure below.

![ULA_Pinout](_zx81/_images/ULA.jpg?raw=true "ULA Pinout")


## Transistors

The ZX81 has only two transistors on the board: TR1 and TR2.

The transistor TR1 is part of the circuit that participates in the generation of the display of
so particular screen of the SLOW mode, interconnecting the signals /NMI, /HALT, /WAIT
and /INT, between the ULA and the CPU.

Transistor TR2 amplifies the clock signal at pin 14 of the ULA. The X1 oscillator is
6.5 MHz, which the ULA divides to 3.25 MHz, which is the CLK signal needed by the CPU in its
pin 6


## Electrolytic Capacitors

The ZX81 board only has two electrolytic capacitors: C3 and C5.

Capacitor C3 (22 uF – 16V) is part of the 7805 regulator circuit and serves as
input signal filtering and avoid unregulated power supply ripple.

Capacitor C5 is connected to pin 26 of the CPU which corresponds to the signal
/RESET. The ZX81 does not have a reset button but it has the circuit to be able to
install it. A simple push button between the pins of capacitor C5 enables the reset on the ZX81.


## Screen resolution

The way the ZX81 encodes video memory is slightly different if we have more than 3.25kB of RAM or less. In the case of having more than 3.25kB, the screen lines are encoded in the so-called expanded mode and always have the same size: 24 x 32 bytes (768 bytes + 25 bytes in HALT instructions = 793 bytes). The first byte in video memory is a HALT opcode, and each line is terminated with a HALT (end of line).

![dfile](_zx81/_images/pantalla%201.jpg?raw=true "Display File")


In the case of having less than 3.25kB, the minimum size is 25 bytes, which corresponds to the first byte (HALT), plus 1 byte (HALT) for each of the 24 rows. That is, an empty screen occupies 25 bytes.

The following figure shows an example of the space occupied by the rendering after running the program in Basic on a computer with less than 3.25 kB of RAM. 

![dfile2](_zx81/_images/pantalla%202.jpg?raw=true "Display File 2")
