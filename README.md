

#include "F28x_Project.h"     // Device Headerfile and Examples Include File


#define EPWM_TIMER_TBPRD 4999 ;
#define EPWM_TIMER_TBSTEP 499 ;
//###########################################################################
// Global variables used in the project before main function. Declare other     // variables in this section of the code.
//###########################################################################


//###########################################################################
// Declare the service routines and functions
//###########################################################################


// interrupt void isr1(void);
// void program1(void);
// void program2(void);
void clockwise(Uint16 cycles, Uint16 delay);
void half_clockwise(Uint16 cycles, Uint16 delay);
void counter_clockwise(Uint16 cycles, Uint16 delay);
void stop(Uint16 delay);
void LED_red();
void LED_blue();
void LED_green();
void LED_off();


interrupt void xint1_isr(void);

Uint16 reset = 0;
Uint16 fast = 2200;
Uint16 slow = 2800;


//######################   Beginning of the main code #######################


void main(void)
{

// Initialize System Control:
// PLL, WatchDog, enable Peripheral Clocks
// This example function is found in the F2837xD_SysCtrl.c file.
    InitSysCtrl();


// Clear all interrupts and initialize PIE vector table:
// Disable CPU interrupts
    DINT;

// Initialize the PIE control registers to their default state.
// The default state is all PIE interrupts disabled and flags
// are cleared.
// This function is found in the F2837xD_PieCtrl.c file.
    InitPieCtrl();

// Disable CPU interrupts and clear all CPU interrupt flags:
    IER = 0x0000;
    IFR = 0x0000;

// Initialize the PIE vector table with pointers to the shell Interrupt
// Service Routines (ISR).
// This will populate the entire table, even if the interrupt
// is not used in this example.  This is useful for debug purposes.
// The shell ISR routines are found in F2837xD_DefaultIsr.c.
// This function is found in F2837xD_PieVect.c.
    InitPieVectTable();


// Enable global Interrupts and higher priority real-time debug events:
    //EINT;  // Enable Global interrupt INTM
    //ERTM;  // Enable Global realtime interrupt DBGM



//###########################################################################
// User specific code: Your code comes here.
//###########################################################################


    //LED SETUP:
    EALLOW;

        GpioCtrlRegs.GPAGMUX1.bit.GPIO0 = 0;
        GpioCtrlRegs.GPAMUX1.bit.GPIO0 = 0;
        GpioCtrlRegs.GPADIR.bit.GPIO0 = 1;

        GpioCtrlRegs.GPAGMUX1.bit.GPIO2 = 0;
        GpioCtrlRegs.GPAMUX1.bit.GPIO2 = 0;
        GpioCtrlRegs.GPADIR.bit.GPIO2 = 1;

        GpioCtrlRegs.GPAGMUX1.bit.GPIO13 = 0;
        GpioCtrlRegs.GPAMUX1.bit.GPIO13 = 0;
        GpioCtrlRegs.GPADIR.bit.GPIO13 = 1;


    EDIS;

    EALLOW;
    PieVectTable.XINT1_INT = &xint1_isr;
    EDIS;

    EALLOW;

    GpioCtrlRegs.GPAGMUX2.bit.GPIO16 = 0;
    GpioCtrlRegs.GPAMUX2.bit.GPIO16 = 0;
    GpioCtrlRegs.GPADIR.bit.GPIO16 = 1;

    GpioCtrlRegs.GPAGMUX2.bit.GPIO31 = 0;
    GpioCtrlRegs.GPAMUX2.bit.GPIO31 = 0;
    GpioCtrlRegs.GPADIR.bit.GPIO31 = 1;

    GpioCtrlRegs.GPBGMUX1.bit.GPIO34 = 0;
    GpioCtrlRegs.GPBMUX1.bit.GPIO34 = 0;
    GpioCtrlRegs.GPBDIR.bit.GPIO34 = 1;

    GpioCtrlRegs.GPBGMUX1.bit.GPIO41 = 0;
    GpioCtrlRegs.GPBMUX1.bit.GPIO41 = 0;
    GpioCtrlRegs.GPBDIR.bit.GPIO41 = 1;

    GpioCtrlRegs.GPBGMUX1.bit.GPIO42 = 0;
    GpioCtrlRegs.GPBMUX1.bit.GPIO42 = 0;
    GpioCtrlRegs.GPBDIR.bit.GPIO42 = 1;

    GpioCtrlRegs.GPBGMUX1.bit.GPIO43 = 0;
    GpioCtrlRegs.GPBMUX1.bit.GPIO43 = 0;
    GpioCtrlRegs.GPBDIR.bit.GPIO43 = 1;

    EDIS;

    EALLOW;
    GpioCtrlRegs.GPBGMUX1.bit.GPIO33 = 0;
    GpioCtrlRegs.GPBMUX1.bit.GPIO33 = 0;
    GpioCtrlRegs.GPBDIR.bit.GPIO33 = 0;


    GPIO_SetupXINT1Gpio(33);
    GpioCtrlRegs.GPBQSEL1.bit.GPIO33 = 2;
    GpioCtrlRegs.GPBCTRL.bit.QUALPRD2 = 255;
    XintRegs.XINT1CR.bit.POLARITY = 0; // Falling edge interrupt
    XintRegs.XINT1CR.bit.ENABLE = 1; // Enable XINT1
    EDIS;

    PieCtrlRegs.PIECTRL.bit.ENPIE = 1; // Enable the PIE block
    PieCtrlRegs.PIEIER1.bit.INTx4 = 1; // Enable PIE Group 1 INT4 (for XINT1)
    IER |= M_INT1;

    EINT;

    for (;;)
        {



//        counter_clockwise(450,fast); //POS 3
//        LED_red();
//        counter_clockwise(300,slow); // POS 5
//        LED_off();
//
//        clockwise(300,fast); // POS 3
//        LED_blue();
//        counter_clockwise(300,slow); //POS 5
//        LED_off();
//
//        clockwise(300,fast);// POS 3
//        LED_green();
//        counter_clockwise(300,slow);// POS 5
//        LED_off();
//
//        clockwise(300,fast); //POS 3
//        LED_red();
//        LED_blue();
//        LED_green();
//        counter_clockwise(300,slow);// POS 5
//        LED_off();
//        counter_clockwise(300,fast);// POS 7
//
//        stop(2000000);

        half_clockwise(1100,4000); //POS 0

        }




}

