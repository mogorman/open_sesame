*z wave module is zm3102n (cant be because pin 11 is ground)
* pin 1,6,12,16,17 are ground this matches the module that is on the daughter card
* pin 10 and 15 are rx and tx for serial
* pin txd (15) connects to pin 8 on daughter board
* pin rxd (10) connnects to pin ping 6 on daughter board.
* pin 3,5,7,9 on daughter board are gnd
* 9600 baud bus

DAUGHTER BOARD
pin 01 NC GND
pin 02 to pin 5 u1 pin 7 u2
pin 03 GND
pin 04 pin6 on u1 pin 6 u2
pin 05 GND
Pin 06 rx
pin 07 GND
pin 08 tx
pin 09 GND
pin 10 vcc 3.3v
pin 11 NC 3.3v
pin 12 NC GND?
pin 13 cap to ground  Raw battery voltage 
pin 14 cap to ground  Raw battery voltage

main BOARD
MCU IS 40 pin

pin 01 NC NOWHERE
pin 02 mcu pin 10
pin 03 GND
pin 04 mcu pin 11
pin 05 GND
Pin 06 mcu pin 23
pin 07 GND
pin 08 mcu pin 24
pin 09 GND
pin 10 vcc 3.3v
pin 11 NC 3.3v
pin 12 ???
pin 13 Raw battery voltage 
pin 14 Raw battery voltage

traffic
0xbd 0x0f 0x03 0xff 0x0f 0x03 0xcf
0xbd 0x0f 0x03 0xc0
0xbd 0x0f 0x03 0xc0
0xbd 0x0f 0x00 0xc0
0xff 0x0f 0x03 0xcf
0xff 0x0f 0x03 0xcf
0xff 0x0f 0x0c 0xcf
0xff 0x0f 0x00 0xc0
0xff 0x0f 0x0c 0xc0
0xff 0x0f 0x0f 0xcf
0xff 0x0f 0x0c 0xcf 0xff
0xff 0x0f 0x00 0xc0
0xff 0x0f 0x0c 0xc0
0xff 0x0f 0x0f 0xcf

pin 8 traffic
0x00
0xbd 0x04 0x01
0xbd 0x04 0x02 0xe7
0xbd 0x04 0x01 0xe7
0xbd 0x04 0x02 0xe7
0xbd 0x04 0x04 0xe7
0xbd 0x04 0x01 0xe7
0xbd 0x04 0x03 0x0e
0xbf 0x0f 0x03 0x3f


it looks like first byte might be a sequence
UART>0xbd 0x04 0x03 0x0e
UART>rrrr
0xFF 0x0F 0x03 0x3F 

putting in a bad code seems to lock the lock up

READ: FAILED, NO DATA0x00 
UART>0xbd 0x04 0x03 0x0e
UART>rrrr
0xFF 0x0F 0x03 0x3F 

UART>0x00 0xbd 0x04 0x01
UART>rrrr
0xFF 0x0F 0x03 0x3F 


start in locked position
UART>0x00 0xbd 0x04 0x01
UART>rrrr
0xBF 0x0F 0x03 0x7F 


UART>0x00 0xbd 0x04 0x01
UART>rrrr
0xBF 0x0F 0x03 0x7F 


READ: FAILED, NO DATA0x00 
UART>0x00 0xbd 0x04 0x01
UART>rrrr
0xBF 0x0F 0x03 0x7F 


unlocked 
UART>0x00 0xbd 0x04 0x01
UART>rrrr
0xBF 0x0F 0x03 0x7F 


recoverd with this one multiple times 
UART>0x00 0xbd 0x04 0x01
UART>rrrr
0xBF 0x0F 0x03 0x7F 


UART>0xbd 0x04 0x03 0x0e
UART>rrrr
0xBF 0x0F 0x03 0x7F 


UART>0x00 0xbd 0x04 0x01
UART>rrrr
0xBD 0x0F 0x03 0x3F 


receive from door after connect to zwave pin 8

