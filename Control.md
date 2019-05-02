# Abstract Control System Design

This is a description of the components of a control system framework
at an abstract level, not taking into consideration implementation
details.

## API Components

There are five components of the interface the control system
framework presents to subsystems: State, Configuration, Control, Data
Monitoring, and Exception Handling.

### State

Subsystems act abstractly as state machines, with five states (one
being implicit):

| **State** | **Meaning**                                           |
|-----------|-------------------------------------------------------|
| _OFFLINE_ | the system is not running (implicit)                  |
| STARTING  | the system has been started and is configuring itself |
| ONLINE    | the system is now running normally                    |
| STOPPING  | the subsystem has is preparing to shut down normally  |
| ABORTING  | the subsystem is preparing to shut down unexpectedly  |

Systems are implicitly _OFFLINE_ when they're not running. When a
subsystem is started, it transitions to the STARTING state. The system
now:

1. connects to the broker,
2. initializes itself with a [configuration](#configuration),
3. runs an _on\_start_ hook, and finally
4. transitions to the ONLINE state.

Once online, the system is communicating with the network, handling
[commands](#control) and publishing [data](#data-monitoring). It also
runs an _on\_online_ hook.

When the system recieves a command to stop, it transitions to the
STOPPING state. The system now:

1. runs an _on\_stop_ hook,
2. relinquishes all held resources, and
3. transitions to the STOPPED state.




The relationship between these states can be understood as follows,
with state transitions annotated with _hooks_ where appropriate:

![Abstract control flow diagram][diagram:control-flow]

### Configuration

Systems initialize themselves with a runtime configuration -- think
command-line arguments, or a .ini file. This is a series of key-value
pairs.

### Control

Systems expose a number of (possibly parameterized) command endpoints
to clients. Clients may call these commands remotely. This is RPC.

From the perspective of the client this RPC is inherently
asynchronous; however, the client caller receives a future which
resolves when the server returns from the call (possibly with a return
value), allowing for simulated synchronous RPC.

### Data Monitoring

Systems expose a number of data points which are then published for
clients to monitor. Data points should assert their type when possible.

When defining a data point, systems have two options for controlling
the rate that point is published:

1. The data point is polled at a constant rate defined by the system, or
2. The data point is published whenever its value changes.

### Exception Handling

Systems expose their possible internal exception states. For example,
a camera driver system would define an exception for "lost connection
to camera", and maybe another for "camera temperature is too
high". Additionally, there are a number of built-in exception states,
like "lost connection to broker" or "system aborted".

A _fatal_ exception is handled like any other exception, and
additionally [transitions the system](#state) to the ABORTING state to
initiate an unexpected system shutdown.

When a client connects to a system, it defines _handlers_ for server
system exceptions, which are run when the server system raises the
associated exception.


[diagram:control-flow]: abstract-control-flow.svg
