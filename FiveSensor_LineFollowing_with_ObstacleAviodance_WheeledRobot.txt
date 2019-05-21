                                         Line Follower Header File

//Header File 
#ifndef  __revobot_H                                     //custom header file creation
#define  __revobot_H
//-------------------------------------------------------
#include <adc.h>                                         //header files required
#include <stdio.h>
#include <stdlib.h>
#include <p18f4550.h>
#include <delays.h>
#include <pwm.h>
#include <i2c.h>
#include <math.h>
#include <xlcd.h>
//--------------------------------------------------------
#pragma udata                                             //code required for bootloading 
extern void  _startup (void);
#pragma code _RESET_INTERRUPT_VECTOR = 0x000800
void  _reset (void)
{
_asm goto _startup _endasm
}
#pragma code

#pragma code _HIGH_INTERRUPT_VECTOR = 0x000808
void _high_ISR (void)
{
;
}
#pragma code _LOW_INTERRUPT_VECTOR = 0x000818
void _low_ISR (void)
{
}
#pragma code

//-------------------------------------------------------
                                                        //constant defenitions
#define l0 PORTAbits.RA0	
#define l1 PORTAbits.RA1		
#define l2 PORTAbits.RA2
#define mode (PORTA&0b00000111)		
#define  ws 0
#define  bs 1
#define  ob 0
#define nob 1
#define  cw 0
#define  aw 1
#define pit 1
#define nopit 0
#define wall   0
#define nowall 1
#define light   1
#define nolight 0  
#define rightlsensor   PORTAbits.RA5		
#define leftlsensor    PORTEbits.RE0		
#define rightosensor   PORTEbits.RE1		
#define leftosensor    PORTEbits.RE2	
#define buzzer PORTAbits.RA3
#define bld            PORTBbits.RB4		
#define motorra  PORTCbits.RC1	
#define motorrb  PORTDbits.RD0
#define motorla  PORTCbits.RC2
#define motorlb  PORTCbits.RC0
#define motor_r_fwd   PORTCbits.RC1=1;PORTDbits.RD0=0	
#define motor_r_bwd   PORTCbits.RC1=0;PORTDbits.RD0=1
#define motor_l_fwd   PORTCbits.RC2=1;PORTCbits.RC0=0	
#define motor_l_bwd   PORTCbits.RC2=0;PORTCbits.RC0=1
#define motor_r_stp   PORTCbits.RC1=0;PORTDbits.RD0=0
#define motor_l_stp   PORTCbits.RC2=0;PORTCbits.RC0=0	
#define allanalog   OpenADC(ADC_FOSC_2 & ADC_12_TAD, ADC_CH4 & ADC_REF_VDD_VSS & ADC_INT_OFF, ADC_13ANA);
#define allipdigital  OpenADC(ADC_FOSC_2 & ADC_12_TAD, ADC_CH4 & ADC_REF_VDD_VSS & ADC_INT_OFF, ADC_0ANA);
int rightldr,leftldr,rightthreshold,leftthreshold;         //variables required for working of light follower


void initialize( void)
{
T2CON=0b00000110;
PR2=0b11111111;
CCPR1L = 0b00110011 ;
CCP1CON = 0b00111100 ;
CCPR2L = 0b00110011 ;
CCP2CON = 0b00111100 ;
OpenADC(ADC_FOSC_2 & ADC_12_TAD, ADC_CH4 & ADC_REF_VDD_VSS & ADC_INT_OFF, ADC_0ANA);

}




void speedirr(int r,int dir)                               //function for speed and direction control of right motor
{
if(dir==cw)
{
SetDCPWM1(r);
PORTCbits.RC0=0;
}
if(dir==aw)
{
SetDCPWM1(1023-r);
PORTCbits.RC0=1;
}


}


void speedirl(int l,int dir)                              //function for speed and direction control of left motor
{
if(dir==cw)
{
SetDCPWM2(l);
PORTDbits.RD0=0;

}
if(dir==aw)
{
SetDCPWM2(1023-l);
PORTDbits.RD0=1;

}


}

