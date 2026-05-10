

#include <avr/io.h>
#include <util/delay.h>

int main(void)
{
    
    DDRC |= (1 << DDC0) | (1 << DDC1) | (1 << DDC2) | (1 << DDC3);

    
    DDRC &= ~((1 << DDC4) | (1 << DDC5) | (1 << DDC6));

    while (1)
    {
       
        PORTC |= (1 << PC0);
        delay(1000);
        PORTC &= ~(1 << PC0);

       
        PORTC |= (1 << PC1);
        delay(1000);
        PORTC &= ~(1 << PC1);

        PORTC |= (1 << PC2);
        delay(1000);
        PORTC &= ~(1 << PC2);

     
        PORTC |= (1 << PC3);
        delay(1000);
        PORTC &= ~(1 << PC3);
    }

    return 0;
}