0x00
0xbd 0x04 0x01
0xbd 0x04 0x02 0xe7
0xbd 0x04 0x03 0xe7 0x05 0x1a
0xbd 0x04 0x04 0xe7 0x07 0x1f
0xbd 0x04 0x05 0xe7 0x07 0x1e
0xbd 0x04 0x06 0xe7 0x03 0x19


Unlock web interface

0xbd 0x04 0x07 0xe7 0x07 0x1c
0xbd 0x04 0x08 0xe7 0x07 0x13
0xbd 0x04 0x09 0xe7 0x05 0x10
0xbd 0x04 0x0a 0xe7 0x07 0x11
0xbd 0x04 0x0b 0xe7 0x07 0x10

Lock web interface

0xbd 0x04 0x0c 0xe7 0x03 0x13
0xbd 0x04 0x0d 0xe7 0x07 0x16
0xbd 0x04 0x0e 0xe7 0x07 0x15

mycode 1234 web interface

0xbd 0x0a 0x0f 0xe7 0x0b 0x00 0x01 0x12 0x12 0x34 0x34 0x17
0xbd 0x06 0x10 0xe7 0x0c 0x00 0x01 0x03

remove mycode 1234 web interface

0xbd 0x06 0x11 0xe7 0x0d 0x00 0x01 0x03
0xbd 0x06 0x12 0xe7 0x0c 0x00 0x01 0x01
0xbd 0x06 0x13 0xe7 0x0c 0x00 0x01 0x00

does not seem to remove it though

0xbd 0x06 0x14 0xe7 0x0d 0x00 0x01 0x06
0xbd 0x06 0x15 0xe7 0x0c 0x00 0x01 0x06
0xbd 0x06 0x16 0xe7 0x0c 0x00 0x01 0x05

mycode2  pin 2244

0xbd 0x0a 0x17 0xe7 0x0b 0x00 0x02 0x12 0x12 0x34 0x34 0x0c
0xbd 0x06 0x18 0xe7 0x0c 0x00 0x02 0x08

enter 1234 on pin pad
NO DATA
press lock button on pin pad
no data

Refresh web page
no data

delete mycode2

0xbd 0x06 0x1c 0xe7 0x0d 0x00 0x02 0x0d
0xbd 0x06 0x1d 0xe7 0x0c 0x00 0x02 0x0d
0xbd 0x06 0x1e 0xe7 0x0c 0x00 0x02 0x0e

cant seem to delete

poll now 

0xbd 0x04 0x1f 0xe7 0x07 0x04
0xbd 0x04 0x20 0xe7 0x07 0x3b

mycode3 9077

0xbd 0x0a 0x21 0xe7 0x0b 0x00 0x03 0x90 0x90 0x78 0x78 0x3b
0xbd 0x06 0x22 0xe7 0x0c 0x00 0x03 0x33

remove mycode3

0xbd 0x06 0x26 0xe7 0x0d 0x00 0x03 0x36
0xbd 0x06 0x27 0xe7 0x0c 0x00 0x03 0x36
0xbd 0x06 0x28 0xe7 0x0c 0x00 0x03 0x39

bad entries not sure what this is 

0xbd 0x04 0x29 0xe7 0x03 0x36
0xbd 0x04 0x2a 0xe7 0x07 0x31
0xbd 0x04 0x2b 0xe7 0x07 0x30

i take it back its deleting it on the door but not on the web interface

mycode 1357

0xbd 0x04 0x2c 0xe7 0x03 0x33
0xbd 0x0a 0x2d 0xe7 0x0b 0x00 0x04 0x12 0x34 0x56 0x78 0x38
0xbd 0x06 0x2e 0xe7 0x0c 0x00 0x04 0x38
0xbd 0x04 0x2f 0xe7 0x07 0x34

transmit from door after connect to zwave pin 6

0xbd 0x0f 0x00 0xc0 0x30 0x03 0x03
0xbf 0x0f 0x03 0xcf 0x03 0x30 0x0f
0xbf 0x0f 0x00 0xcf 0x4f 0x30 0x00 0x03 0x43

