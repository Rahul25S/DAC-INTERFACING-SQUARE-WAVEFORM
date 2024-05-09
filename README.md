
#include <LPC17xx.H>
#include "GLCD.H"
#define __FI        1                       /* Font index 16x24               */

void delay(void)		/* Delay Routine */
{
  unsigned char i;
  for(i=0;i<100;i++);
}

main()

{ 
	unsigned int i,j;
	
  LPC_SC->PCONP     |= (1 << 15);            /* enable power to GPIO & IOCON  */
 
	LPC_GPIO0->FIODIR |= 0x007F8000;          /* PortA and PortB as outputs */      
  LPC_GPIO1->FIODIR |= 0x07F80000;
	
	
	#ifdef __USE_LCD
  GLCD_Init();                               /* Initialize graphical LCD      */

  GLCD_Clear(White);                         /* Clear graphical LCD display   */
  GLCD_SetBackColor(Blue);
  GLCD_SetTextColor(White);
  GLCD_DisplayString(0, 0, __FI, "        ESA         ");
  GLCD_DisplayString(1, 0, __FI, "      Bangalore     ");
  GLCD_DisplayString(2, 0, __FI, "  www.esaindia.com  ");
  GLCD_SetBackColor(White);
  GLCD_SetTextColor(Blue);
  GLCD_DisplayString(5, 0, __FI, "     Dual DAC       ");
  GLCD_DisplayString(6, 0, __FI, "   Square Wave      ");
	 
#endif
	while(1)
  {
		for(i=0; i<0x0FF; i++)
	  {
		LPC_GPIO0->FIOPIN =(LPC_GPIO0->FIOPIN & 0xFF807FFF) | 0x007F8000;    //send High on PortA
		LPC_GPIO1->FIOPIN = (LPC_GPIO1->FIOPIN & 0xF807FFFF) | 0x07F80000;    //send High on PortB
			delay();
 	  }
    
	  for(j=0xFF; j>0; j--)
    {
		LPC_GPIO0->FIOPIN =(LPC_GPIO0->FIOPIN & 0xFF807FFF);                //send Low on PortA
		LPC_GPIO1->FIOPIN = (LPC_GPIO1->FIOPIN & 0xF807FFFF);                //send Low on PortB
			delay();
  	}
	}
}
