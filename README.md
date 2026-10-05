# SaludWatch
The SaludWatch is a monitoring aid device to help track temperature, blood oxy and heart rate while also showing you the time. 
These functions help elderly people track their vitals and know when to take medicine without having to walk to get a device, its simple design makes it easy to understand.

I came up with the idea thanks to my grandma(As weird as it may sound). It all started with my grandma having to climb the stairs to the third floor of my three-story house because she felt dizzy, it was too much! Plus it was dangerous,
one might say "But can't you just move the monitoring aid to the first floor so she doesn't have to go all the way up?" That's an option, yes, however, my grandma likes having her items organized, moving it to the first floor would just make her forget it's there in the first place.
That's when I thought of a great invention, what if the monitoring aid was ON her at ALL times? It would probably look ugly, however if I disguise it as another device which is on you MOST of the time, like a watch, then it would be better!
And that's how SaludWatch was born, the Wi-Fi, LED and buzzer functions were added later after discovering what the ESP32-WROOM-32 has(And can do).

<img width="1155" height="805" alt="image" src="https://github.com/user-attachments/assets/022b59c6-0d17-484c-96e8-9966c52b1d36" />

(Schematic of the SaludWatch)

## Now, let's get technical, how does it really work?

+ The sensors, display, buzzer, and buttons connect to an ESP32-WROOM-32 development board.
+ The board reads the sensors, draws everything on a small OLED display, and uses its built-in Wi-Fi to sync the time and send alerts.
+ A LiPo battery powers it through a TP4056 charging module with battery protection (Using the USB charger).
+ And a MT3608 step-up converter raises the battery voltage to the 5V the board needs.

```mermaid
graph TD;
    USBCharger-->TP4056;
    TP4056-->SlideSwitch;
    TP4056-->3.7VLiPobattery
    SlideSwitch-->MT3608;
    MT3608-->ESP32-WROOM-32;
    MAX30102-->|I2C|ESP32-WROOM-32;
    DS18B20-->|1-Wire|ESP32-WROOM-32;
    Buttons-->ESP32-WROOM-32;
    ESP32-WROOM-32-->|I2C|1.3inOLEDdisplay;
    ESP32-WROOM-32-->Buzzer+LED;
    ESP32-WROOM-32-->|Wi-Fi|AlertToFamily;
```

### Expected program flow
1. Boot, connect to Wi-Fi and sync to the local time zone then disconnect(Other modules are still active)
2. Time will show, the screen wakes up if any button is pressed.
3. Press the forward button to send a measurement request, wait 15-30 seconds after a "Keep still" notice, then show results on the OLED.
4. If the old person needs to take medicine soon or something is off, the buzzer will sound and the LED flash, an alert will ONLY be sent If something is off.
5. Otherwise the old person can check their vitals and then the device goes into energy-saving mode, wakes up on a button press.

## ESSENTIAL file locations

- `Hardware/` — 3D Case design and Schematic with wiring using labels
- `Hardware/Case/3DCase.f3d` — 3D Case design
- `Hardware/kicad/SaludWatch.kicad_pro` — Kicad Schematic + Wiring
- `Hardware/kicad/SaludWatch.kicad_sch` — Kicad Schematic + Wiring
- `Firmware/` — Code? Not yet, planned for the future functional prototype
- `Docs/` — Misc, non-editable files, diagrams and layout

