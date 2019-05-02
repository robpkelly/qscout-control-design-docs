# Notes: QSCOUT Control System Interface

This document is a collection of my notes on a potential design for an interface
to a distributed IIoT control system framework over MQTT and Sparkplug-B

## Infrastructure

This section explains the terminology I'm using throughout my notes.

### Control Server

A _control server_ in this framework is a device "driver" application. These are
the applications that will use the API to the control system framework. The
control server controls any number of underlying devices (actuators, lasers,
sensors, etc) and exposes a control interface to other applications on the MQTT
network.

In _Sparkplug-B_ jargon this corresponds to an _Edge-of-Network (EoN) Node,_ and
the controlled devices are simply _devices._

### Primary Server

The _primary server_ is a special case of a control server which controls and
supervises operation of every control server on the MQTT network.

In _Sparkplug-B_ jargon this is the _Primary Application._

### Historian Server

The _historian server_ is a server which writes all _scientific_ and
_engineering_ data to a time-series database for post-experiment diagnostics and
reporting. _Scientific_ data are collected observations, _engineering_ data are
device telemetry for diagnostics.

This server could likely be a component of the _primary server_ but need not be.

### Broker

The _broker_ is the MQTT host handling the connections of all servers on the
MQTT network. There's a wide selection of enterprise message brokers to choose
from. Eclipse Mosquitto seems to be a popular choice for MQTT.

_Sparkplug-B_ inexplicably refers to this as the _MQTT Server._

## Abstract Control System Design

See [this document](Control.md) for a very high-level overview of the concepts
and components of a control system framework.

## Interface Sketch

This is an informal outline of the control system API exposed to control servers
by our framework.

### State

**TODO**

### User (Developer) Story

**TODO**