void  acquire_ldr_digital_values()                       //values acquiring digital for functioning as light follower
{
allanalog;


Delay10TCYx( 5 );                                        //to get right ldr vaue 
ConvertADC();         
while( BusyADC() );   
rightldr=ReadADC();  
SetChanADC(ADC_CH8);                                     //to change analog channel

Delay10TCYx( 5 );                                        //to get left ldr value   
ConvertADC();         
while( BusyADC() );   
leftldr=ReadADC();   
SetChanADC(ADC_CH12);                                     //to change analog channel

Delay10TCYx( 5 );                                        //to get right potentiometer value
ConvertADC();         
while( BusyADC() );  
rightthreshold=ReadADC();  
SetChanADC(ADC_CH9);

Delay10TCYx( 5 );     
ConvertADC();        
while( BusyADC() );   
leftthreshold=ReadADC();                                 //to get left potentiometer value 
SetChanADC(ADC_CH10);                                           //to change analog channel

if(rightldr<rightthreshold)
rightldr=1;                                              //compare ldr value with corresponding potentiometer
else
rightldr=0;
if(leftldr<leftthreshold)
leftldr=1;
else
leftldr=0;

allipdigital;
}

int convert2digital(int l)
{
switch (l)
{
case 0:SetChanADC(ADC_CH0);
        break;
case 1:SetChanADC(ADC_CH1);
        break;
case 2:SetChanADC(ADC_CH2);
        break;
case 3:SetChanADC(ADC_CH3);
        break;
case 4:SetChanADC(ADC_CH4);
        break;
case 5:SetChanADC(ADC_CH5);
        break;
case 6:SetChanADC(ADC_CH6);
        break;
case 7:SetChanADC(ADC_CH7);
        break;
case 8:SetChanADC(ADC_CH8);
        break;
case 9:SetChanADC(ADC_CH9);
        break;
case 10:SetChanADC(ADC_CH10);
        break;
case 11:SetChanADC(ADC_CH11);
        break;
case 12:SetChanADC(ADC_CH12);
        break;

}



                                    //to set analog channel                       
Delay10TCYx( 5 );                                       //delay to charge ADC's internal capacitor
ConvertADC();                                           //conversion
while( BusyADC() );                                     //check if conversion is over
return(ReadADC());                                      //return the 10 bit digital value
}    

#endif



------------------------------------------------------------------------------------------------------------------------------------------------------

					Line Following Code

// Code for line following
//-----------------------------------------------------------------
// header file for Line Follower
#include<linefollow.h>  	
void main(void)
{ 
//setting PORTA as inputs except PA3
TRISA=0b11110111;     
//setting PORTB as outputs
TRISB=0b00000000;     
//setting PORTC as outputs
TRISC=0b00000000;     
//setting PORTD as outputs
TRISD=0b00000000;     
//setting PORTE as outputs
TRISE=0b11111111;     
//Making the buzzer off
buzzer=0;      
/*Initializing adc, pwm modules and setting PA0, PA1, PA2  AND PA3 pins as analog.*/
initialize();         
// loop to perform line follower using 2 sensors
while(1)             
{
 // if line is between 2 sensors 
  if(rightlsensor==ws && leftlsensor==ws)                             
  { 
    // then move forward
    speedirr(512,cw);                                                                                 
    speedirl(512,cw);    
  }
  
// if the bot has drifted to the left of the line 
  if(rightlsensor==bs && leftlsensor==ws)                                         
  {
     // Then move the bot to the right
      speedirr(512,aw);                                                             
      speedirl(512,cw);    
   }
   // if the bot has drifted to the right of the line  
 if(rightlsensor==ws && leftlsensor==bs)                                    
{
  // then move the bot to the left
   speedirr(512,cw);                                                                                
   speedirl(512,aw);    
} 

 /* if the bot has encountered a black line or surface orthogonal to its path */
if(rightlsensor==bs && leftlsensor==bs)                                    {
   speedirr(0,cw);                                                                    
   // Then move stop
   speedirl(0,cw);    
}   
}
}
------------------------------------------------------------------------------------------------------------------------------------------------------

					Ardiuno-PIC Interfacing