## Layout + 3D design
The final third design of all my drawn designs, had to do a box because 3D modelling is hard ;(. (Especially considering I have never done it, I was happy once I found out I could make any weird shaped figure with the line and extrude tool!)
<img width="950" height="438" alt="image" src="https://github.com/user-attachments/assets/1b4e75eb-0630-4783-9687-22a0e49f0534" />

Board layout which I used to make the 3D Case model
<img width="772" height="545" alt="image" src="https://github.com/user-attachments/assets/cbe80eb7-acf7-46f1-8c93-33622b292007" />

Exterior view, here we can see the lid, walls and strap lugs. The lid has one hole for the OLED display and two square sized ones for the two buttons. At the strap lugs' side we can also see the slideSwitch hole and to the side another one for the DS18B20
<img width="820" height="613" alt="image" src="https://github.com/user-attachments/assets/877963da-5b32-4277-ad35-214c0625bde9" />

(Only-Base view, see that hole in the distance? That's for the ESP32-WROOM32, and also opposite hole to the DS18B20 hole is for the micro-usb charger for the TP4056)
<img width="854" height="581" alt="image" src="https://github.com/user-attachments/assets/327072af-cfd6-418b-91e9-ff8f85392ee7" />

Lid+2layer view. Here we can appreciate the beautifully modelled lid, I even made a fit-able lid using the join operation with the extrude tool! And about the two layers, that coin sized hole is for the connections between all the modules. The layers aren't really attached at all, there's an space of 0.3mm from all sides with the two shelves I made[You can see them in the second pic]

## Materials list
Prices were in Peruvian soles(Since I live in Peru) so I calculated the prices using the exchange rate of Sol -> USD:
| # | Part | Qty | Unit price ($) | Total ($) | Where to buy |
|---|---|---|---|---|---|
| 1 | ESP32-WROOM-32 38-pin dev board | 1 | $11.60 | $11.60 | [Nanoparuro](https://nanoparuro.com/shop/esp32-wroom-32-c-esp-32-placa-de-desarrollo-entrada-c-38-pines-wifi-bt-1604?search=ESP32&order=name+asc#attr=) |
| 2 | USB-TC 1 meter | 1 | 2.90 | 2.90 | [Nanoparuro](https://nanoparuro.com/shop/usb-tc-cable-usb-a-tipo-c-para-transferir-datos-de-1-metro-usb-tc-1610#attr=) |
| 3 | MAX30102 pulse oximeter module | 1 | 2.90 | 2.90 | [Nanoparuro](https://nanoparuro.com/shop/max30102-modulo-sensor-de-pulso-de-ritmo-cardiaco-pulsioximetro-pulsimetro-oximetro-max30102-1672?search=MAX30205&order=name+asc#attr=) |
| 4 | OLED 0.96in I2C display | 1 | 5.80 | 5.80 | [Nanoparuro](https://nanoparuro.com/shop/oled096-i2c-display-oled-0-96-pulgadas-i2c-oled096-i2c-1111?search=OLED&order=name+asc#attr=) |
| 5 | DS18B20 waterproof temperature probe | 1 | 2.90 | 2.90 | [Nanoparuro](https://nanoparuro.com/shop/ds18b20-cable-sensor-de-temperatura-digital-ds18b20-a-prueba-de-agua-55degc-a-125degc-304?search=DS18B20&order=name+asc#attr=) |
| 6 | TP4056 charger with protection | 1 | 1.42 | 1.42 | [Nanoparuro](https://nanoparuro.com/shop/tp4056-prot-tp4056-prot-cargador-con-proteccion-para-baterias-de-litio-3-7v-con-entrada-micro-usb-233?search=TP4056&order=name+asc#attr=) |
| 7 | MT3608 step-up converter | 1 | 1.60 | 1.60 | [Nanoparuro](https://nanoparuro.com/shop/mt3608-mt3608-conversor-dc-dc-step-up-ajustable-2-a-24-vsal-28v-242?search=MT3608&order=name+asc#attr=) |
| 8 | Slide switch KBB-20 | 1 | 0.35 | 0.35 | [Nanoparuro](https://nanoparuro.com/shop/kbb-20-switch-deslizable-2-posiciones-kbb-20-132?search=KBB-2&order=name+asc#attr=) |
| 9 | 3.7V 1000 mAh LiPo battery | 1 | 4.35 | 4.35 | [Nanoparuro](https://nanoparuro.com/shop/bateria-de-3-7v-litio-366?search=Bateria&order=name+asc#attr=550) |
| 10 | D830B Multimeter | 1 | 5.79 | 5.79 | [Nanoparuro](https://nanoparuro.com/shop/dt-830d-multimetro-digital-con-puntas-de-prueba-d830b-203?search=multimetro&order=name+asc#attr=) |
| 11 | Tax and shipping (estimate) | 1 | 9.00 | 9.00 | not bought yet |
| | **Total** | | | **$48.61** | **about $48.61** |

## Limitations
Some parts such as the ESP32 board, the battery, the step up, and DS18B20 are very large and are only meant to prove the functionality of the prototype, they will change in a later version(They are also the only ones I found available near my hometown). I might also have mentioned that a blood pressure module would be added, but since they are VERY expensive(40$) and exceed the budget, those will be totally optional.