Unlock over web interface

0xbf 0x0f 0x03 0xcf 0x4f 0x00 0x00 0x80 0xf3
0xbf 0x0f 0x0c 0xc0 0x0f 0x00 0x33
0xff 0x0f 0x0f 0xc0 0x0f 0x00 0x30

Lock over web interface

0xbf 0x0f 0x0c 0xcf 0x4f 0x00 0x00 0x03 0x7f
0xbf 0x0f 0x0f 0xcf 0x4f 0x00 0x00 0x83 0xfc
0xbf 0x0f 0x0f 0xc0 0x0f 0x03 0x33
0xbf 0x0f 0x00 0xc0 0x0f 0x03 0x3c

mycode 1234
0xbf 0x00 0x0c 0xcf 0x43 0x00 0x03 0x03 0x00 0x7f
0xbf 0x00 0x33 0xc0 0x0c 0x00 0x03 0x30 0x30 0x7c 0x7c 0x03
0xbf 0x00 0x3c 0xc0 0x0c 0x00 0x03 0x30 0x7c 0x7c 0x0c

remove mycode 1234
GARBAGE
0xff 0x0f 0x40 0x3f 
0xff 0x00 0xfc 0xc0
GARBAGE
0xff 0x0f 0x03 0xcf
0xff 0x0c 0x03 0xc0
GARBAGE


so i dont know why but this seems to work reliably 
UNLOCK door 0xbd 0x04 0x03 0xe7 0x05 0x1a 
            0xbd 0x04 0x09 0xe7 0x05 0x10

LOCK door 0xbd 0x04 0x0c 0xe7 0x03 0x13

25:D:   BD:04:0C:AA:03:5E:

ADD CODE 1234 7th byte is number
0xbd 0x0a 0x0f 0xe7 0x0b 0x00 0x01 0x12 0x12 0x34 0x34 0x17
DELETE CODE 1234 second last byte is number 
0xbd 0x06 0x11 0xe7 0x0d 0x00 0x01 0x03



dont forget to apply voltage


2mm or 0.08" header spacing


decoding packet 
0xbd 0x04 0x03 0xe7 0x05 0x1a
0xbd = mcu address / my return address
0x04 = byte length
0x03 = sequence number 
0xe7 = my return address / mcu address
0x05 = payload
0x1a = checksum

checksum is an lrc
        uint8_t checksum = 0xFF;
        int i;
        for (i = 1; i < message_length; i++) {
                printf("Byte = %02X Checksum = %02X\n", message[i], checksum);
                checksum ^= message[i];
        }


urn:schemas-micasaverde-com:device:DoorLock:1
D_DoorLock1.xml
82,220,0,4,64,3,R,B,RS,W1,|32S,76S,78S:2,98S,99S,112S,113S:1,114S,117S,128S,133S,134S,139S,152S,

http://wiki.micasaverde.com/index.php/Luup_UPnP_Variables_and_Actions#DoorLock1

0x03 lock
0x05 unlock
0x07 status

 it looks like it echos back  your command with success
3:4:    BD:04:02:AA:03:50:

4:5:    BD:04:03:AA:05:57:
5:6:    BD:04:04:AA:03:56:

6:7:    BD:04:05:AA:05:51:
7:8:    BD:04:06:AA:03:54:

wrong address error?
BD:05:14:55:03:02:BA:

status response request
BD:05:C9:AA:07:01:9F:
01  is locked 
00 is in the middle aka didnt turn completely
02 is open



167:0:  BD:05:01:E7:09:00:15:
???
232:0:  BD:07:02:E7:27:00:00:02:38:
???

37:0:   BD:04:01:E7:18:05:
87:0:   BD:04:02:E7:18:06:
137:0:  BD:

395:0:  BD:05:02:AA:18:01:4B:  // every boot first message
517:0:  BD:05:01:E7:09:46:53: // second boot message
580:0:  BD:07:02:E7:27:46:00:02:7E: //third boot message

