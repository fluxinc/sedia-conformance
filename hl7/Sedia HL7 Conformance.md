# Sedia :productName

HL7 Conformance Statement
Version 1.0.0

2020-12-07

:mailingAddress

## Conformance Statement Overview

Sedia :productName is a :productDescription

Sedia :productName receives HL7 v2.x ORM messages, and sends ORU messages with the aid of an HL7 broker based on the NHAPI .NET library.

## Introduction

This HL7 Conformance Statement specifies the behaviour and functionality of the :productName system, with regard to supported HL7 Messages and Message Segments. : productName is an :productDescription.

Contact Address
:mailingAddress

## Revision History

| Document Version | Date of Issue | Author         | Description     |
| ---------------- | ------------- | -------------- | --------------- |
| 1.0.0            | 2020-12-07    | Wojtek Grabski | Initial version |

## Audience

This document is written for the people that need to understand how :productName will integrate into their healthcare facility. This includes both those responsible for overall workflow network policy and architecture, as well as integrators who need to have a detailed understanding of the HL7 features of the product. Integrators are expected to fully understand all the HL7 terminology, how the tables in this document relate to the product’s functionality, and how that functionality integrates with other devices that support compatible HL7 features.

## Remarks

The scope of this HL7 Conformance Statement is to facilitate integration between :productName and other HL7 products. The Conformance Statement should be read and understood in conjunction with the HL7 v2.x Standard. HL7 by itself does not guarantee interoperability. The Conformance Statement does, however, facilitate a first-level comparison for interoperability between different applications supporting compatible HL7 functionality.

This Conformance Statement is not supposed to replace validation with other HL7 equipment to ensure proper exchange of intended information. In fact, the user should be aware of the following important issues:

- The comparison of different Conformance Statements is just the first step towards assessing interconnectivity and interoperability between the product and other HL7 conformant equipment.
- Test procedures should be defined and executed to validate the required level of interoperability with specific compatible HL7 equipment, as established by the healthcare facility.

## Use Case Model

:productName can receive ORM (Order Message) from a HIS, RIS, or general HL7 Engine indicating that a patient was scheduled for an MR or CT Scan and the order is complete. These messages should be received prior to actual image or measurement data being transferred from cvi42 or other analysis software.

When a report is changed to a Finalized or Addendum state, an ORU (Observation Message) is returned to an upstream HIS, RIS, or HL7 Engine, presumably but not necessarily the same system that originated the transaction with an ORM.

## Dynamic Interaction Model

ORM messages originate with a HIS, RIS or HL7 Engine upstream from the :productName server. Received ORM messages are stored in :productName’s database and matched later to incoming DICOM-sourced data.

ORU messages are later produced when a physician creates a Finalized or Addendum report and are returned to an HL7 node, presumably the one that created the ORM message. Typically report data will be included either by embedding text or encoded data, or by including reference pointers to report data at a local or network location.

## Dynamic Definitions

### ORM (Event O01)

MSH PID PV1 [ PV2 ] ORC [ OBR ] [ { OBX } ]

### ORU (Event R01)

MSH PID PV1 [ PV2 ] ORC OBR [ { OBX } ]

## Static Definitions - Message Level

### ORM^O01

| Segment     | ORM Message                | Usage | Cardinality | Comment |
| ----------- | -------------------------- | ----- | ----------- | ------- |
| MSH         | Message Header             | R     | [1..1]      |         |
| PID         | Patient Identification     | R     | [1..1]      |         |
| PV1         | Patient Visit              | RE    | [1..1]      |         |
| [PV2]       | Patient Visit - Additional | O     | [0..1]      |         |
| ORC         | Common Order               | R     | [1..1]      |         |
| [OBR]       | Observation                | O     | [0..1]      |         |
| [ { OBX } ] | Observation / Result       | O     | [0..*]      |         |

### ORU^R01

