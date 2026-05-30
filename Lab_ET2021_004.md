#include <avr/io.h>
#include <util/delay.h>

// Global definitions
const int seqLength = 4;
int userInput[4];
int password[4] = {0, 1, 2, 3}; // Map buttons to 0, 1, 2, 3
int count = 0;

// Pin Definitions
#define BUTTON_MASK ((1 << PD2) | (1 << PD3) | (1 << PD4) | (1 << PD5))
#define GREEN_LED   (1 << PB0) // Pin 8
#define RED_LED     (1 << PB1) // Pin 9

void setup() {
    // 1. Configure PD2-PD5 as Inputs
    DDRD &= ~BUTTON_MASK;
    // 2. Enable Internal Pull-ups for Inputs
    PORTD |= BUTTON_MASK;
    
    // 3. Configure PB0, PB1 as Outputs
    DDRB |= (GREEN_LED | RED_LED);
}

void checkPassword() {
    bool correct = true;
    for (int i = 0; i < seqLength; i++) {
        if (userInput[i] != password[i]) correct = false;
    }

    if (correct) {
        PORTB |= GREEN_LED;  // Access Granted (Pin 8)
        _delay_ms(2000);
        PORTB &= ~GREEN_LED;
    } else {
        PORTB |= RED_LED;    // Access Denied (Pin 9)
        _delay_ms(2000);
        PORTB &= ~RED_LED;
    }
    count = 0; 
}

int main() {
    setup();
    while (1) {
        // Read PD2-PD5 (shift right to normalize to 0-3 for logic)
        for (int i = 0; i < 4; i++) {
            if (!(PIND & (1 << (i + 2)))) { 
                userInput[count++] = i;
                _delay_ms(300); // Debounce
            }
        }

        if (count == seqLength) {
            checkPassword();
        }
    }
    return 0;
}
