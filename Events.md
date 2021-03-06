# Documentation for the 6 Events Files

## Table of Contents
- [Summary](#summary)
- [Data Filtering](#data-filtering)
- [Data Size](#data-size)
- [BsmEventFlag](#bsmeventflag)
- [BrakeByte2Events](#brakebyte2events)
- [ExteriorLightsEvents](#exteriorlightsevents)
- [SteerAngleEvents](#steerangleevents)
- [ThrottlePositionEvents](#throttlepositionevents)
- [WiperStatusFrontEvents](#wiperstatusfrontevents)
- [Reference](#reference)

## Summary
This document describes the Events file filtered dataset that is available on Flux through Globus Connect. There are 6 event files available:
- BsmEventFlag
- BrakeByte2Events
- ExteriorLightsEvents
- SteerAngleEvents
- ThrottlePositionEvents
- WiperStatusFrontEvents

## Data Filtering
Each event was filtered according to the de-identified BSM transmitted dataset. For an event to be included, it had to meet one of three requirements:
1. The start time and end time had to both be within the de-identified time window for that trip.
2. The start time had to be within the de-identified time window for that trip. In this case, the end time was censored to match the ending time of the de-identified time window.
3. The end time had to be within the de-identified time window for that trip. In this case, the start time was censored to match the starting time of the de-identified time window.

## Data Size
The total size of the dataset is about 8.0 GB (uncompressed).

## BsmEventFlag
The `BsmEventFlag.csv` file contains *flagged events*, which are defined as the occurrence of unusual events. The events logged in this file are communicated through the vehicle’s CAN bus, from the vehicle component that initiated the event, to the onboard WSU; which then transmits the message. Once the host vehicle generates and transmits an unusual event, the receiving vehicle willl process this information and produce actions(s) that corresponds to the unusual event. These unusual events range from a vehicle’s hazard lights being on to a vehicle’s airbag being deployed.

### Variables
Column Number|Name|Description|Units
:---:|---|---|---
1|RxDevice|receiving Device Id (For VADs this is the same as TxDevice); equivalent to VehicleID|none  
2|FileId|unique number assigned to each pcap file|none  
3|TxDevice|sending Device Id (static 2 bytes of the BSM 4 byte temporary Id)|none  
4|Gentime|number of microseconds since Jan 1, 2004 (UTC +00:00)|microseconds  
5|Value|event code|none

### BsmEventFlag Value
The entry in the `Value` field defines one of the 13 events for BsmEventFlag. There can be more than one event represented.

Decimal Value|Name|Description
:---:|---|---
0|eventHazardLights|Hazard lights activated
1|eventStopLineViolation|A vehicle anticipates passing the stop line at an intersection without coming to a full stop before reaching it
2|eventABSactivated|Anti-locking braking system activated for more than 100ms in duration and active
4|eventTractionControlLoss|Traction control system activated for more than 100ms in duration and active
8|eventStabilityControlactivated|Stability control system activated for more than 100ms in duration and active
16|eventHazardousMaterials|The vehicle is known to be carrying hazardous material and is placarded as such
32|eventEmergencyResponse|An authorized public safety vehicle is engaged in a service call and is currently moving (lights and sirens may not be evident)
64|eventHardBraking|The vehicle has decelerated or is decelerating at a rate of greater than 0.4g
128|eventLightsChanged|The external lighting (headlights, park lights) of the vehicle has changed recently
256*|eventWipersChanged|Status of the front or rear wipers of the vehicle has changed recently
512*|eventFlatTire|The vehicle has determined that at least one tire has run flat
1024*|eventDisabledVehicle|Vehicle declaring itself as a disabled vehicle
4096*|eventAirBagDeployment|At least one airbag has been deployed

\*Value does not exist in the dataset

## BrakeByte2Events
The `BrakeByte2Events.csv` file communicates the state of some of the component of the vehicle’s brake system. The brake system components described include:
1. the state of the antilock brakes
2. the state of the stability control system
3. the application of the brake boost
4. the state of the auxiliary brake system.

### Variables
Column Number|Name|Description|Units
:---:|---|---|---
1|RxDevice|receiving Device Id (For VADs this is the same as TxDevice); equivalent to VehicleID|none  
2|FileId|unique number assigned to each pcap file|none  
3|TxDevice|sending Device Id (static 2 bytes of the BSM 4 byte temporary Id)|none  
4|StartTme|number of microseconds since Jan 1, 2004 (UTC +00:00)|microseconds  
5|EndTime|number of microseconds since Jan 1, 2004 (UTC +00:00)|microseconds 
6|Value|vehicle's brake system state|none  

### BrakeByte2Events Value
The entry in the `Value` field is based on the conversion of that entry from an 8-bit string. This 8-bit string communicates the state of four brake-related components of a vehicle; each using two bits to present the status of the each component.

1. The first two bits (**00**000000) pertain to the state of the vehicle’s antilock brake system (ABS).
2. The next two bits (00**00**0000) pertain to state of the vehicle’s stability control unit (SCU).
3. Bits five and six (0000**00**00) communicates whether BrakeBoost (BB) in being applied. BrakeBoost is a part of a system that detects the potential of a situation in which maximum brake power will be required and pre-charges the brake system even before the driver depresses the brake pedal. The application of the BrakeBoost indicates a situation that warrants emergency braking. Note, not all vehicles are equipped with BrakeBoost capability.
4. The last two bits (000000**00**) present the state of the auxiliary brake system. The auxiliary brake system is often called the parking brake.

Decimal Value|8-bit string|Condition|Detailed Description
:---:|---|---|---
0|**00**000000|ABS is unavailable|a vehicle is not equipped with ABS/ABS is unavailable
64|**01**000000|ABS is off|ABS is available but in the off position 
128|**10**000000|ABS is on|ABS is on but not engaged
192|**11**000000|ABS is engaged|ABS is on and engaged
0|00**00**0000|SCU is unavailable|a vehicle is not equipped with a SCU/SCU is unavailable
16|00**01**0000|SCU is off|ABS is in the off position
32|00**10**0000|SCU is on|ABS is on or engaged
0|0000**00**00|BB is unavailable|a vehicle is not equipped with a BB/BB is unavailable
4|0000**01**00|BB is off|BB is in the off position 
8|0000**10**00|BB is on|BB is on/is being applied 
0|000000**00**|AuxB is unavailable|vehicle is not equipped with AuxB/AuxB is unavailable
1|000000**01**|AuxB is off|AuxB is in the off position
2|000000**10**|AuxB is on|AuxB is on/active
3|000000**11**|BB is reserved|

## ExteriorLightsEvents
The `ExteriorLightEvents.csv` file communicates the state of all the vehicle’s exterior lights.

### Variables
Column Number|Name|Description|Units
:---:|---|---|---
1|RxDevice|receiving Device Id (For VADs this is the same as TxDevice); equivalent to VehicleID|none  
2|FileId|unique number assigned to each pcap file|none  
3|TxDevice|sending Device Id (static 2 bytes of the BSM 4 byte temporary Id)|none  
4|StartTme|number of microseconds since Jan 1, 2004 (UTC +00:00)|microseconds  
5|EndTime|number of microseconds since Jan 1, 2004 (UTC +00:00)|microseconds 
6|Value|state of all the vehicle's exterior lights|none  

### ExteriorLightsEvents Value
The entry in the `Value` field is based on the conversion of that entry from an 8-bit string.

Decimal Value|8-bit string|Condition|Detailed Description
:---:|---|---|---
0|00000000|allLightsOff|All exterior lights are off
1|00000001|lowBeamHeadLightsOn|Low beam headlights are on
2|00000010|highBeamHeadLightsOn|High beam headlights are on
4|00000100|leftTurnSiganlOn|Left turn signal is on
8|00001000|rightTurnSignalOn|Right turn signal is on
12|00001100|hazardSignalOn|Hazard signal is on
16|00010000|automaticLightControlOn|Lights are on due to automatic light control
32|00100000|daytimeRunningLightsOn|Day time running lights are on
64|01000000|fogLightOn|Fog lights are on
128*|10000000|parkingLightsOn|Parking lights are on

\*Value does not exist in the dataset

## SteerAngleEvents
The `SteerAngleEvents.csv` file communicates the angle of the steering wheel.

### Variables
Column Number|Name|Description|Units
:---:|---|---|---
1|RxDevice|receiving Device Id (For VADs this is the same as TxDevice); equivalent to VehicleID|none  
2|FileId|unique number assigned to each pcap file|none  
3|TxDevice|sending Device Id (static 2 bytes of the BSM 4 byte temporary Id)|none  
4|StartTme|number of microseconds since Jan 1, 2004 (UTC +00:00)|microseconds  
5|EndTime|number of microseconds since Jan 1, 2004 (UTC +00:00)|microseconds 
6|Value|8-bit representation of steering wheel angle|none

### SteerAngleEvents Value
The entry in the `Value` field contains an 8-bit unsigned integer that can be converted to steering angle degree. The least significant bit (LSB) units = 1.5 degrees and entries in this field have a range [0,255].

#### Converting to Steering Angle (Degrees)
- For values between 0 and 126, simply multiply by 1.5 degrees. 
- For values between 129 and 255, mask off the highest bit (which is being used as a sign bit) by doing a bitwise AND with a value of 127. Then swap the remaining bit values by doing a bitwise XOR with a value of 127 and then multiply by -1.5.

**Note**: Positive/Negative signed integer means right/left respectively. 

## ThrottlePositionEvents
The `ThrottlePositionEvents.csv` file presents the relative position of the throttle. Throttle position is measured in percent, communicating the displacement of the throttle from its default position to it maximum displacement. 

### Variables
Column Number|Name|Description|Units
:---:|---|---|---
1|RxDevice|receiving Device Id (For VADs this is the same as TxDevice); equivalent to VehicleID|none  
2|FileId|unique number assigned to each pcap file|none  
3|TxDevice|sending Device Id (static 2 bytes of the BSM 4 byte temporary Id)|none  
4|StartTme|number of microseconds since Jan 1, 2004 (UTC +00:00)|microseconds  
5|EndTime|number of microseconds since Jan 1, 2004 (UTC +00:00)|microseconds 
6|Value|relative position of the throttle|percent

## WiperStatusFrontEvents
The `WiperStatusFrontEvents.csv` file is intended to communicate whether it is raining or snowing at the vehicle’s current location and how hard the precipitation is. If the wipers are in the *On* position, it serves as a proxy for whether or not it is raining or snowing. The wipers’ *swipes per minute* serves as a proxy for how hard it is raining or snowing.

### Variables
Column Number|Name|Description|Units
:---:|---|---|---
1|RxDevice|receiving Device Id (For VADs this is the same as TxDevice); equivalent to VehicleID|none  
2|FileId|unique number assigned to each pcap file|none  
3|TxDevice|sending Device Id (static 2 bytes of the BSM 4 byte temporary Id)|none  
4|StartTme|number of microseconds since Jan 1, 2004 (UTC +00:00)|microseconds  
5|EndTime|number of microseconds since Jan 1, 2004 (UTC +00:00)|microseconds 
6|Value|front wiper status code|none

### WiperStatusFrontEvents Value
Value|Condition|Detailed Description
:---:|---|---
0|Unavailable|The status of the vehicle wiper is unavailable or the vehicle is not equipped with the wiper sensor status
1|Off|Front wipers are not activated
2|Intermittent|Front wipers are operated at an intermittent frequency
3|Low|Front wipers are operated at a low frequency
4|High|Front wipers are operated at a high frequency
126*|Washer in Use|Wipers are active due to the use of the washer fluid
127*|Automatic Present|The wipers have the ability to be automatically turned on

\*Value does not exist in the dataset

## Reference
The documentation was taken from this [Data.Transportation.gov Word document](https://data.transportation.gov/api/views/a7qq-9vfe/files/2bf7d0d1-5bdf-4342-82b2-0c84a58149ff)