| Segment     | ORM Message                | Usage | Cardinality | Comment                     |
| ----------- | -------------------------- | ----- | ----------- | --------------------------- |
| MSH         | Message Header             | R     | [1..1]      |                             |
| PID         | Patient Identification     | R     | [1..1]      |                             |
| PV1         | Patient Visit              | RE    | [1..1]      |                             |
| [ PV2 ]     | Patient Visit - Additional | O     | [0..1]      | Returned if present in ORM. |
| ORC         | Common Order               | R     | [1..1]      |                             |
| OBR         | Observation                | O     | [0..1]      | Single OBR returned.        |
| [ { OBX } ] | Observation / Result       | O     | [0..*]      |                             |

## Static Definitions - Segment Level

### MSH

| Sequence | Name                                    | Usage | Cardinality | Comment                               |
| -------- | --------------------------------------- | ----- | ----------- | ------------------------------------- |
| 1        | Field Separator                         | R     | [1..1]      |                                       |
| 2        | Encoding Characters                     | R     | [1..1]      |                                       |
| 3        | Sending Application                     | R     | [1..1]      | Sender in ORM becomes Receiver in ORU |
| 4        | Sending Facility                        | R     | [1..1]      | Sender in ORM becomes Receiver in ORU |
| 5        | Receiving Application                   | R     | [0..1]      | Receiver in ORM becomes Sender in ORU |
| 6        | Receiving Facility                      | R     | [0..1]      | Receiver in ORM becomes Sender in ORU |
|          | Date / Time of Message                  | O     | [0..1]      |                                       |
|          | Security                                | O     | [0..1]      |                                       |
|          | Message Type                            | R     | [1..1]      |                                       |
| 10       | Message Control ID                      | R     | [1..1]      |                                       |
| 11       | Processing ID                           | R     | [1..1]      |                                       |
| 12       | Version ID                              | R     | [1..1]      |                                       |
| 13       | Sequence Number                         | O     | [0..1]      |                                       |
| 14       | Continuation Pointer                    | O     | [0..1]      |                                       |
| 15       | Accept Acknowledgement Type             | O     | [0..1]      |                                       |
| 16       | Application Acknowledgement Type        | O     | [0..1]      |                                       |
| 17       | Country Code                            | O     | [0..1]      |                                       |
| 18       | Character Set                           | O     | [0..*]      |                                       |
| 19       | Principal Language of Message           | O     | [0..1]      |                                       |
| 20       | Alternate Character Set Handling Scheme | O     | [0..1]      |                                       |

### PID

| Sequence | Name                              | Usage | Cardinality | Comment |
| -------- | --------------------------------- | ----- | ----------- | ------- |
| 1        | Set ID - PID                      | O     | [0..1]      |         |
| 2        | Patient ID                        | B     | [0..1]      |         |
| 3        | Patient Identifier List           | R     | [1..*]      |         |
| 4        | Alternate Patient ID - PID        | B     | [1..*]      |         |
| 5        | Patient’s Name                    | R     | [1..*]      |         |
| 6        | Mother’s Maiden Name              | O     | [0..*]      |         |
| 7        | Date/Time of Birth                | O     | [0..1]      |         |
| 8        | Sex                               | O     | [0..1]      |         |
| 9        | Patient Alias                     | O     | [0..*]      |         |
| 10       | Race                              | O     | [0..*]      |         |
| 11       | Patient Address                   | O     | [0..*]      |         |
| 12       | Country Code                      | B     | [0..1]      |         |
| 13       | Phone Number - Home               | O     | [0..*]      |         |
| 14       | Phone Number - Work               | O     | [0..*]      |         |
| 15       | Primary Language                  | O     | [0..1]      |         |
| 16       | Marital Status                    | O     | [0..1]      |         |
| 17       | Religion                          | O     | [0..1]      |         |
| 18       | Patient Account Number            | O     | [0..1]      |         |
| 19       | Social Security Number - Patient  | B     | [0..1]      |         |
| 20       | Driver’s License Number - Patient | O     | [0..1]      |         |
| 21       | Mother’s Identifier               | O     | [0..*]      |         |
| 22       | Ethnic Group                      | O     | [0..*]      |         |
| 23       | Birth Place                       | O     | [0..1]      |         |
| 24       | Multiple Birth Indicator          | O     | [0..1]      |         |
| 25       | Birth Order                       | O     | [0..1]      |         |
| 26       | Citizenship                       | O     | [0..*]      |         |
| 27       | Veterans Military Status          | O     | [0..1]      |         |
| 28       | Nationality                       | O     | [0..1]      |         |
| 29       | Patient Death Date and Time       | O     | [0..1]      |         |
| 30       | Patient Death Indicator           | O     | [0..1]      |         |

