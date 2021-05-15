# Eagle to OpenSCAD part position export

> This ULP exports coordinates of selected parts in OpenSCAD array format.

## Installation
Just copy the ulp into your Eagle ulp folder. Or store it anywhere else where you can find it in step 1 of Usage below.

## Compatibility
As of now, it works with Eagle 7.7.0 and OpenSCAD 2021.01. But it is most likely not that strictly limited. Incompatibility reports will be appreciated.

## Usage
Use as you wish, but the typical scenario is:
1. Open the board of your choice in Eagle board editor and run this ULP script.
1. Fill in the dialog:

   **RegExp Pattern** allows for selecting only some parts by their name:
     - It works as usual regular expression and is as mighty as one can get. However, typically you will need to do way simpler things like writing a substring which is contained in the name of all parts you need to export. 
     - Some hints for regexp-impaired:
       - Leave empty to export all parts.
       - Put ^ to start of the RegExp to force the pattern to start at the start, i.e. 'C' will match C123 as well as XC123, whereas ^C will not match XC123.
       - Use square brackets \[ and \] to define several possible characters, e.g. [ACH] will match any of these three letters.
       - Use . to match any single character
       - Use ?, *, and + to quantify the preceding character: \? zero or once \* zero or more times \+ one or more times
       - Last but not least, as per Eagle ULP programming guide: *Details on regular expressions can be found, for instance, in the book Mastering Regular Expressions by Jeffrey E. F. Friedl.*
       
   **Export parts from layer** defines, whether parts from top or bottom layer are to be exported. More precisely, "Top" are parts which are not mirrored, "Bottom" are mirrored parts. This is not exactly correct, but works for most cases.
     
   **Export parts of type** defines part type to export:
     - "THT" Through hole technology, i.e. "legged" parts which need a hole to be soldered,
     - "SMD" Surface mount devices, i.e. "legless" parts which are soldered flat on the board.
     - Whether a part is TTH or SMD is decided based on its contacts type - does it have a smd contact? It is SMD. Does it have a non-smd contact? It is THT. Should a part have smd as well as non-smd contacts, it would be both THT and SMD. Even I have not met such part yet, it might be possible somewhere in a parallel universe.
     
   **OpenSCAD variable name** defines the variable name; In the export, an array will be created and this is its name.
     
   **Include more info** may provide more info in the output to make further OpenSCAD programming easier:
     - Part name, value, package as per what it says,
     - As comments - that info will be put as a comment at the end of line for each array member,
     - Inside the array - that info will be part of the array, i.e. each array member will contain the coordinates plus whatever info you select.
     
   **Units** lets you select what will be the coordinates units. Probably milimetres.

       
1. Press OK.
1. In the next dialog, select filename to export to and press OK.
1. Either #include the file in your OpenSCAD script, or just copy that file contents into your script.

## Dialog defaults
If you want to change the default settings of the dialog, feel free to open the ulp in text editor and change values of variables at the start of the "board(B) {" block in respect to the dlgDialog definition below.

## Output examples

Info inside the array:
```
partsPositions = [
// Date exported: 2021-05-15 12:23:10
// Board name: D:/test.brd
// Exporting: bottom, SMD
// Selection pattern: (empty)
// Units: milimeters
//[   x   ,   y   , rot, name, value, package ], 
  [  31.75,  23.50,  90, "C1", "10n", "C0805" ], 
  [   8.89,  23.50,  90, "C2", "100n", "C0805" ], 
  [  11.43,  23.50,  90, "C3", "100n", "C0805" ], 
  [  19.05,  23.50, 270, "C10", "100n", "C0805" ], 
  [  16.51,  23.50, 270, "C11", "100n", "C0805" ], 
  [  13.97,  23.50, 270, "C12", "100n", "C0805" ], 
  [  11.43,  58.74,   0, "D1", "ES2D", "SMB" ], 
  [  17.46,  61.28, 270, "FU1", "", "1812" ], 
  [  34.29,  23.50, 270, "R1", "4k7", "R0805" ], 
  [  39.37,  57.15, 180, "R2", "100k", "R0805" ], 
  [  11.11,  14.61,   0, "R6", "1k", "R0805" ], 
  [  11.11,  12.07,   0, "R7", "1k", "R0805" ], 
  [  11.11,   9.53,   0, "R8", "1k", "R0805" ], 
];
```

