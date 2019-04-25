# Abstract Control System Design

This is a description of the components of a control system framework
at an abstract level, not taking into consideration implementation
details.

## API Components

There are five components of the interface the control system
framework presents to subsystems: State, Configuration, Control, Data
Monitoring, and Exception Handling.

### State

Subsystems act abstractly as state machines, with seven states (one
being implicit):

| **State** | **Meaning**                                           |
|-----------|-------------------------------------------------------|
| _OFFLINE_ | the system is not running (implicit)                  |
| STARTING  | the system has been started and is configuring itself |
| ONLINE    | the system is now running normally                    |
| STOPPING  | the subsystem has is preparing to shut down normally  |
| STOPPED   | the subsystem has shut down normally                  |
| ABORTING  | the subsystem is preparing to shut down unexpectedly  |
| ABORTED   | the subsystem has shut down unexpectedly              |

The relationship between these states can be understood as follows:

![Abstract control flow diagram][diagram:control-flow]

### Configuration

### Control

### Data Monitoring

### Exception Handling


[diagram:control-flow]: abstract-control-flow.svg