### PV1

PV1 segments are ignored by :productName, however the segment will be stored and returned to the ORU as-is.

### PV2

PV2 segments are ignored by :productName, however the segment will be stored and returned to the ORU as-is.

### ORC

| Sequence | Name                             | Usage | Cardinality | Comment |
| -------- | -------------------------------- | ----- | ----------- | ------- |
| 1        | Order Control                    | R     | [1..1]      |         |
| 2        | Placer Order Number              | O     | [0..1]      |         |
| 3        | Filler Order Number              | O     | [0..1]      |         |
| 4        | Placer Group Number              | O     | [0..1]      |         |
| 5        | Order Status                     | O     | [0..1]      |         |
| 6        | Response Flag                    | O     | [0..1]      |         |
| 7        | Quantity/Timing                  | O     | [0..1]      |         |
| 8        | Parent                           | O     | [0..1]      |         |
| 9        | Date/Time of Transaction         | O     | [0..1]      |         |
| 10       | Entered By                       | O     | [0..*]      |         |
| 11       | Verified By                      | O     | [0..*]      |         |
| 12       | Ordering Provider                | O     | [0..*]      |         |
| 13       | Enterer’s Location               | O     | [0..1]      |         |
| 14       | Call Back Phone Number           | O     | [0..2]      |         |
| 15       | Order Effective Date/Time        | O     | [0..1]      |         |
| 16       | Order Control Code Reason        | O     | [0..1]      |         |
| 17       | Entering Organization            | O     | [0..1]      |         |
| 18       | Entering Device                  | O     | [0..1]      |         |
| 19       | Action By                        | O     | [0..*]      |         |
| 20       | Advanced Beneficiery Notice Code | O     | [0..1]      |         |
| 21       | Ordering Facility Name           | O     | [0..*]      |         |
| 22       | Ordering Facility Address        | O     | [0..*]      |         |
| 23       | Ordering Facility Phone NUmber   | O     | [0..*]      |         |
| 24       | Ordering Provider Address        | O     | [0..*]      |         |

### OBR

