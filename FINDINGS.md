# EasySocket Module Findings

## Overview
The `EasySocket` module (version 1.03, 28 Jan 1998) is a networking utility for RISC OS developed by Justin Fletcher. It provides a high-level abstraction over the standard RISC OS socket SWIs, making it easier to perform network operations.

## Module Details
- **Title:** EasySocket
- **Help String:** EasySocket 1.03 (28 Jan 1998) © Justin Fletcher
- **SWI Chunk Base:** &58000
- **SWI Prefix:** ESocket

## SWI Calls
The following SWIs are implemented (chunk &58000):

| Offset | Name | Possible Functionality |
| :--- | :--- | :--- |
| &00 | `ESocket_ConnectToHost` | Connects to a remote host (R0: hostname, R1: port). Returns handle in R0. |
| &01 | `ESocket_CheckState` | Checks the status of a socket handle (R0: handle). Returns state in R0. |
| &02 | `ESocket_Forget` | Closes/discards a socket handle (R0: handle). |
| &03 | `ESocket_SendLine` | Sends a string followed by CRLF (R0: handle, R1: string). |
| &04 | `ESocket_ReadLine` | Reads a line of text (R0: handle, R1: buffer, R2: size). |
| &05 | `ESocket_SendData` | Sends raw data (R0: handle, R1: data, R2: length). |
| &06 | `ESocket_ReadData` | Reads raw data (R0: handle, R1: buffer, R2: size). |
| &07 | `ESocket_Closed` | Checks if a socket is closed (R0: handle). Returns status in R0. |
| &08 | `ESocket_Listen` | Listens for connections on a local port (R0: port). Returns listening handle in R0. |
| &09 | `ESocket_Accept` | Accepts an incoming connection (R0: listening handle). Returns new handle in R0. |
| &0A | `ESocket_ConnectionName` | Gets the remote host name/IP of a connection (R0: handle). Returns pointer in R0. |
| &0B | `ESocket_Monitor` | Possibly related to monitoring socket activity. |
| &0C | `ESocket_ResetMonitor` | Resets socket monitoring statistics. |

## Commands
The module provides the following * commands:
- `*ESockets`: Lists active socket connections.
- `*EMonitors`: Lists or manages socket monitors.

## Implementation Notes
- The module is likely a wrapper around the `Socket_` SWIs (&41200).
- It uses a custom handle-based system to track socket state.
- It likely handles non-blocking I/O and buffering internally.
