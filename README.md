# ESP32 Arm DJI O3

Code from [here](https://github.com/ramiss/arduino_DJI_03_RC_ARM), this is just slightly adapted for my hardware. This allows enabling the full power on the O3 so you can use it for long range planes or cars. I though with the latest firmware was OK to override via the menu, but without arming it, the power is never the maximum the Air unit can deliver.

![image](https://github.com/user-attachments/assets/e314a554-8a70-4e70-adb2-ad0439fd0e38)

## What I am using

There is some minor details mising, like the gopro mount. But that is a personal preference. My idea is just to hook the whole video system to something when I want to drive them.

| Part        | Comments          |
|-------------|----------------------|
| [DJI O3 Air Unit](https://s.click.aliexpress.com/e/_DFMCnNp) | The camera
| [1800 mAh Original DJI Goggles 2 Battery](https://www.aliexpress.com/item/1005007042116539.html)      | Power bank|
| [Alloy Waterproof Case](https://s.click.aliexpress.com/e/_DlB1UcT)      | Enclosure for camera|
| [ESP32 30Pin-Type-C](https://s.click.aliexpress.com/e/_DmRukHz) | Microcontroller board, you have to desolder the pins |
| [JST 6P 1.25 extension](https://s.click.aliexpress.com/e/_DeUX0MT) | For connecting to the case connector |
| [JST 2P 2mm for the battery](https://s.click.aliexpress.com/e/_DC9pJQ7) | Can be any connector/cable |
| [USB-C to barrel](https://s.click.aliexpress.com/e/_DDsXURt) | To connect battery as usb-c |
| [Small switch 3mm handle](https://s.click.aliexpress.com/e/_DlFpkBD) | To enable the Arming |
| Heat shrink, super glue and tape | |
| 33 kOhm and 5.5 kOhm resistors | Optional for sensing the battery, not very interesting in the case of the DJI battery |
| Some translucent glue | For the "led indicators" |
| 3d printer and PLA | For the enclosure |

> [!TIP]  
> To prevent fogging on the camera (O3 gets insanely hot when it is fully armed), DarwinFPV has some instructions of glueing the camera module to the case. I used foam double sided tape. Works OK

## Wiring

The [source repo](https://github.com/ramiss/arduino_DJI_03_RC_ARM) has the wiring. From the DJI connector of the camera, I connected as follows:

| RED | BLACK | YELLOW | GREEN | BLUE | WHITE |
|-----|------|----------|-------|------|-------|
|  VIN (before the switch, so it does not affect the camera) | GND | D17 | D16 | GND | D18 |

> [!NOTE]  
> Since all cables are connected to the ESP32, if something does not work just modify the pins on `Serial1.begin(115200, SERIAL_8N1, 16, 18)`. If O3 is not detected then change the RX pin (16 in this case). If the O3 is detected but not ARMED, change the TX pin (18 in this case).

![image](https://github.com/user-attachments/assets/9ce7f696-b97e-4998-9752-6d0dc13da06f)

The voltage divider can be configured in the code in the `map(input, 1475, 1893, 76, 92)`, it is a simplified version of the original, just with a range I am getting from `analogRead(ANALOG_IN)` to the real voltage multiplied for 10. In this example, when `analogRead(ANALOG_IN) == 1475` my multimeter was saying `7.6` volts (`76` in the `map` function).

## Aseembly

The case has holes that align only when the ESP32 was installed with the USB to the left as shown in one of the photos below. 

### Led indicator

The device reports several states with the lights:

State | Red | Blue
--- | --- | ---
 Powered off | OFF  | OFF
 Powered on  | ON | OFF
 Initializing | ON | Very faint (5%)
 Waiting for O3 | ON | Faint (10%)
 ARMING | ON | ON

For the "led indicators" I used some UV activated glue, to try to keep the water out:

![image](https://github.com/user-attachments/assets/3ce6d0f7-11d1-4ce4-a453-e00443eb8e88)

Besides this, I used double side tape for the ESP32, glue both cables to the bottom case and the switch in the upper position.

![image](https://github.com/user-attachments/assets/9a6122da-0625-4415-aaba-33ca0724bd2e)

This is how it should look at the end:

![image](https://github.com/user-attachments/assets/66b95012-adcb-46da-8170-935ee653c663)