| Sequence | Name                                     | Usage | Cardinality | Comment                                                       |
| -------- | ---------------------------------------- | ----- | ----------- | ------------------------------------------------------------- |
| 1        | Set ID - OBR                             | O     | [0..1]      |                                                               |
| 2        | Placer Order Number                      | O     | [0..1]      |                                                               |
| 3        | Filler Order Number                      | O     | [0..1]      |                                                               |
| 4        | University Service ID                    | R     | [1..1]      |                                                               |
| 5        | Priority                                 | O     | [0..1]      |                                                               |
| 6        | Requested Date/Time                      | O     | [0..1]      |                                                               |
| 7        | Observation Date/Time                    | O     | [0..1]      |                                                               |
| 8        | Observation End Date/Time                | O     | [0..1]      |                                                               |
| 9        | Collection VOlume                        | O     | [0..1]      |                                                               |
| 10       | Collector Identifier                     | O     | [0..*]      |                                                               |
| 11       | Specimen Action COde                     | O     | [0..1]      |                                                               |
| 12       | Danger Code                              | O     | [0..1]      |                                                               |
| 13       | Relevant Clinical Info                   | O     | [0..1]      |                                                               |
|          | Specimen Received Date/Time              | O     | [0..1]      |                                                               |
| 14       | Speciment Source                         | O     | [0..1]      |                                                               |
| 15       |                                          |       | [0..1]      |                                                               |
| 16       | Ordering Provider                        | O     |             |                                                               |
| 17       | Order Callback Phone Number              | O     | [0..2]      |                                                               |
| 18       | Placer Field 1                           | O     | [0..1]      |                                                               |
| 19       | Placer Field 2                           | O     | [0..1]      |                                                               |
| 20       | Filler Field 1                           | O     | [0..1]      |                                                               |
| 21       | Filler Field 2                           | O     | [0..1]      |                                                               |
| 22       | Results Report/Status Change - Date/Time | O     | [0..1]      |                                                               |
| 23       | Charge to Practice                       | O     | [0..1]      |                                                               |
| 24       | Diagnostic Service Section ID            | O     | [0..1]      |                                                               |
| 25       | Result Status                            | O     | [0..1]      | “F” in ORU message, and “C” for Correction (Addendum) reports |
| 26       | Parent Status                            | O     | [0..1]      |                                                               |
| 27       | Quantity/Timing                          | O     | [0..*]      |                                                               |
| 28       | Result Copies To                         | O     | [0..5]      |                                                               |
| 29       | Parent                                   | O     | [0..1]      |                                                               |
| 30       | Transportation Mode                      | O     | [0..1]      |                                                               |
| 31       | Reason for Study                         | O     | [0..*]      |                                                               |
| 32       | Principal Result Interpreter             | O     | [0..1]      |                                                               |
| 33       | Assistant Result Interpreter             | O     | [0..*]      |                                                               |
| 34       | Technician                               | O     | [0..*]      |                                                               |
| 35       | Transcriptionist                         | O     | [0..*]      |                                                               |
| 36       | Scheduled Date/Time                      | O     | [0..1]      |                                                               |
| 37       | Number of Sample Containers              | O     | [0..1]      |                                                               |
| 38       | Transport Logistics of Collected Sample  | O     | [0..*]      |                                                               |
| 39       | Collector’s Comment                      | O     | [0..*]      |                                                               |
| 40       | Transport Arrangement Responsibility     | O     | [0..1]      |                                                               |
| 41       | Transport Arranged                       | O     | [0..1]      |                                                               |
| 42       | Escort Required                          | O     | [0..1]      |                                                               |
| 43       | Planned Patient Transport Comment        | O     | [0..*]      |                                                               |
| 44       | Procedure Code                           | O     | [0..1]      |                                                               |
| 45       | Procedure Code Modifier                  | O     | [0..*]      |                                                               |

### OBX

| Sequence | Name                        | Usage | Cardinality | Comment                                                                                                                                                                                                |
| -------- | --------------------------- | ----- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1        | Set ID - OBX                | O     |             |                                                                                                                                                                                                        |
| 2        | Value Type                  | O     | [0..1]      | ORU contains ‘ED’ for Base64-encoded PDF data.                                                                                                                                                         |
| 3        | Observation Identifier      | R     | [1..1]      |                                                                                                                                                                                                        |
| 4        | Obersation Sub-ID           | O     | [0..1]      |                                                                                                                                                                                                        |
| 5        | Observation Value           | O     | [0..*]      | Subfield 1 contains “Adobe Acrobat”, subfield 2 contains “PDF”, subfield 4 contains “Base64” indicating that this data is encapsulated using Base64 encoding, and subfield 5 contains the actual data. |
| 6        | Units                       | O     | [0..1]      |                                                                                                                                                                                                        |
| 7        | References Range            | O     | [0..1]      |                                                                                                                                                                                                        |
| 8        | Abnormal Flags              | O     | [0..5]      |                                                                                                                                                                                                        |
| 9        | Probability                 | O     | [0..1]      |                                                                                                                                                                                                        |
| 10       | Nature of Abnormal Test     | O     | [0..*]      |                                                                                                                                                                                                        |
| 11       | Observation Result Tatus    | R     | [1..1]      | “F” in ORU message, and “C” for Correction (Addendum) reports                                                                                                                                          |
| 12       | Date Last Obs Normal Values |       | [0..1]      |                                                                                                                                                                                                        |
| 13       | User Defined Access Checks  |       | [0..1]      |                                                                                                                                                                                                        |
| 14       | Date/Time of Observation    |       | [0..1]      |                                                                                                                                                                                                        |
| 15       | Producer’s ID               |       | [0..1]      |                                                                                                                                                                                                        |
| 16       | Responsible Observer        |       | [0..*]      |                                                                                                                                                                                                        |
| 17       | Observation Method          |       | [0..*]      |                                                                                                                                                                                                        |

### OBX-3 Observation Identifiers for Discrete Data

No discrete OBX data is transmitted beyond encapsulated Base64-encoded PDF.
