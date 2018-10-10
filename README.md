# 30Mhz
基于stc15f104w小单片机的30Mhz频率的产生

##  
    #include <reg52.h>
    #include "intrins.h"
    sbit pwm=P3^4;

    typedef unsigned int u16;	  //对数据类型进行声明定义
    typedef unsigned char u8;

    void delay_us();
    void Timer0Init();

    void main()
    {
     Timer0Init(); //定时器0初始化

    while(1){

            INT0 = 1;                   //ready read INT0 port
            while (!INT0);              //check INT0
            _nop_();
            _nop_();
            IT0 = 1;                    //set INT0 int type (1:Falling only 0:Rising & Falling)
            IE0 = 0;                    //clear INT0 flag
            EX0 = 1;                    //enable INT0 interrupt

      pwm=0;
      delay_us();
      pwm=1;
      delay_us();
    }		  }


    void Timer0Init()
    {
      TMOD|=0X01;//选择为定时器0模式，工作方式1，仅用TR0打开启动。

      TH0=0XFC;	//给定时器赋初值，定时1ms
      TL0=0X18;	
      ET0=1;//打开定时器0中断允许
      EA=1;//打开总中断
      TR0=1;//打开定时器			
    }

    void delay_us()
    {
    unsigned int i,j,k;
    for(i=0;i<2;i++)
    for(j=0;j<4;j++)

    ;
    for(k=0;k<15;k++)
    ;
    }

    void exint0() interrupt 0           //interrupt 0 (location at 0003H)
    {
        EX0 = 0;                        //disable INT0 interrupt
        P1++;
        IE0 = 0;                        //clear INT0 flag
    }

    void Timer0() interrupt 1
    {
      static u16 i=0,j=0;
      TH0=0XFC;	//给定时器赋初值，定时1ms
      TL0=0X18;
      i++;
      if(i==5000)
      {
        i=0;
        j++;
        if(j==30)  {
             j=0;
          PCON = 0x02;                //MCU power down	
          }
      }	
    }
    
 ##  
