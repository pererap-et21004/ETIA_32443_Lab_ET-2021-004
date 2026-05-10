

#include <avr/io.h>
#include <util/delay.h>

int main(void)
{
    
    
    DDRD = 0b00000011; 
    
    
    DDRB = 0b00000000;
    DDRB |= (1 << PB0);

    
    while (1)
    {
       
        
        if (PIND & (1 << PD3)) 
        {
           
            PORTB |= (1 << PB0);
        }
        else 
        {
           
            PORTB &= ~(1 << PB0);
        }
    }

    return 0;
}