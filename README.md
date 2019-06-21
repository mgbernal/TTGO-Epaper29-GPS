# TTGO-Epaper29-GPS

Create a gps device using a TTGO ESP32 Epaper Board GPS module.  Parts used:

* https://www.aliexpress.com/item/32850386996.html
* https://www.aliexpress.com/item/32323993155.html (after purchasing, this is very unlikely to be an authentic uBlox chip)


Change an existing or create a new esp32 variant based on LOLIN32.  Change the following in the pins_arduino.h file:

```
static const uint8_t MISO = 19;
```
to
```
static const uint8_t MISO = 2;
```

Add following library using Arduino IDE Manage Libraries or Download Zip:

* https://github.com/ZinggJM/GxEPD
* https://github.com/mikalhart/TinyGPSPlus

Solder headers onto gps chip

Connect From ESP32 Badge side connector

1) Yellow to GPS GND
2) White to GPS RX
3) Brown to GPS TX
4) Red to VCC