//######################   End of main code #######################


//###################### Start of functions codes #############

//######################    Functions codes for program1 #############

/*  void program1(void)
{
/// codes for program 1
}
*/


//######################    Functions codes for program2 #############

/* void program1(void)
{
/// codes for program 2
}
*/

void clockwise(Uint16 cycles, Uint16 delay){

    GpioDataRegs.GPASET.bit.GPIO31 = 1;
    Uint16 i=0;
    for( ; i<cycles; ++i){

        if (reset == 1)
            break;

        GpioDataRegs.GPBSET.bit.GPIO41 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO43 = 1;
        GpioDataRegs.GPBSET.bit.GPIO42 = 1;
        GpioDataRegs.GPACLEAR.bit.GPIO16 = 1;
        DELAY_US(delay);

        GpioDataRegs.GPBCLEAR.bit.GPIO41 = 1;
        GpioDataRegs.GPBSET.bit.GPIO43 = 1;
        GpioDataRegs.GPBSET.bit.GPIO42 = 1;
        GpioDataRegs.GPACLEAR.bit.GPIO16 = 1;
        DELAY_US(delay);

        GpioDataRegs.GPBCLEAR.bit.GPIO41 = 1;
        GpioDataRegs.GPBSET.bit.GPIO43 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO42 = 1;
        GpioDataRegs.GPASET.bit.GPIO16 = 1;
        DELAY_US(delay);

        GpioDataRegs.GPBSET.bit.GPIO41 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO43 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO42 = 1;
        GpioDataRegs.GPASET.bit.GPIO16 = 1;
        DELAY_US(delay);
    }
    GpioDataRegs.GPACLEAR.bit.GPIO31 = 1;

}