6338:0: BD:07:03:E7:27:00:00:41:7A: ???
6510:0: BD:07:04:E7:27:00:00:02:3E: ???
6582:0: BD:07:05:E7:27:00:00:01:3C: // closed lock event
6646:0: BD:07:06:E7:27:00:00:02:3C: // open lock event

16400:0:        BD:04:03:AA:0E:5C: // remove

polling maybe?
20135:0:        BD:05:06:AA:07:01:50:
20192:0:        BD:05:07:AA:07:01:51:
20250:0:        BD:05:08:AA:09:46:17:


0xBD 0x07 0x03 0xE7 0x27 0x00 0x00 0x02 0x39
0xBD 0x07 0x04 0xE7 0x27 0x00 0x00 0x01 0x3D
0xBD 0x04 0x09 0xAA 0x0F 0x57
0xBD 0x0B 0x0A 0xAA 0x10 0x14 0x06 0x21 0x02 0x09 0x38 0x06 0x42
0xBD 0x04 0x0B 0xAA 0x0F 0x55



lock

21163:0:        BD:05:0D:AA:07:01:5B:
21221:0:        BD:05:0E:AA:07:01:58:

Door was locked externally
24170:0:        BD:07:08:E7:27:00:00:41:71:

door unlock keypad
25679:0:        BD:07:0F:E7:27:00:01:42:74:

door unlocked externally
24359:0:        BD:07:09:E7:27:00:00:82:B3:

24479:0:        BD:05:1C:AA:07:02:49:
24538:0:        BD:05:1D:AA:07:02:48:
24779:0:        BD:07:0A:E7:27:00:00:01:33:
24805:0:        BD:07:0B:E7:27:00:00:02:31:
25069:0:        BD:08:0C:E7:29:00:01:01:00:35:
25147:0:        BD:0A:1F:AA:0C:00:01:12:34:56:78:45:
25186:0:        BD:0A:20:AA:0C:00:01:12:34:56:78:7A:

door unlock
25679:0:        BD:07:0F:E7:27:00:01:42:74:




READ: 0xBD 
READ: 0x04 
READ: 0x02 
READ: 0xE7 

READ: 0xBD 
READ: 0x04 
READ: 0x06 
READ: 0xE7 


3:0:    0xBD 0x04 0x03 0xE7 0x18 0x07 
8:0:    0xBD 0x04 0x04 0xE7 0x18 0x00 
13:0:   0xBD 0x04 0x05 0xE7 0x18 0x01 
18:0:   0xBD 0x04 0x06 0xE7 0x18 0x02 


pin 8 uninit

78:0:   0xBD 0x04 0x01 0xE7 0x18 0x05 
83:0:   0xBD 0x04 0x02 0xE7 0x18 0x06 

minus button pressed
108:0:  0xBD 0x04 0x03 0xE7 0x0E 0x11 
114:0:  0xBD 0x04 0x04 0xE7 0x0E 0x16 
193:0:  0xBD 0x04 0x05 0xE7 0x0E 0x17 

plus button pressed
418:0:  0xBD 0x04 0x06 0xE7 0x07 0x1D 
424:0:  0xBD 0x04 0x07 0xE7 0x07 0x1C 
430:0:  0xBD 0x04 0x08 0xE7 0x09 0x1D 

479:0:  0xBD 0x0B 0x09 0xE7 0x0F 0x14 0x06 0x21 0x03 0x33 0x52 0x06 0x42 
481:0:  0xBD 0x04 0x0A 0xE7 0x10 0x06 0xBD 0x0B 0x0B 0xE7 0x0F 0x14 0x06 0x20 0x20 0x33 0x54 0x05 0x67 
484:0:  0xBD 0x04 0x0C 0xE7 0x03 0x13 

496:0:  0xBD 0x04 0x0D 0xE7 0x07 0x16 
502:0:  0xBD 0x04 0x0E 0xE7 0x07 0x15 

