---
title: Component Selection
---

## __My Role in the Project__
My role in this project is to provide exhibit-goers with a Human Machine Interface (HMI) that allows them to take control of the display's actuator. Please refer to [my team's webpage](https://asu-egr314-2025-s-201.github.io/02-Ideation%20and%20Concept%20Generation/) for a more detailed descripton of the product. My goal is to allow the user to navigate through the interface with three seperate buttons for movement and selection. Inputs should be reflected on the interface, which is done through a small OLED LCD display. In regards to my communication responsibilities, I am responsible for sending UART commands to my team members in charge of actuators and internet as the stepper motor should move on user command and display information on a mobile device respectively.

## __Table 1: Selecting a Pushbutton__
Potential Solutions | Pros | Cons
--------------------|----------|------
<img src="https://mm.digikey.com/Volume0/opasdata/d220001/medias/images/316/PTS636-Gull-Wing.jpg" alt="Button #1" width="200" height="150"> <br> Option #1: PTS636SM43SMTR LFS Surface Mount Push Button ($0.23)<br> [link to product](https://www.digikey.com/en/products/detail/c-k/PTS636SM43SMTR-LFS/10071723) | - Good PCB Contact <br> - Tactile <br> - Inexpensive | - Long Shipping Time <br> - Small for User Interactivity
<img src="https://m.media-amazon.com/images/I/71zbJf55BXL._AC_SL1500_.jpg" alt="Button #2" width="200" height="150"> <br> Option #2: Gikfun Tactile Surface Mount Push Button ($8.68) <br> [link to product](https://www.amazon.com/Gikfun-12x12x7-3-Tactile-Momentary-Arduino/dp/B01E38OS7K) | - Fun and Colorful for Exhibit <br> - Tactile <br> - Fast Shipping | - Expensive <br> - Comes in Bulk <br> - Must Reorientate Pins for PCB
<img src="https://mm.digikey.com/Volume0/opasdata/d220001/medias/images/516/MFG_K8.jpg" alt="Button #3" width="200" height="150"> <br> Option #3: K8WH41G LFS Tactile Surface Mount Push Button w/ LED ($6.45) <br> [link to product](https://www.digikey.com/en/products/detail/c-k/K8WH41G-LFS/4176636?s=N4IgTCBcDaINIA4DqAJALARgOIgLoF8g) | - LED for Exhibit Experience <br> - Tactile | - Long Shipping Time <br> - Extra Pins for LED Control <br> - Must Buy Other Buttons for Different LED Color

__Choice: Option #2: Gikfun Tactile Surface Mount Push Button__ <br>
__Rationale:__ This option was selected due to the extreme amount of customizability available. Considering the team's STEM exhibit works closely with color recognition, it was important to select something that works well within the theme. Also, these buttons funtion to provide excellent tactile feedback while offering a level of interactivity other options cannot. While it's true that this option was the most expensive, it remains in the range of budget.

## __Table 2: Selecting an OLED Screen__
Potential Solutions | Pros | Cons
--------------------|----------|------
<img src="https://m.media-amazon.com/images/I/41qBPSM9XqL._AC_SY450_.jpg" alt="OLED #1" width="200" height="150"> <br> Option #1: Songhe 0.96inch OLED LCD Display Board (~$2.00 per board)<br> [link to product](https://www.amazon.com/Songhe-0-96-inch-I2C-Raspberry/dp/B085WCRS7C/) | - I2C Serial Communication <br> - 5 Pack <br> - Provided from ASU | - No Price Listed <br> - Small Resolution <br> - Might be Difficult for Users to See
<img src="https://mm.digikey.com/Volume0/opasdata/d220001/medias/images/5481/MFG_WEA012864DBPP3N00003.jpg" alt="OLED #2" width="200" height="150"> <br> Option #2: Winstar Graphic LCD Display Module ($7.02) <br> [link to product](https://www.digikey.com/en/products/detail/winstar-display/WEA012864DBPP3N00003/20533257?s=N4IgTCBcDaIOoFECCAGAjGAHANgCwBEAhABWIGYA5FalMkAXQF8g) | - I2C Serial Communication <br> - Easy Communication w/ ESP32 <br> | - Small Resolution <br> - Not Provided from ASU <br> - Might be Difficult for Users to See
<img src="https://m.media-amazon.com/images/I/61XoEybK7YL._AC_SL1010_.jpg" alt="OLED #3" width="200" height="150"> <br> Option #3: HiLetgo 0.91inch I2C Serial OLED LCD Display ($6.49) <br> [link to product](https://www.amazon.com/HiLetgo-Serial-Display-SSD1306-Arduino/dp/B01N0KIVUX/ref=sr_1_1?crid=U6CMNNUE3S1R&dib=eyJ2IjoiMSJ9.AJb-I67N7TP6meO9EMPF6J1ZkDYELWiav4UO60YOqPavIi9KIwhvuc2U34QCZpVdz2eeWTXB255DvI-X8_APnQ.CHhMrB__sr00Pyb42g_DVkgdrkJmFBLo_I_o6ejjz9s&dib_tag=se&keywords=HiLetgo+i2c+91+inch+display&qid=1738957024&sprefix=hiletgo+i2c+91+inch+displa%2Caps%2C389&sr=8-1) | - I2C Serial Communication <br> - Rectangular Display <br> - Fast Shipping | - Small Resolution <br> - Not Provided from ASU

__Choice: Option #1: Songhe 0.96inch OLED LCD Display Board__ <br>
__Rationale:__ Overall, all solutions in this table were very similar and varied only by price and display size. The reason option #1 was selected was due to the fact that ASU provides the screen, which saves money and shipping time. Familiarity with this device will be gained through in-class labs as well.

## __Table 3: Selecting a Voltage Regulator__
Potential Solutions | Pros | Cons
--------------------|----------|------
<img src="https://mm.digikey.com/Volume0/opasdata/d220001/medias/images/4895/31%7ESOT223%7EE%2CG%2CH%7E4.JPG" alt="Regulator #1" width="200" height="150"> <br> Option #1: AP2114H-3.3TRG1 Linear Voltage Regulator ($0.61) <br> [link to product](https://www.digikey.com/en/products/detail/diodes-incorporated/AP2114H-3-3TRG1/4470756?s=N4IgTCBcDaIIIAUwEZkBYASBaAzAOhwBUAlAcWRAF0BfIA) | - Meets Requirements <br> - Inexpensive <br> - Compact Design | - Requires External Components <br> - 6V Input <br> - Linear, not Adjustable
<img src="https://mm.digikey.com/Volume0/opasdata/d220001/medias/images/2802/497%7E8SOIC-3.9%7E%7E8.jpg" alt="Regulator #2" width="200" height="150"> <br> Option #2: L6981C33DR - Buck Switching Regulator ($2.80) <br> [link to product](https://www.digikey.com/en/products/detail/stmicroelectronics/L6981C33DR/16841475) | - High Efficiency <br> - Adjustable and Fixed Capabilities <br> - Compact Design | - Requires External Components <br> - Expensive for Regulator <br>
<img src="https://mm.digikey.com/Volume0/opasdata/d220001/derivates/6/003/213/095/600%7ESOT23-5%7E%7E5_web%28640x640%29.jpg" alt="Regulator #3" width="200" height="150"> <br> Option #3: SC189ZSKTRT - 3.3V Buck Switching Regulator ($1.89) <br> [link to product](https://www.digikey.com/en/products/detail/semtech-corporation/SC189ZSKTRT/2182360) | - High Efficiency <br> - Inexpensive <br> - Compact Design | - Requires External Components <br> - Soldering Might be Difficult <br>

__Choice: Option #2: L6981C33DR - 3.3V Buck Switching Regulator__ <br>
__Rationale:__ This solution was selected as it meets all project requirements with high efficiency as well. While it does require extra components, the datasheet helps identify a proper setup for good results. Even though this option is expensive for a regulator, this is a necessary tradeoff to take as it implements functionality better than other options. 

## __Table 4: Selecting a Power Supply__
Potential Solutions | Pros | Cons
--------------------|----------|------
<img src="https://mm.digikey.com/Volume0/opasdata/d220001/medias/images/4838/WR9HD1333CCP-F%28R6B%29.jpg" alt="Power Supply #1" width="200" height="150"> <br> Option #1: WR9HD1333CCP-F(R6B) - 9V 12 W AC/DC External Wall Mount ($3.94) <br> [link to product](https://www.digikey.com/en/products/detail/globtek-inc/WR9HD1333CCP-F-R6B/13245472) | - Barrel Jack Output <br> - Inexpensive <br> | - 1.33A (Sufficient for ESP32, Might Need More Room for Other Peripherals) <br> - Long Shipping Time
<img src="https://mm.digikey.com/Volume0/opasdata/d220001/medias/images/2268/MFG_WSUxxx-Series.jpg" alt="Power Supply #2" width="200" height="150"> <br> Option #2: WSU090-2000-13 - 9V 18 W AC/DC External Wall Mount ($12.15)  <br> [link to product](https://www.digikey.com/en/products/detail/triad-magnetics/WSU090-2000-13/6600197) | - Barrel Jack Output <br> - 2A Supply <br> | - Expensive <br> - Long Shipping Time <br> - Bulky Design
<img src="https://m.media-amazon.com/images/I/31UE8SX03rS._AC_.jpg" alt="Power Supply #3" width="200" height="150"> <br> Option #3: BestCH 9V 3.0A AC Power Supply Adapter ($4.52) <br> [link to product](https://www.amazon.com/gp/product/B09ZTKTLGW/) | - Barrel Jack Output <br> - 3A Supply <br> - Provided by ASU <br> | - Bulky Design <br> - Datasheet not Provided

__Choice: Option #3: BestCH 9V 3.0A AC Power Supply Adapter__ <br>
__Rationale:__ The rationale behind this selection is the fact that is can provide up to 3.0A of current for the subsystem. This is more than enough to allow for complete functionality. Furthermore, this component is provided by ASU and allows finances to be focused into other areas. Although this design is bulky, it does the best at meeting project requirements. 

## __Microcontroller Used: ESP32-S3-WROOM-1-N4__
ESP Info                                      | Answer |
--------------------------------------------- | ------ |
Model                                       | S3-WROOM-1-N4
Product Page URL                            | [link](https://www.espressif.com/en/products/modules)
ESP32-S3-WROOM-1-N4 Datasheet URL           | [link](https://www.espressif.com/sites/default/files/documentation/esp32-s3-wroom-1_wroom-1u_datasheet_en.pdf)
ESP32 S3 Datasheet URL                      | [link](https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf)
ESP32 S3 Technical Reference Manual URL     | [link](https://www.espressif.com/sites/default/files/documentation/esp32_technical_reference_manual_en.pdf)
Vendor link                                 | [link](https://www.digikey.com/en/products/detail/espressif-systems/ESP32-S3-WROOM-1-N4/16162639)
Code Examples                               | [Arduino Example w/ LCD](https://randomnerdtutorials.com/esp32-esp8266-i2c-lcd-arduino-ide/) <br> [StackExchange Forum](https://stackoverflow.com/questions/79141161/esp32-s3-geek-lcd-screen-programming)
External Resources URL(s)                   | [Expressif Tutorial](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/get-started/index.html) <br> [YouTube Tutorial (Good w/ BLE)](https://www.youtube.com/watch?v=UooqZeFWRoE)
Unit cost                                   | $2.95
Absolute Maximum Current for entire IC      | 1500mA
Supply Voltage Range                        | 3.0V - 3.6V (3.3V is Typical)
Absolute Maximum current <br> (for VDD3P3)  | 500mA
Maximum GPIO current <br> (per pin)         | 40mA
Supports External Interrupts?               | Yes
Required Programming Hardware, Cost, URL    | [Schematic Checklist](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32s3/schematic-checklist.html#) <br> [PCB Layout Design](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32s3/pcb-layout-design.html) <br> [Hardware Development](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32s3/hardware-development.html)

### __Pin Layout__
<img src="https://wiki.autosportlabs.com/images/a/aa/Microcontroller_pin_layout.png" alt="Pin Layout" width="500" height="750">

#### __Pin Allocation__
Peripheral | Pin Assignment (Name, Number)
-----------|----------------
Power   | 3V3, 2
Ground  | GND, 1 <br> GND, 40 <br> GND, 41
USB     | IO19, 13 (D-) <br> IO20, 14 (D+)
UART    | IO17, 10 (Tx) <br> TX18, 11 (Rx)
I2C     | IO21, 23 (SCL) <br> IO47, 24 (SDA)
GPIO    | IO4, 4 <br> IO5, 5 <br> IO6, 6

## __Summary Table (All Components Selected)__
Component Selected | Image
-------------------|------
ESP32-S3-WROOM-1-N4 Microcontroller ($2.95)| <img src="https://mm.digikey.com/Volume0/opasdata/d220001/medias/images/4257/MFG_Attachment-2-ESP32-S3-WROOM-1.jpg" alt="Pin Layout" width="200" height="200">
BestCH 9V 3.0A AC Power Supply Adapter ($4.52) | <img src="https://m.media-amazon.com/images/I/31UE8SX03rS._AC_.jpg" alt="Power Supply #3" width="200" height="150">
L6981C33DR - Buck Switching Regulator ($2.80) | <img src="https://mm.digikey.com/Volume0/opasdata/d220001/medias/images/2802/497%7E8SOIC-3.9%7E%7E8.jpg" alt="Regulator #2" width="200" height="150">
Songhe 0.96inch OLED LCD Display Board (~$2.00 per board) | <img src="https://m.media-amazon.com/images/I/41qBPSM9XqL._AC_SY450_.jpg" alt="OLED #1" width="200" height="150">

### __Power Budget__
![HMI Power Budget](static/images/PowerBudget.png)

The power budget is extremely useful in providing viable component options that  safely operate the entire board. This process acts as confirmation and reassurance, while eliminating overall risks of damaging devices. 