Info as comments:
```
partsPositions = [
// Date exported: 2021-05-15 12:24:46
// Board name: D:/test.brd
// Exporting: bottom, SMD
// Selection pattern: ^[RC]
// Units: milimeters
//[   x   ,   y   , rot ], // name | value | package | 
  [  31.75,  23.50,  90 ], // C1 | 10n | C0805 | 
  [   8.89,  23.50,  90 ], // C2 | 100n | C0805 | 
  [  11.43,  23.50,  90 ], // C3 | 100n | C0805 | 
  [  19.05,  23.50, 270 ], // C10 | 100n | C0805 | 
  [  16.51,  23.50, 270 ], // C11 | 100n | C0805 | 
  [  13.97,  23.50, 270 ], // C12 | 100n | C0805 | 
  [  34.29,  23.50, 270 ], // R1 | 4k7 | R0805 | 
  [  39.37,  57.15, 180 ], // R2 | 100k | R0805 | 
  [  11.11,  14.61,   0 ], // R6 | 1k | R0805 | 
  [  11.11,  12.07,   0 ], // R7 | 1k | R0805 | 
  [  11.11,   9.53,   0 ], // R8 | 1k | R0805 | 
];
```

```
partsPositions = [
// Date exported: 2021-05-15 12:27:00
// Board name: D:/test.brd
// Exporting: top, THT SMD
// Selection pattern: (empty)
// Units: milimeters
//[   x   ,   y   , rot ], // name | package | 
  [  31.12,  55.88, 270 ], // C4 | E2,5-5 | 
  [  36.20,  55.88, 270 ], // C5 | E2,5-5 | 
  [   6.67,  40.64,  90 ], // C17 | C0805 | 
  [  31.12,  60.96,   0 ], // PWR1 | AMSR-78-NZ | 
  [   6.03,  45.09, 180 ], // R3 | R0805 | 
  [   6.03,  47.62, 180 ], // R4 | R0805 | 
  [   6.03,  50.16, 180 ], // R5 | R0805 | 
  [  29.85,   3.81,   0 ], // SW1 | DT2112C | 
  [  11.43,  63.82,   0 ], // X1 | ARK550/2 | 
  [  29.21,  12.07,  90 ], // XC1 | MA03-1LOCK | 
  [  36.83,  12.07,  90 ], // XC2 | MA03-1LOCK | 
  [  13.97,   9.53,  90 ], // XC3 | MA05-1LOCK | 
  [  34.29,  45.09,  90 ], // XC9 | MA05-1LOCK | 
  [  26.67,  45.09,  90 ], // XC10 | MA05-1LOCK | 
  [  41.91,  43.82,  90 ], // XC12 | MA04-1LOCK | 
  [   8.89,  45.09,  90 ], // XC13 | MA05-1LOCK | 
  [  21.59,   8.26,  90 ], // XC14 | MA04-1LOCK | 
];
```

```
partsPositions = [
// Date exported: 2021-05-15 12:28:24
// Board name: D:/test.brd
// Exporting: top, THT SMD
// Selection pattern: C1
// Units: milimeters
//[   x   ,   y   , rot ], // name | 
  [   6.67,  40.64,  90 ], // C17 | 
  [  29.21,  12.07,  90 ], // XC1 | 
  [  26.67,  45.09,  90 ], // XC10 | 
  [  41.91,  43.82,  90 ], // XC12 | 
  [   8.89,  45.09,  90 ], // XC13 | 
  [  21.59,   8.26,  90 ], // XC14 | 
];
```

## Author
David Obdrzalek, david.obdrzalek@email.cz

## License
This project is licensed under the terms of the MIT license.