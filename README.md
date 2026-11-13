# ee186_lab4

## ADC

The ADC can be set to 12, 10, 8, or 6 bits. This is chosen with the resolution
setting ADC_RESOLUTION_XB, where X is 12, 10, 8, or 6.

The maximum sampling rate at these resolutions differs between fast and slow channel:
12-bit: 5.33 (fast), 4.21 (slow)
10-bit: 6.15 (fast), 4.71 (slow)
8-bit: 7.27 (fast), 5.33 (slow)
6-bit: 8.88 (fast), 6.15 (slow)

I found this on the datasheet in Section 6.3.21 "Analog-to-digital converter characteristics"


We need a voltage divider circuit because the LDR's resistance changes with light, 
but we are trying to measure voltage for the ADC input. Placing the LDR in series
with the 10kohm resistor between 3.3V and Gnd forms a votlage divider that converts
the change in resistance to a change in voltage V_out:

V_out = 3.3V (10kohm / (R_LDR + 10kohm)), where R_LDR is the resistance of the LDR. 
R_LDR decreases when exposed to more light, which produces an increase in V_out.
We can assume the 10kohm resistor has a resistance that is close to the R_LDR, because
it is also the right resistance to keep V_out between 0 and 3.3V

If the sensor were connected directly to the board, the reading would read 3.3V
regardless of changes in light. This is because the ADC input has a high internal
resistance so there would be no significant voltage drop across LDR. 
Alternatively, if we connect the LDR between the pin and Gnd, the pin would float
because we lack the resistor to pull down to ground.

LUX Measurements

| LUX | Header 2 | Header 3 |
| 100 | 2960 | 2383 |
| 200 | 2300 | 2580 | 
| 300 | 3375 | 2717 | 
| 400 | 3480 | 2815 | 
| 500 | 3600 | 2890 | 
| 600 | 3620 | 2900 | 
| 700 | 3650 | 2930 | 
| 800 | 3680 | 2970 | 
| 900 | 3720 | 2970 | 
| 1000 | 3720 | 3000 | 

## DAC

If B = 9 and we use 12-bit output DAC mode that provides the best resolution:  
V_out = (B/2^n) * V_ref = (9/4096) * V_ref = .0022 * V_ref
If V_ref = 3.3V, V_out = 7.25 mV


As we increase the sampling rate of the DAC, the steps up/down as we build the
waveform are smaller and the quality improves. We know that we need at least 2x
the tone frequency by the Nyquist frequency, or else the tone would alias.

Sine waves produce a single frequencey with no harmonics, sounding smoothest.
Square waves produce odd harmonics (3f, 5f, ect) at 1/n amplitude, and sound more hollow. 
Sawtooth waves produce all harmonics (2f, 3f, ect) at 1/n amplitude, and sounds the harshest. 
However, these all share the same pitch because of their shared fundamental frequency.

We need a capacitor to block the DC voltage and only pass AC voltage. The capacitor
acts as a high-pass filter so that only the audio signal reaches the speaker.


