# Bot filesystem #

## FSM implementation ##

A Robot are Finite State Machine that consist of:
  * `Events`		that the bot responds to
  * `States`		where the bot waits between events
  * `Transitions`	between states in response to events
  * `Actions`		taken during transitions
  * `Variables`	that hold values needed between events

Two-dimensional tables is representations of FSM. Rows and columns represent _Events_ and _States_, and cells contain _Actions_ and _Transitions_.

_Event/State_ table of `/std/string`
| `Event V State ->` |  wait  | read | overflow |
|:-------------------|:-------|:-----|:---------|
|    `RESET` | init |  init   |   init     |
|    `INPUT` | State->read; | getchar |        |
|    `SEEK`  |    |   |  clear |

## Reflection to file system ##

On the [Botfs](Botfs.md) the **`States`** representation as directories, and the **`Events`** are named the **`Actions`**, which are stored as files in these directories. The **`Actions`** contain instructions for execution.
```
/std/..
    in  <- Standart input
    out <- Standart output
    err <- Standart log output
    wait <- Standart events input (queue)
    event <- Standart events output
    ...
    string/..
        deus/..   <- Special VM files
            ex    <- Instruction Pointer
            flags <- Status Flags 
            state <- Current State
            key   <- Secret Key
            pub   <- Public Key
            ...
State-> wait/..
            reset/  <- Event
            input/
        read/..
            reset/ <- Event
            input/..
		asm	<- Action as mnemocode
		exec	<- Action as bytecode
		kode	<- Action on the [Kode]
		x86	<- Action as native by architecture 
		x64
		ppc601
		sparcv7
		...
            overflow/
            ...
State-> overflow/..
            reset
            ...
```
The **`Action`** may generate new **`Event`**. [Deus](Deus.md) writes new events to the **`deus/queue`** of receivers. All the Bots, which must receive the **`Event`** are mounted to the service catalog `event/EventName`. They have the opportunity to be there or disappear from of implementation as required.

The **`Transitions`** here are not files, because it is does not object, but action. The **`Transition`** is a statement of [Kode](Kode.md).