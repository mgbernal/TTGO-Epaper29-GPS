// include library, include base class, make path known
#include <GxEPD.h>
#include <GxIO/GxIO_SPI/GxIO_SPI.h>
#include <GxIO/GxIO.h>

#include <TinyGPS++.h>
TinyGPSPlus gps;  // The TinyGPS++ object

#define RXD2 22
#define TXD2 21

#include <Fonts/FreeMonoBold12pt7b.h>
#define DEFALUT_FONT FreeMonoBold12pt7b

#include "board_def.h"

typedef enum {
    RIGHT_ALIGNMENT = 0,
    LEFT_ALIGNMENT,
    CENTER_ALIGNMENT,
} Text_alignment;

GxIO_Class io(SPI, ELINK_SS, ELINK_DC, ELINK_RESET);
GxEPD_Class display(io, ELINK_RESET, ELINK_BUSY);

float latitude , longitude;
String lat_str , lng_str, location_str;

void displayInit(void){
    static bool isInit = false;
    if (isInit) {
        return;
    }
    isInit = true;
    display.init(115200);
    display.setRotation(1);
    display.eraseDisplay();
    display.setTextColor(GxEPD_BLACK);
    display.setFont(&DEFALUT_FONT);
    display.setTextSize(0);
    display.fillScreen(GxEPD_WHITE);
    display.update();
}

void displayText(const String &str, int16_t y, uint8_t alignment){
    int16_t x = 0;
    int16_t x1, y1;
    uint16_t w, h;
    display.setCursor(x, y);
    display.getTextBounds(str, x, y, &x1, &y1, &w, &h);

    switch (alignment) {
    case RIGHT_ALIGNMENT:
        display.setCursor(display.width() - w - x1, y);
        break;
    case LEFT_ALIGNMENT:
        display.setCursor(0, y);
        break;
    case CENTER_ALIGNMENT:
        display.setCursor(display.width() / 2 - ((w + x1) / 2), y);
        break;
    default:
        break;
    }
    display.println(str);
}

void setup(){
    Serial.begin(115200);
    delay(500);
    Serial2.begin(9600, SERIAL_8N1, RXD2, TXD2);
    displayInit();
}

void loop(){
    while (Serial2.available()) {
        if (gps.speed.isUpdated()){
          showPartialUpdate(gps.speed.mph());
        }
        if (gps.course.isUpdated()){
          showPartialUpdateCourse(gps.course.deg());
        }
        smartDelay(200);
    }
}

static void smartDelay(unsigned long ms){
  unsigned long start = millis();
  do{
    while (Serial2.available())
      gps.encode(Serial2.read());
  } while (millis() - start < ms);
}

void showPartialUpdate(float value){
  uint16_t box_x = 10;
  uint16_t box_y = 15;
  uint16_t box_w = 120;
  uint16_t box_h = 20;
  uint16_t cursor_y = box_y + box_h - 6;
  display.fillRect(box_x, box_y, box_w, box_h, GxEPD_WHITE);
  display.updateWindow(box_x, box_y, box_w, box_h, true);
  display.setFont(&FreeMonoBold12pt7b);
  display.setTextColor(GxEPD_BLACK);
  display.fillRect(box_x, box_y, box_w, box_h, GxEPD_WHITE);
  display.setCursor(box_x, cursor_y);
  display.print(value, 2);
  display.updateWindow(box_x, box_y, box_w, box_h, true);
}

void showPartialUpdateCourse(float value){ 
  uint16_t box_x = 10;
  uint16_t box_y = 50;
  uint16_t box_w = 120;
  uint16_t box_h = 20;
  uint16_t cursor_y = box_y + box_h - 6;
  display.fillRect(box_x, box_y, box_w, box_h, GxEPD_WHITE);
  display.updateWindow(box_x, box_y, box_w, box_h, true);
  display.setFont(&FreeMonoBold12pt7b);
  display.setTextColor(GxEPD_BLACK);
  display.fillRect(box_x, box_y, box_w, box_h, GxEPD_WHITE);
  display.setCursor(box_x, cursor_y);
  display.print(value, 1);
  display.updateWindow(box_x, box_y, box_w, box_h, true);
}