mykey
529:0:  0xBD 0x0A 0x0F 0xE7 0x0B 0x00 0x01 0x12 0x34 0x56 0x78 0x1F 
537:0:  0xBD 0x06 0x10 0xE7 0x0C 0x00 0x01 0x03 
541:0:  0xBD 0x06 0x11 0xE7 0x0C 0x00 0x01 0x02 

565:0:  0xBD 0x04 0x12 0xE7 0x07 0x09 
571:0:  0xBD 0x04 0x13 0xE7 0x07 0x08 

640:0:  0xBD 0x04 0x14 0xE7 0x05 0x0D 
652:0:  0xBD 0x04 0x15 0xE7 0x07 0x0E 


power up already configured
679:0:  0xBD 0x04 0x01 0xE7 0x18 0x05 
684:0:  0xBD 0x04 0x02 0xE7 0x18 0x06 


minus pressed 
794:0:  0xBD 0x04 0x03 0xE7 0x0E 0x11 
817:0:  0xBD 0x04 0x04 0xE7 0x0E 0x16 

plus pressed

pin 6 delete

909:0:  0xBD 0x04 0x06 0xAA 0x0E 0x59 

pin 6 add

924:0:  0xBD 0x04 0x07 0xAA 0x0E 0x58 

pin 6 active 
1050:0: 0xBD 0x05 0x02 0xAA 0x18 0x01 0x4B 
1062:0: 0xBD 0x05 0x01 0xE7 0x09 0x46 0x53 
1069:0: 0xBD 0x07 0x02 0xE7 0x27 0x46 0x00 0x02 0x7E 

pin 8 active 
1107:0: 0xBD 0x04 0x01 0xE7 0x18 0x05 
1112:0: 0xBD 0x04 0x02 0xE7 0x18 0x06 
1193:0: 0xBD 0x04 0x03 0xE7 0x07 0x18 
1199:0: 0xBD 0x04 0x04 0xE7 0x07 0x1F 
1205:0: 0xBD 0x04 0x05 0xE7 0x09 0x10 
1254:0: 0xBD 0x0B 0x06 0xE7 0x0F 0x14 0x06 0x21 0x03 0x46 0x47 0x06 0x2D 
1256:0: 0xBD 0x04 0x07 0xE7 0x10 0x0B 0xBD 0x0B 0x08 0xE7 0x0F 0x14 0x06 0x20 0x20 0x46 0x49 0x05 0x0C 


lock
1540:0: 0xBD 0x04 0x09 0xE7 0x03 0x16 
unlock
1548:0: 0xBD 0x04 0x0A 0xE7 0x05 0x13 
status
1553:0: 0xBD 0x04 0x0B 0xE7 0x07 0x10 
1559:0: 0xBD 0x04 0x0C 0xE7 0x07 0x17 
add 1234
1575:0: 0xBD 0x0A 0x0F 0xE7 0x0B 0x00 0x01 0x12 0x12 0x34 0x34 0x17 
1583:0: 0xBD 0x06 0x10 0xE7 0x0C 0x00 0x01 0x03 
1587:0: 0xBD 0x06 0x11 0xE7 0x0C 0x00 0x01 0x02 

remove
1712:0: 0xBD 0x04 0x12 0xE7 0x0E 0x00 
add

bootup configured
2451:0: 0xBD 0x04 0x01 0xE7 0x18 0x05 
2456:0: 0xBD 0x04 0x02 0xE7 0x18 0x06 


bootup unconfigured
2552:0: 0xBD 0x04 0x01 0xE7 0x18 0x05 
2557:0: 0xBD 0x04 0x02 0xE7 0x18 0x06 

pin 6

remove button
1901:0: 0xBD 0x04 0x17 0xAA 0x0E 0x48 
add button

boot up 
2117:0: 0xBD 0x05 0x18 0xAA 0x07 0x02 0x4D 
2123:0: 0xBD 0x05 0x19 0xAA 0x07 0x02 0x4C 
2129:0: 0xBD 0x05 0x1A 0xAA 0x09 0x46 0x05 