void half_clockwise(Uint16 cycles, Uint16 delay){

    GpioDataRegs.GPASET.bit.GPIO31 = 1;
    Uint16 i=0;
    for( ; i<cycles; ++i){

        if (reset == 1)
            break;

        //1
        GpioDataRegs.GPBSET.bit.GPIO41 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO43 = 1;
        GpioDataRegs.GPBSET.bit.GPIO42 = 1;
        GpioDataRegs.GPACLEAR.bit.GPIO16 = 1;
        DELAY_US(delay);

        //2
        GpioDataRegs.GPBCLEAR.bit.GPIO41 = 1;
        GpioDataRegs.GPBSET.bit.GPIO43 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO42 = 1;
        GpioDataRegs.GPACLEAR.bit.GPIO16 = 1;
        DELAY_US(delay);

        //3
        GpioDataRegs.GPBCLEAR.bit.GPIO41 = 1;
        GpioDataRegs.GPBSET.bit.GPIO43 = 1;
        GpioDataRegs.GPBSET.bit.GPIO42 = 1;
        GpioDataRegs.GPACLEAR.bit.GPIO16 = 1;
        DELAY_US(delay);

        //4
        GpioDataRegs.GPBCLEAR.bit.GPIO41 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO43 = 1;
        GpioDataRegs.GPBSET.bit.GPIO42 = 1;
        GpioDataRegs.GPACLEAR.bit.GPIO16 = 1;
        DELAY_US(delay);

        //5
        GpioDataRegs.GPBCLEAR.bit.GPIO41 = 1;
        GpioDataRegs.GPBSET.bit.GPIO43 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO42 = 1;
        GpioDataRegs.GPASET.bit.GPIO16 = 1;
        DELAY_US(delay);

        //6
        GpioDataRegs.GPBCLEAR.bit.GPIO41 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO43 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO42 = 1;
        GpioDataRegs.GPASET.bit.GPIO16 = 1;
        DELAY_US(delay);

        //7
        GpioDataRegs.GPBSET.bit.GPIO41 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO43 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO42 = 1;
        GpioDataRegs.GPASET.bit.GPIO16 = 1;
        DELAY_US(delay);

        //8
        GpioDataRegs.GPBSET.bit.GPIO41 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO43 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO42 = 1;
        GpioDataRegs.GPACLEAR.bit.GPIO16 = 1;
        DELAY_US(delay);
    }
    GpioDataRegs.GPACLEAR.bit.GPIO31 = 1;

}

void counter_clockwise(Uint16 cycles, Uint16 delay){

    GpioDataRegs.GPBSET.bit.GPIO34 = 1;
    Uint16 i=0;
    for( ; i<cycles; ++i){

        if (reset == 1)
            break;

        GpioDataRegs.GPBSET.bit.GPIO41 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO43 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO42 = 1;
        GpioDataRegs.GPASET.bit.GPIO16 = 1;
        DELAY_US(delay);

        GpioDataRegs.GPBCLEAR.bit.GPIO41 = 1;
        GpioDataRegs.GPBSET.bit.GPIO43 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO42 = 1;
        GpioDataRegs.GPASET.bit.GPIO16 = 1;
        DELAY_US(delay);

        GpioDataRegs.GPBCLEAR.bit.GPIO41 = 1;
        GpioDataRegs.GPBSET.bit.GPIO43 = 1;
        GpioDataRegs.GPBSET.bit.GPIO42 = 1;
        GpioDataRegs.GPACLEAR.bit.GPIO16 = 1;
        DELAY_US(delay);

        GpioDataRegs.GPBSET.bit.GPIO41 = 1;
        GpioDataRegs.GPBCLEAR.bit.GPIO43 = 1;
        GpioDataRegs.GPBSET.bit.GPIO42 = 1;
        GpioDataRegs.GPACLEAR.bit.GPIO16 = 1;
        DELAY_US(delay);

    }
    GpioDataRegs.GPBCLEAR.bit.GPIO34 = 1;

}

void stop(Uint16 delay){

    GpioDataRegs.GPBCLEAR.bit.GPIO41 = 1;
    GpioDataRegs.GPBCLEAR.bit.GPIO43 = 1;
    GpioDataRegs.GPBCLEAR.bit.GPIO42 = 1;
    GpioDataRegs.GPACLEAR.bit.GPIO16 = 1;
    DELAY_US(delay);

}

void LED_red(){
    GpioDataRegs.GPASET.bit.GPIO13 = 1;
}

void LED_blue(){
    GpioDataRegs.GPASET.bit.GPIO0 = 1;
}

void LED_green(){
    GpioDataRegs.GPASET.bit.GPIO2 = 1;
}

void LED_off(){
    GpioDataRegs.GPACLEAR.bit.GPIO13 = 1;
    GpioDataRegs.GPACLEAR.bit.GPIO0 = 1;
    GpioDataRegs.GPACLEAR.bit.GPIO2 = 1;
}


interrupt void xint1_isr(void)
{
    reset = 1;
    stop(4000000);
    // Acknowledge this interrupt to get more from group 1
    PieCtrlRegs.PIEACK.all = PIEACK_GROUP1;
}
