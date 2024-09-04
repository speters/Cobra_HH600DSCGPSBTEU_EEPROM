# Cobra HH600 marine handheld VHF radio data dump

The Cobra HH600 is a marine handheld VHF radio with DSC and ATIS.

## Opening the device

 * remove battery
 * remove 4 rubber plugs on the back of the device
 * unscrew antenna
 * unscrew cap for microphone jack
 * pull off volume potentiometer knob
 * pop out rubber gasket around volume potentiometer
 * unscrew nut around volume potentiometer
 * unscrew 6 Philips screws
 * remove back panel
 * pop off GPS antenna connector from GPS pcb
 * unsolder 2 * 2 wires at lower end of pcb
 * unsolder 2 wires at upper end of pcb
 * remove solder bridge between shielding of GPS and main PCB
 * pull GPS pcb
 * remove 2 screws holding main pcb (1 screw on the lower left and 1 screw previously hidden under the GPS pcb)
 * gently lift main pcb and pull down to remove
 * unclip flex cable to keypad
 * bend straight 2 upper holding tabs of tin LCD enclosure
 * desolder other 2 tabs of tin LCD enclosure
 * remove LCD enclosure, plastic diffuser, diffuser sheet
 
 Under the LCD you can now see the `MX25L1606E` serial flash memory.

For readout, the MX25L1606E has to be desoldered.

## Reading the serial flash

Using a TL866-II programmer and `minipro` program on Linux:

```
minipro -f ihex -p MX25L1606E@SOP8 -r FILE.ihex`
```

## Findings in the dump

The dump contains channel setup, as well as configuration data like `MMSI` and `ATIS code`.

It looks like the previously stored config is retained in the flash memory.

ATIS and MMSI are encoded as BCD numbers. Encoding scheme for the ATIS callsign is described on [ATIS @ Wikipedia](https://de.wikipedia.org/wiki/Automatic_Transmitter_Identification_System).

### MMSI and ATIS callsign

```patch
diff -uw t_099999999.ihex t_999999999.ihex
--- t_099999999.ihex    2024-09-04 15:27:15.556919340 +0200
+++ t_999999999.ihex    2024-09-04 14:40:07.539878097 +0200
@@ -544,7 +544,7 @@
 :1021E000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
 :1021F000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEF
 :102200005B172D43590C22384E5AFFFFFFFFFFFF8B
-:10221000FFFFFFFF5C0B0A3732096363635AFFFF5E
+:10221000FFFFFFFF5C0B0A3732636363635AFFFF04
 :10222000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFBE
 :10223000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFAE
 :10224000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF9E
@@ -3616,7 +3616,7 @@
 :10E1E000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF3F
 :10E1F000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF2F
 :10E200005B172D43590C22384E5AFFFFFFFFFFFFCB
-:10E21000FFFFFFFF5C0B0A3732096363635AFFFF9E
+:10E21000FFFFFFFF5C0B0A3732636363635AFFFF44
 :10E22000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFE
 :10E23000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEE
 :10E24000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFDE
```
