<div align="center">
<h1>Aixt</h1>
<h2>Programming Framework for Microcontrollers</h2>
</div>

Aixt is a programming framework for microcontrollers which uses a modern language syntax based on [_V_](https://vlang.io/) and able to be used by low-resource devices. Aixt is composed by 3 main components:

- the **Aixt** programing language based on the [_V language_](https://vlang.io/) syntax.
- the **Aixt to C Transpiler**, which transpile de **Aixt** source code to _C_, for an expecific _C_ compiler of each microcontroller.

and 

- the **Aixt API**, which makes the programming easy by standardizing the setup and I/O functions.

working together as follows, to obtain the binary file:

```mermaid
stateDiagram-v2

    Aixt: Aixt language
    state Aixt {
        source: Source code
        API
    } 

    Aixt2C: Aixt to C Transpiler
    state Aixt2C {
        Transpiler: Transpiler (V)
        setup: setup file (TOML)
    }

    C: C language
    state C {
        Tr_Code: Transpiled code
        API_C: API in C
    }

    state Microcontroller {
        PICs: PIC
        ATM: AT
        ESP
        RP2040
        others2: ...
        NXT: NXT brick
    }

    C_Compiler: C Compiler
    state C_Compiler {
        others: ...
        XC8  
        XC16 
        ImageCraft
        GCC  
        others 
        nbc: nbc (NXC) 
    }
    
    machine
    state machine {
        BF: Binary file
    }
    
    Aixt    --> Aixt2C 
    Aixt2C  --> Tr_Code
    C       --> C_Compiler
    C_Compiler  --> machine
    Microcontroller --> API_C
```

Aixt is designed as modular as possible, to make it easy to append new devices and boards.


## Aixt to C Transpiler

The transpiler is written in [_V_](https://vlang.io/) using the _V's_ native self-compiler (a transpiler from _V_ to _C_). This is implemented in the folders `\aixt_build`, `\aixt_cgen` and in the main source code `aixt.v`. It generates code for 3 different backends:
- **c**: for the microcontroller native C compiler
- **nxc**: for the NXC compiler (LEGO Mindstorms NXT)
- **arduino**: for the Arduino IDE

## Aixt Language

**Aixt** programing language implements a subset of [_V language_](https://vlang.io/). The main differences are show as follows:

feature                 |V                                  | Aixt
------------------------|-----------------------------------|----------------------------------------
variables               |immutable by default               | mutable by default
strings                 |dynamic-sized                      | fixed-sized
arrays                  |dynamic-sized                      | fixed-sized
default integers size   |32 bits                            | depends on the device  
structs                 |allow functions (object-oriented)  | don't allow functions (only structured)

## Aixt Examples
Just like _V_, the `main` function is optional in Aixt if the source code is all in only one file.

### Example with `main` function
```v
/* Turning ON the onboard LED 10 for the Explorer16 board 
for the PIC24FJ microcontroller (XC16 compiler)*/

fn main() { 
    pin_high(led10)    //turn ON the LED 10 (PORTA7)
}
```

### Example without `main` function (Script mode)
```v
/* Turning ON the onboard LED 10 for the Explorer16 board 
for the PIC24FJ microcontroller (XC16 compiler)*/

for {   //infinite loop
    pin_high(led10)     // LED 10 blinking 
    sleep_ms(500)
    pin_low(led10)
    sleep_ms(500)
}
```

## Aixt API

The **Aixt API** is inspired by _Micropython_, _Arduino_ and _Tinygo_ projects. The API for all the ports includes at least functions for:
- Digital input/output
- Analog inputs (ADC)
- PWM outputs
- Serial port (UART)

## Installing Aixt from source
clone the Aixt repository:
```
git clone https://github.com/fermarsan/aixt
```

compile `aixt.v`: 
```
cd aixt
v aixt.v
```

## Using Aixt
run it in a Linux-based system as:
```
./aixt <command> <device_or_board> <source_file>
```
or in Windows:
```
aixt.exe <command> <device_or_board> <source_file>
```

### Running examples:
```
./aixt -t Emulator test.v
```
```
./aixt -b NXT ports/NXT/projects/1_motor_forward.v
```

## Editor/IDE

Aixt is designed for mainly working with VS Code using the [VS Code extension for V language](https://marketplace.visualstudio.com/items?itemName=VOSCA.vscode-v-analyzer). Aixt brings a project template including a VS Code task file (`tasks.json`) including the commands: transpile, compile,run, build, clean, and help. 

Anyways, Aixt can be used with any editor/IDE that supports V language, such as Vim, Emacs, Sublime Text, Atom, etc, but running Aixt commands from the terminal.

## Project's name
The project's name is inspired in _Veasel_, the Weasel pet of _V Language_, and at the same time is a tribute to _Ticuna_ people who live in the Amazonic forest in the borders of _Colombia_, _Brasil_ and _Perú_. Weasels are _mustelids_ just like otters, so the name **Aixt** comes from _Aixtü_, which is a way to say otter in [_Ticuna_](https://www.sil.org/system/files/reapdata/90/20/51/90205190508691852389084667097660892450/tca_Ticuna_Dictionary_2016_web.pdf) language.


## Acknowledgement
Aixt thanks Alexander Medvednikov and all the contributors of [The V Programming Language](https://github.com/vlang) for their original work. Aixt is not only inspired by the _V_ syntax, but also it is written in _V_. Aixt also thanks [ARMOS research group](https://armos-ud.gitlab.io/armos/index.html) for their support to this project.

## License
Aixt is licensed under the [MIT License](https://github.com/fermarsan/aixt/LICENSE).