// Arduino code
// This code will try to pass any received serial data to the serial logger built into the
//  Arduino IDE. It out puts the binary for 'a' which we will try to match with real
//  serial data from the PIC chip's output.

int ledPin = 13;                // built in led
int incomingByte = 'a';

void setup()
{
  Serial.begin(9600);
  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, HIGH);           // turn on the LED so we know when the program starts
  Serial.print("Good to go!\n");        // Let the serial monitor know when the program is
                                        //  running as well...
  Serial.print(incomingByte, BIN);      // Print 'a' in binary format. Hopefully our PIC's
                                        //  serial data will match this.
}

void loop()
{

  if (Serial.available() > 0) {
    while (Serial.available() > 0) {
       incomingByte = Serial.read();
       Serial.print(incomingByte, BIN);

    }
  Serial.println();
  }
} 


// PIC code
#include <16F690.h>
#fuses INTRC_IO, NOWDT, NOBROWNOUT, PUT, NOMCLR
#use delay(clock=4000000)
#use rs232(baud=9600, xmit=PIN_C1, rcv=PIN_C2, bits=8) // Set LED 2 (and the corresponding output) to the transmit pin

void main(void)
{
	while(1){
		output_c(0b00000001);	// turn on LED 0 on the board
		delay_ms(1000);		// wait 1 second
		output_c(0b00000000);	// turn the LED off
		delay_ms(1000);		// wait 1 second
		putc('a');  // Send the character 'a' to the serial out.
			    //  This should be '01100001' or '1100001'
                           //  when it gets to the serial monitor
	}
} 

------------------------------------------------------------------------------------------------------------------------------------------------------

					Light Dependent Resistir Code

int r,g,b;
void setup()
{
  Serial.begin(9600);
}

void loop()
{
  delayMicroseconds(100);//give 0 to flush all the outputs
  int ldr=analogRead(3);
  int perldr;
  perldr=ldr*100/1023;
  int i;
  i=perldr*255/100;
  analogWrite(3,i);
  analogWrite(5,i);
  analogWrite(6,i);
  delay(10);//give 0 to flush all the outputs
  
}

--------------------------------------------------------------------------------------------------------
				OBSTACLE AVOIDANCE ROBOT MOTION CODE

// Code for obstacle avoidance
//---------------------------------------------------------------------
#include<revobot.h>
// Header file for Revoboard
void main(void)
{
// Setting PORTA as inputs except PA3
TRISA=0b11110111;
// Setting PORTB as outputs
TRISB=0b00000000;
// Setting PORTC as outputs
TRISC=0b00000000;
// setting PORTD as outputs
TRISD=0b00000000;
// setting PORTE as outputs
TRISE=0b11111111;
// making the buzzer off
buzzer =0;
// initializing adc, pwm modules and setting PA0,PA1,PA2 AND
PA3 pins as analog
initialize();
// loop to perform obstacle avoidance
while(1)
{
// if the bot has encountered an obstacle normally
if(rightosensor == ob && leftosensor == ob)
{
// first move the bot backwards
speedirr(1000,aw);
speedirl(1000,aw);
// for a small amount of time
Delay10KTCYx(2);
// Until the left sensor avoids the obstacle
while(leftosensor==0)
{
// turn to the the right to check for a path
speedirr(1000,aw);
speedirl(1000,cw);
}
}
// if the left sensor detects an obstacle
if(rightosensor==nob && leftosensor==ob)
{
// then turn to the right to avoid it
speedirr(1000,aw);
speedirl(1000,cw);
}
// if the right sensor detects an obstacle
if(rightosensor==ob && leftosensor==nob)
{
// then turn to the left to avoid it
speedirr(1000,cw);
speedirl(1000,aw);
}
// if both sensors dont detect any obstacle
if(rightosensor==nob && leftosensor==nob)
{
// then move forward
speedirr(1000,cw);
speedirl(1000,cw);
}
}
}
