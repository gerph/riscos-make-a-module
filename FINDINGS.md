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

## Implementation Details (from Disassembly)
- **Initialisation:**
  - Claims `EventV` (Event 16).
  - Enables `Event_Internet` (19) and `Event_OutputEmpty` (2) via `OS_Byte 14`.
  - Allocates &180 bytes of workspace via `OS_Module 6`.
- **Finalisation:**
  - Releases `EventV`.
  - Disables events via `OS_Byte 13`.
  - Frees workspace via `OS_Module 7`.
- **Event Handling:**
  - Uses `Event_Internet` to track socket state changes.
  - Uses `Event_OutputEmpty`? (Need to clarify why Event 2 is used, usually it's for buffer empty).
- **Socket Operations:**
  - Uses `Socket_Ioctl` extensively to set sockets to non-blocking mode (FIONBIO).
  - Uses `Socket_Select` in some places (likely for checking if data is ready).
  - Uses `Resolver_GetHost` for background/asynchronous DNS resolution.
  - The module seems to handle asynchronous connection state transitions via events.
- **SWI Dispatch:**
  - Uses a standard branch table for SWI offsets 0-12.
- **Service Handler:**
  - Handles `Service_WimpCloseDown` (&53) to clean up sockets owned by a task. Uses `R0` (task handle) to identify which sockets to close.

## Unclear Parts
- **Event 2:** Why is the module enabling "Character entering buffer" (Event 2)? This is usually for serial/parallel ports or VDU. It might be related to how it handles callbacks or internal task scheduling.
- **Workspace Layout:** The static allocation of &180 bytes suggests some global state beyond the handle list.
- **Magic Numbers:** There are several ADRs to constant blocks that look like error messages or bitmask tables.

## SWI Refinements
- `ESocket_ConnectToHost`: Uses `Resolver_GetHost` for DNS.
- `ESocket_ReadLine`: Supports various line endings (CR, LF, CRLF). Can return data partially if the buffer is full.
- `ESocket_Closed`: Peeks at the socket to check if it's still alive.
- `ESocket_Monitor`: Registers a monitoring block for a socket.
