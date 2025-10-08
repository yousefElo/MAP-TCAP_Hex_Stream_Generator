# MAP TCAP Hex Stream Generator

## Overview
This project provides a standalone web page (`index.html`) that lets you compose MAP `Begin` messages and export the resulting TCAP hex stream. The UI exposes the most common MAP operations used for subscriber management and SMS procedures and converts human-readable parameters into the appropriate ASN.1 encoded payload.

## Features
- Generate TCAP `Begin` messages for several MAP operations, including `updateLocation`, `sendAuthenticationInfo`, `sendRoutingInfo`, and SMS forwarding/reporting procedures
- Automatic encoding of IMSI, ISDN, and other MAP argument fields to the correct TBCD/ASN.1 format
- SMS text input mode: Enter SMS content as clear ASCII text instead of raw TPDU hex for MO/MT ForwardSM operations
- Built-in catalog of MAP v3 Application Context OIDs with optional custom entry
- Interactive breakdown showing the OTID, dialog portion, component portion, and MAP argument
- Clipboard copy helper for direct use in external SS7 traffic simulators

## System Requirements
- A modern desktop browser with JavaScript enabled (tested with current Chrome, Edge, and Firefox releases)
- No server-side or build tooling is required; the page runs entirely in the browser

## Dependencies
No external packages are needed. All logic and styling are contained within `index.html`.

## Usage
1. Open `index.html` in your browser (double-click the file or drag it into an open browser window).
2. Select the desired MAP operation from the dropdown.
3. Provide the required parameters (IMSI, MSC/VLR numbers, etc.). Additional fields appear automatically based on the operation.
4. Adjust the application context if necessary using the provided list or choose **Custom** to enter an alternate OID.
5. Click **Generate Hex Stream** to produce the TCAP message.
6. Copy the hex output using the **Copy Hex** button and paste it into your SS7/TCAP simulator workflow.

## Customization
- **Additional operations**: Extend the `operations` object in `index.html` with new field definitions and encoding logic.
- **Application contexts**: Amend the `applicationContexts` array to add or reorder MAP ACNs.
- **Styling**: Modify the CSS block near the top of `index.html` to match your preferred theme.

## Testing
Manual testing is recommended: generate a hex stream with known parameters and compare the result to trusted captures or simulator expectations. Because the page is self-contained, no automated test harness is bundled.

## Reference: MAP/CAP/EIR Application Contexts

### OID Root Segments
- `0` – ITU-T
- `4` – identified-organization
- `0` – ETSI
- `0.1.0.1.1` – GSM-MAP
- `0.4.0.0.1.0.1.1` – MAP
- `0.4.0.0.1.0.50.1` – CAMEL (CAP)
- `0.4.0.0.1.0.1.2` – EIR

### MAP Application Contexts (v3)

| MAP Application Context               | Description                   | OID                      |
| ------------------------------------- | ----------------------------- | ------------------------ |
| networkLocUpContext-v3                | Location Update               | `0.4.0.0.1.0.1.1.1.1.3`  |
| locationCancellationContext-v3        | Cancel Location               | `0.4.0.0.1.0.1.1.1.2.3`  |
| roamingNumberEnquiryContext-v3        | Provide Roaming Number        | `0.4.0.0.1.0.1.1.1.3.3`  |
| interVlrInfoRetrievalContext-v3       | Send Parameters               | `0.4.0.0.1.0.1.1.1.4.3`  |
| msPurgingContext-v3                   | Purge MS                      | `0.4.0.0.1.0.1.1.1.5.3`  |
| subscriberDataMngtContext-v3          | Insert Subscriber Data        | `0.4.0.0.1.0.1.1.1.6.3`  |
| authenticationFailureReportContext-v3 | Auth Failure Report           | `0.4.0.0.1.0.1.1.1.7.3`  |
| resetContext-v3                       | Reset                         | `0.4.0.0.1.0.1.1.1.8.3`  |
| alertingContext-v3                    | Alert SC                      | `0.4.0.0.1.0.1.1.1.9.3`  |
| readyForSMContext-v3                  | Ready for Short Message       | `0.4.0.0.1.0.1.1.1.10.3` |
| moForwardSMContext-v3                 | Mobile-Originated SMS Forward | `0.4.0.0.1.0.1.1.1.11.3` |
| mtForwardSMContext-v3                 | Mobile-Terminated SMS Forward | `0.4.0.0.1.0.1.1.1.12.3` |
| reportSM-DeliveryStatusContext-v3     | Delivery Report (Legacy)      | `0.4.0.0.1.0.1.1.1.13.3` |
| reportSM-DeliveryOutcomeContext-v3    | Outcome Report                | `0.4.0.0.1.0.1.1.1.14.3` |
| infoRetrievalContext-v3               | Send Authentication Info      | `0.4.0.0.1.0.14.3` |
| shortMsgGatewayContext-v3             | Report SM Delivery Status     | `0.4.0.0.1.0.20.3` |
| istAlertingContext-v3                 | IST Alert                     | `0.4.0.0.1.0.1.1.1.21.3` |
| callControlTransferContext-v3         | Call Control Transfer         | `0.4.0.0.1.0.1.1.1.25.3` |

### CAMEL (CAP) Application Contexts

| CAP Application Context | Description            | OID                    |
| ----------------------- | ---------------------- | ---------------------- |
| cap-dpsmsAC-v3          | SMS Control            | `0.4.0.0.1.0.50.1.1.3` |
| cap-dpGPRSAC-v3         | GPRS Control           | `0.4.0.0.1.0.50.1.2.3` |
| cap-dpBCSMAC-v3         | Basic Call State Model | `0.4.0.0.1.0.50.1.3.3` |
| cap-dpSMSAC-v4          | SMS Control (v4)       | `0.4.0.0.1.0.50.1.4.4` |
| cap-dpBCSMAC-v4         | Call Control (v4)      | `0.4.0.0.1.0.50.1.5.4` |

### EIR Application Contexts

| EIR Application Context | Description | OID                   |
| ----------------------- | ----------- | --------------------- |
| checkIMEIContext-v3     | Check IMEI  | `0.4.0.0.1.0.1.2.1.3` |

### OAM / HLR / VLR Special Contexts

| Application Context                        | Description                | OID                      |
| ------------------------------------------ | -------------------------- | ------------------------ |
| hlrSubscriberDataMngtContext-v3            | Subscriber Data Mgmt       | `0.4.0.0.1.0.1.1.1.6.3`  |
| anyTimeInterrogationContext-v3             | Any Time Interrogation     | `0.4.0.0.1.0.1.1.1.15.3` |
| anyTimeSubscriptionInterrogationContext-v3 | Any Time Sub Interrogation | `0.4.0.0.1.0.1.1.1.16.3` |

### MAP Protocol Version Bitmasks

| Raw BIT STRING bytes | Explanation              | Supported MAP version(s) |
| -------------------- | ------------------------ | ------------------------ |
| `07 80`              | 1 bit used (bit 2)       | v3 only                  |
| `06 C0`              | 2 bits used (bits 1,2)   | v2 + v3                  |
| `05 E0`              | 3 bits used (bits 0,1,2) | v1 + v2 + v3             |
| `07 20`              | 1 bit used (bit 1)       | v2 only                  |
