---
title: Module's API
---

## Overview
This page lists all possible API calls that subsystem A3 can handle. Subsystem A3 acts as the onboard side of the bluetooth relay connecting the handheld controller to the boat.
To facilitate communications, A3 uses defined message types following the team-wide UART protocol as well as GATT charecteristics defined by it's custom service. All of these message type are relayed to the necessary downstream board over UART or upstream via bluetooth with the exception of message type L (rollcall), which is a team-wide debugging API used to test the daisy-chain network by lighting each up board as it travels through the chain.

## UART Messages

**Message Type A - Set Steering Angle**

||**Byte 1** |**Byte 2**|**Byte 3**|**Byte 4**|
| :-------: | :-------: | :-------: | :-------: | :-------: |
| Variable Name | Sender_ID | Reciever_ID | Message_Type | Angle |
| Variable Type | char | char | char | uint8_t |
| Min Value | A | E | A | 0 |
| Max Value | A | E | A | 255|
| Example | A | E | A | 125 | 

**Message Type B - Set Throttle Percentage:**

||**Byte 1** |**Byte 2**|**Byte 3**|**Byte 4**|
| :-------: | :-------: | :-------: | :-------: | :-------: |
| Variable Name | Sender_ID | Reciever_ID | Message_Type | Throttle |
| Variable Type | char | char | char | uint8_t |
| Min Value | A | D | B | 0 |
| Max Value | A | D | B | 255 |
| Example | A | D | B | 125 | 

**Message Type C - Set Camera Angle:**

||**Byte 1** |**Byte 2**|**Byte 3**|**Byte 4**|**Byte 5**|
| :-------: | :-------: | :-------: | :-------: | :-------: | :-------: |
| Variable Name | Sender_ID | Reciever_ID | Message_Type | Yaw | Pitch |
| Variable Type | char | char | char | int8_t | int8_t |
| Min Value | A | G | C | -128 | -128 |
| Max Value | A | G | C | 127 | 127 |
| Example | A | G | C | 125 | 90 |

**Message Type D - Take Photo:**

||**Byte 1** |**Byte 2**|**Byte 3**|
| :-------: | :-------: | :-------: | :-------: |
| Variable Name | Sender_ID | Reciever_ID | Message_Type |
| Variable Type | char | char | char |
| Min Value | A | F | D |
| Max Value | A | F | D |
| Example | A | F | D |

**Message Type E - Send Speed Data:**

||**Byte 1** |**Byte 2**|**Byte 3**|**Byte 4**|
| :-------: | :-------: | :-------: | :-------: | :-------: |
| Variable Name | Sender_ID | Reciever_ID | Message_Type | Speed |
| Variable Type | char | char | char | int8_t |
| Min Value | H | A | E | -128 |
| Max Value | H | A | E | 127 |
| Example | H | A | E | -3 | 

**Message Type F - Send Distance Data:**

||**Byte 1** |**Byte 2**|**Byte 3**|**Byte 4**|**Byte 5**|**Byte 6**|
| :-------: | :-------: | :-------: | :-------: | :-------: | :-------: | :-------: |
| Variable Name | Sender_ID | Reciever_ID | Message_Type | Distance 1 | Distance 2 | Distance 3 |
| Variable Type | char | char | char | char | char | char |
| Min Value | J | A | F | '0' | '0' | '0' |
| Max Value | J | A | F | '9' | '9' | '9' |
| Example | J | A | F | '1' | '8' | '5' |

**Message Type G - Send Temperature Data:**

||**Byte 1** |**Byte 2**|**Byte 3**|**Byte 4**|
| :-------: | :-------: | :-------: | :-------: | :-------: |
| Variable Name | Sender_ID | Reciever_ID | Message_Type | Temperature |
| Variable Type | char | char | char | uint8_t |
| Min Value | I | A | G | 0 |
| Max Value | I | A | G | 255 |
| Example | I | A | G | 125 | 

**Message Type L - Rolecall:**

||**Byte 1** |**Byte 2**|**Byte 3**|
| :-------: | :-------: | :-------: | :-------: |
| Variable Name | Sender_ID | Reciever_ID | Message_Type |
| Variable Type | char | char | char |
| Min Value | A | X | J |
| Max Value | J | X | J |
| Example | A | X | J |

## Controller GATT service attributes

To accommodate wireless remote control, a custom BLE GATT service will be defined. In accordance with the BLE standard, custom services use a 16-byte UUID. The UUID for our controller service is 8bbd8ff7-3d84-4e81-9d46-70b6cb79e76a. The convention 'upstream' is used to refer to the wireless controller/human operator, and 'downstream' refers to the boat. Our service has the following characteristics: </br>

**Upstream Message (5699aead-41fc-4705-9c65-7c84d8bc04c):**</br>
This characteristic contains messages intended to be sent upstream (to the controller) from the boat. Any message of arbitrary length addressed to subsystems A1 or A2 will be written to this characteristic by A3, provided it is of a valid format. A2 will relay any messages addressed to A1 when notified of an update.

|**Read**|**Write**|**Notify**|**Capture**|
| :----: | :-----: | :------: | :-------: |
| True   | False   | True     | False     |

**Downstream Message (a037a1df-ccaf-480c-b7a4-6526a6848887):**</br>
This characteristic contains messages intended to be sent downstream (to the boat) from the controller. Any valid message will be written to this characteristic by subsystem A2 and relayed to the rest of the boat by A3 when notified of an update.

|**Read**|**Write**|**Notify**|**Capture**|
| :----: | :-----: | :------: | :-------: |
| True   | True    | True     | True      |

**Rollcall (d9cba376-e9c9-4db7-a6f2-4c6783c1dade):**</br>
To remove the requirement for parsing relayed messages from the previous two characteristics, a dedicated rollcall characteristic will be used. This can be written by either A2 or A3 to inform the other of a rollcall. Both systems will perform their rollcall functionality and proceed to relay the message upstream or downstream as needed, depending on the sender.

|**Read**|**Write**|**Notify**|**Capture**|
| :----: | :-----: | :------: | :-------: |
| True   | True    | True     | True      |