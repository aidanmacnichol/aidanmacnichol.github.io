---
layout: default
title: "Bare Metal Watch"
permalink: /bareMetalWatch/
---

# Bare Metal Watch
This is a personal project to practice my embedded skills as well as PCB design. The idea was to design and build a very simple digital watch completely from the ground up. There is very limited features on the watch mainly due to time constraints as I did this project while doing my Masters. 

Currently still in a work in progress, just waiting for my PCB's to be delivered. 

## Skills
- Embedded C/C++
- PCB design (KiCad)
- Soldering and HW debugging
- AVR Microcontrollers
- I2C

## Background
This project is based around the ATtiny85 Microcontroller. As it has limited GPIO I used two 8-bit shift registers to interface with the 4 digit, 7-segment display I am using. Additionally the The ATtiny does not have a RTC so a separate module is used in conjunction with an oscillator. 

Everything is done from scratch for this project except the I2C implementation. I tried to code my own bit banging solution but it proved too time consuming and difficult as I do not have access to an Oscilliscope or logic analyzer for debugging. 

## **Schematic:** 

![Schematic](/assets/images/schm.png)

## **PCB** Has gone through a few iterations. The first version was circular with a few glaring issues: 

1. Wrong resistor footprint (too small to hand solder) 
2. Band cuttouts placed wrong. 

![pcb1](/assets/images/pcb1.jpg)

The second design is just a simple rectangle which made routing a little easier. 

![pcb](/assets/images/pcb.png)

## **Code**

[Repository](https://github.com/aidanmacnichol/bareMetalWatch/tree/main/code/src)

To program the chip I used a sparkfun AVR programmer. 

Right now it is super barebones. Updating the LCD is done in the main loop and there are no interrupts. In the future I would like to add a button so the user can set and change the time themselves. 

The most difficult part was **interfacing with the RTC.** I used already implemented code called "USI_TWI_Master" for the I2C communication but implemented setting and reading the time myself according to the datasheet. 

    #include <avr/io.h>
    #include "rtc.h"
    #include <util/delay.h>
    #include "USI_TWI_Master.h"

    #define RTC_ADDRESS 0x68

    void rtc_init() {
        USI_TWI_Master_Initialise();
        
        uint8_t seconds = rtc_read(0x00); // Read the current seconds value
        seconds &= 0x7F;                  // Clear the CH bit (set it to 0)
        rtc_write(0x00, seconds);         // Write back to start the clock
    }

    static uint8_t dec2bcd(uint8_t val) {
        return ((val / 10) << 4) | (val % 10);
    }

    static uint8_t bcd2dec(uint8_t val) {
        return ((val >> 4) * 10) + (val & 0x0F);
    }

    void rtc_write(uint8_t reg, uint8_t value) {
        uint8_t msg[3];
        msg[0] = (RTC_ADDRESS << 1); // DS1307 address with write bit
        msg[1] = reg;                // Register to write to
        msg[2] = value;              // Value to write

        USI_TWI_Start_Read_Write(msg, 3); // Send message
        USI_TWI_Master_Stop();            // Send stop condition
    }

    void rtc_set_time(uint8_t min, uint8_t hour) {
        rtc_write(0x01, dec2bcd(min));  // Set minutes register
        rtc_write(0x02, dec2bcd(hour)); // Set hours register
    }

    uint8_t rtc_read(uint8_t reg) {
        uint8_t msg[2];
        msg[0] = (RTC_ADDRESS << 1); // DS1307 address with write bit
        msg[1] = reg;                // Register to read from

        USI_TWI_Start_Read_Write(msg, 2); // Send the register address
        USI_TWI_Master_Stop();            // Send stop condition after setting register address

        // Set up to read data back from the DS1307
        msg[0] = (RTC_ADDRESS << 1) | 0x01; // DS1307 address with read bit
        USI_TWI_Start_Read_Write(msg, 2);   // Read from DS1307
        USI_TWI_Master_Stop();              // Send stop condition after reading

        return msg[1]; // Return the read value
    }

    void rtc_read_time(uint8_t *min, uint8_t *hour) {
        *min = bcd2dec(rtc_read(0x01));   // Read and convert minutes
        *hour = bcd2dec(rtc_read(0x02));  // Read and convert hours
    }