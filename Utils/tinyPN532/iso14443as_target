/* 
 *  Author: Salvador Mendoza (salmg.net)
 *  Project: Original code from NFCopy85
*/
#define PN532_SCK  (0)
#define PN532_MOSI (2)
#define PN532_SS   (3)
#define PN532_MISO (1)
#define LED   (4)

#include "tinyPN532.h" //shortened version of PN532 Adafruit library

const uint8_t ppse[] = {0x8E,0x6F,0x23,0x84,0x0E,0x32,0x50,0x41,0x59,0x2E,0x53,0x59,0x53,0x2E,0x44,0x44,0x46,0x30,0x31,0xA5,0x11,0xBF,0x0C,0x0E,0x61,0x0C,0x4F,0x07,0xA0,0x00,0x00,0x00,0x03,0x10,0x10,0x87,0x01,0x01,0x90,0x00};

uint8_t Adafruit_PN532::setDataTarget(uint8_t cmd){
  if (cmd == 1){
    if (!sendCommandCheckAck(ppse, sizeof(ppse)))
      return false;
  }
  // read data packet
  readdata(pn532_packetbuffer, 8);                 //unnecessary but could open the door for new processing projects
}

void setup(){
  pinMode(LED, OUTPUT);
  delay(300);
  blink(LED, 100, 1);
  nfc.begin();                                     //initialize the PN532
  uint32_t versiondata = nfc.getFirmwareVersion(); //check PN532 version(ping!)
  if (!versiondata) {
    while(1){
      blink(LED, 50, 6);
      delay(200);
    }
  }
  nfc.setPassiveActivationRetries(0xFF);           //infinite num of retries
  nfc.SAMConfig();                                 //type of cards
}

void setData(int value){                           //communication function!
   nfc.setDataTarget(value);                       //send data to PN532
   nfc.getDataTarget();                            //request data from PN532
}

void loop(){
  blink(LED, 150, 3);
  nfc.AsTarget();                                  //in every loop, run as-target
  nfc.getDataTarget();                             //clean PN532 buffer 
  setData(1);                                      //process data in PN532

  blink(LED, 50, 2);
  delay(1000);
}
