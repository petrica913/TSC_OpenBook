# 📘 OpenBook - EBook Reader with ESP32-C6

## Block diagram


## 🧠 Detailed Hardware Description

### Microcontroller
- **ESP32-C6-WROOM-1-N8** – Main MCU with WiFi 6 and BLE 5.0 support.
- Manages all components and communication interfaces.

### Power Supply
- **USB-C Connector (J2)** – for powering and charging the device.
- **MCP73831** – LiPo battery charging IC.
- **P-Channel MOSFETs (Q1, Q2)** – for automatic switching between USB and battery power.
- **Varistor and 5.1kΩ resistors** – for overvoltage protection and USB-C detection.

### Display
- **eInk display** controlled via **SPI** interface:
  - Lines: CLK, MOSI, CS, DC (Data/Command), RST (Reset), BUSY (status).
  - Extremely power-efficient – consumes power only during screen refresh.

### User Interface
- Physical buttons for menu control:
  - Left/Right navigation and Select.
  - Connected to GPIOs configured as input, some with pull-up/down resistors.

### Sensors (optional)
- **BME680** – temperature, humidity, pressure, and VOC sensor.
  - Communicates via I2C.
  - Connected to GPIOs with 10kΩ pull-up resistors.

### Other Components
- Resistors, filtering and decoupling capacitors.
- Status LEDs (optional).

## 🔋 Estimated Power Consumption

| Component                                | Estimated Current             |
|------------------------------------------|-------------------------------|
| ESP32-C6 (CPU active, without Wi-Fi)     | ~80–120 mA                    |
| E-Paper (page refresh)                   | ~10–15 mA *(only during refresh)* |
| MicroSD (active access)                  | ~30–60 mA                     |
| RTC DS3231                               | ~0.2 mA                       |
| BME688 Sensor (1Hz)                      | ~3.7 µA                       |
| Fuel Gauge MAX17048 (active)             | ~23 µA                        |
| LDO Regulator (quiescent current)        | ~1 µA                         |
| **Total estimated (without refresh)**    | **~110–130 mA**               |
| **Total during EPD refresh**             | **~140–170 mA**               |


## 🔌 Pin Allocation of ESP32-C6

| Pin Number | GPIO | Function              | Componenta                            | Reason for Use                                   |
|------------|------|------------------------|---------------------------------------|--------------------------------------------------------|
| 3          | EN   | Reset                  | Reset Button                          | Hardware-defined enable pin                            |
| 8          | GPIO0 | SQW/INT               | RTC DS3231SN Interrupt                | Used to wakeup from sleep using an interrupt from RTC  |
| 9          | GPIO1 | 32KHz input           | RTC DS3231SN 32KHz output             | Low power timing source for RTC operations             |
| 27         | GPIO2 | MISO                  | SD Card, E-Paper, NOR Flash           | SPI MISO - receives data from connected peripherals    |
| 28         | GPIO3 | EPD_BUSY              | E-Paper Display busy signal           | Indicates the status of the E-Paper display            |
| 30         | GPIO4 | SD_CS                 | SD Card chip select                   | Selects the SD card on the SPI bus                     |
| 31         | GPIO5 | EPD_DC                | E-Paper Display data/command          | Distinguishes data/command                             |
| 32         | GPIO6 | SCK                   | SD Card, E-Paper, NOR Flash           | SPI clock line                                         |
| 33         | GPIO7 | MOSI                  | SD Card, E-Paper, NOR Flash           | SPI data line                                          |
| 10         | GPIO8 | Pull-up               | Reserved - Unused                     | Left free                                              |
| 13         | GPIO9 | Boot                  | Boot Button                           | Enter flashing mode                                    |
| 14         | GPIO10 | EPD_CS               | E-Paper Display chip select           | Selects E-Paper                                        |
| 15         | GPIO11 | FLASH_CS             | NOR Flash chip select                 | Selects external flash                                 |
| 16         | GPIO12 | USB_D-               | USB Data negative                     | USB data line                                          |
| 17         | GPIO13 | USB_D+               | USB Data positive                     | USB data line                                          |
| 23         | GPIO15 | Navigation           | Change Button                         | Input detection                                        |
| 25         | GPIO16 | TX                   | Test Pad                              | TX                                                     |
| 24         | GPIO17 | RX                   | Test Pad                              | RX                                                     |
| 16         | GPIO18 | RTC_RST              | RTC reset                             | Resets RTC module                                      |
| 17         | GPIO19 | I2C_PW               | BME688 power control                  | Powers I2C modules                                     |
| 18         | GPIO20 | EPD_3V3              | DMG2305UX-7                           | Controls power supply to E-Paper display               |
| 19         | GPIO21 | SDA                  | I2C data (RTC, BME688, Battery Gauge) | I2C data line                                          |
| 20         | GPIO22 | SCL                  | I2C clock (RTC, BME688, Battery Gauge)| I2C clock line                                         |
| 21         | GPIO23 | EPD_RST              | E-Paper Display reset                 | Resets the display                                     |