lock
2179:0: 0xBD 0x04 0x1B 0xAA 0x0F 0x45 
2181:0: 0xBD 0x0B 0x1C 0xAA 0x10 0x14 0x06 0x20 0x21 0x02 0x14 0x05 0x52 0xBD 0x04 0x1D 0xAA 0x0F 0x43 
2189:0: 0xBD 0x07 0x07 0xE7 0x27 0x00 0x00 0x02 0x3D 0xBD 0x07 0x08 0xE7 0x27 0x00 0x00 0x81 0xB1 
2201:0: 0xBD 0x05 0x1F 0xAA 0x07 0x01 0x49 
2206:0: 0xBD 0x05 0x20 0xAA 0x07 0x01 0x76 

2229:0: 0xBD 0x04 0x21 0xAA 0x03 0x73 
2241:0: 0xBD 0x05 0x22 0xAA 0x07 0x01 0x74 
2247:0: 0xBD 0x05 0x23 0xAA 0x07 0x01 0x75 


unlock
2252:0: 0xBD 0x07 0x09 0xE7 0x27 0x00 0x00 0x82 0xB3 
2264:0: 0xBD 0x05 0x25 0xAA 0x07 0x02 0x70 
2269:0: 0xBD 0x05 0x26 0xAA 0x07 0x02 0x73 

mykey 1234
2299:0: 0xBD 0x08 0x0A 0xE7 0x29 0x00 0x01 0x01 0x00 0x33 
2307:0: 0xBD 0x0A 0x28 0xAA 0x0C 0x00 0x01 0x12 0x12 0x34 0x34 0x7A 
2310:0: 0xBD 0x0A 
2311:0: 0x29 0xAA 0x0C 0x00 0x01 0x12 0x12 0x34 0x34 0x7B 

bootup configured
2362:0: 0xFD 0x00 0x00 
2374:0: 0xBD 0x05 0x02 0xAA 0x18 0x01 0x4B 
2386:0: 0xBD 0x05 0x01 0xE7 0x09 0x46 0x53 
2392:0: 0xBD 0x07 0x02 0xE7 0x27 0x46 0x00 0x02 0x7E 


bootup unconfigured

2608:0: 0xBD 0x05 0x02 0xAA 0x18 0x01 0x4B 
2620:0: 0xBD 0x05 0x01 0xE7 0x09 0x46 0x53 
2626:0: 0xBD 0x07 0x02 0xE7 0x27 0x46 0x00 0x01 0x7D 

timeout on lock / alarm?
2672:0: 0xBD 0x06 0x03 0xE7 0x2A 0x01 0x00 0x36 


error?
46:0:   0xBD 0x05 0x02 0x55 0xBD 0x02 0x12 


status sending 0xBD 0x4 0x1B 0xE7 0x7 0x0 
394:1C: 0xBD 0x05 0x1B 0xAA 0x07 0x01 0x4D 
400:1C: 0xBD 0x05 0x01 0xE7 0x09 0x50 0x45 
406:1C: 0xBD 0x07 0x02 0xE7 0x27 0x50 0x00 0x01 0x6B 

check code sending 0xBD 0x06 0x20 0xE7 0x0C 0x00 0x1F 0x2D 
86:21:  0xBD 0x05 0x20 0x55 0x0C 0x0A 0x89 

pair_0 sending 0xBD 0x04 0x00 0xE7 0x18 0x04
4:1:    0xBD 0x05 0x00 0xAA 0x18 0x01 0x49
check code sending 0xBD 0x06 0x01 0xE7 0x0C 0x00 0x00 0x13
8:2:    0xBD 0x05 0x01 0x55 0x0C 0x0A 0xA8
check code sending 0xBD 0x06 0x02 0xE7 0x0C 0x00 0x01 0x11
16:3:   0xBD 0x0E 0x02 0xAA 0x0C 0x00 0x01 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x54
check code sending 0xBD 0x06 0x03 0xE7 0x0C 0x00 0x02 0x13


sequence of events needed for operation
register to device
if firstboot
   delete all codes
check codes
if missing
   add codes
check door status

AA is worthless and i still don't have my bag...


