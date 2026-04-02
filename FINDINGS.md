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

| Offset | Name | Functionality |
| :--- | :--- | :--- |
| &00 | `ESocket_ConnectToHost` | Connects to a remote host (R0: hostname, R1: port). Returns handle in R0. |
| &01 | `ESocket_CheckState` | Checks the status of a socket handle (R0: handle). Returns state in R0 (1=Lookup, 2=Connecting, 4=Connected, <0=Error). |
| &02 | `ESocket_Forget` | Closes/discards a socket handle or monitor (R0: handle). |
| &03 | `ESocket_SendLine` | Sends a string (R0: handle, R1: string, R2: flags). Flags: b0=No CR, b1=No LF, b2=13-terminated string. |
| &04 | `ESocket_ReadLine` | Reads a line (R0: handle, R1: buffer, R2: size, R3: flags). Returns buf in R1, len in R2. Flags: b0,1=Terminator (00=CRLF, 01=LF, 10=CR, 11=Any), b2=13-term, b3=Process deletes. |
| &05 | `ESocket_SendData` | Sends raw data (R0: handle, R1: data, R2: length). Returns bytes sent in R0. |
| &06 | `ESocket_ReadData` | Reads raw data (R0: handle, R1: buffer, R2: size). If R1=0, returns bytes available in R2. |
| &07 | `ESocket_Closed` | Checks if a socket is closed (R0: handle, R1: flags). Returns 1 if closed. Flags: b0=Ignore internal buffer. |
| &08 | `ESocket_Listen` | Listens on a local port (R0: port). Port 0 allocates. Returns handle or <0 error. |
| &09 | `ESocket_Accept` | Accepts connection (R0: listen handle, R1: flags). Flags: b0=Close listener. Returns new handle or -8. |
| &0A | `ESocket_ConnectionName` | Gets remote host name/IP (R0: handle). Returns pointer in R0. Resolved multitaskingly. |
| &0B | `ESocket_Monitor` | Creates a monitor (R0: type=0, R1: handle). Returns monitor pointer. |
| &0C | `ESocket_ResetMonitor` | Resets a monitor (R0: monitor, R1: newaddress=0). Returns pollword address in R0. |
| &0D | `ESocket_DecodeState` | NEW (from docs): Converts state number to error string. |
| &0E | `ESocket_OurAddress` | NEW (from docs): Returns local address and port (R0: handle). Exit R0:IP, R1:port. |
| &0F | `ESocket_TheirAddress` | Returns remote address and port (R0: handle). Exit R0:IP, R1:port. |
| &10 | `ESocket_TaskAssociation` | NEW: Associates/Dissociates task (R0: handle, R1: 1=Assoc, 0=Dissoc). |
| &11 | `ESocket_ConnectToAddr` | NEW: Connects to IP (R0: IP, R1: port). |

## Implementation Details (from Source)
- **Magic Numbers:** Socket Magic = `&FEEF1EF0`, Monitor Magic = `&BEEFFEED`.
- **Initialisation:** Claims `EventV` (16), enables events 2 and 19. Also installs a Wimp PostFilter to trap `TaskWindow_Input` (&808C0) messages.
- **Domain IDs:** Uses the value at `[0, #&FF8]` as a "domain ID" to track which task owns a socket/monitor.
- **Monitors:**
  - Type 0: Triggers when:
    - `Event_Internet` (19) subreason 1 occurs for the socket.
    - `Event_Internet` (19) subreason 3 occurs for the socket (closed).
    - `Event_BufferEntering` (2) subreason 0 occurs (keyboard).
    - `TaskWindow_Input` message is received via PostFilter.
- **State Machine:**
  - `checksockets` runs during `CheckState` and other SWIs.
  - Handles `Resolver_GetHost` results (pending code 36).
  - Uses `Socket_Select` (via `selectready`) to check for connection completion.
- **Buffering:** Doubling buffer size (initial 256, max 32768).
- **Cleanup:** `swi_forget` correctly closes socket handles and frees all associated memory (internal buffer, remote name string). Original module failed to clean up on `RMKill` by choice ("to dissuade you"), but our C version should be clean.

## Unclear Parts
- **Event 2:** The source confirms it triggers all type 0 monitors on any keyboard activity. This is quite broad but matches the "TaskWindow" use case.
- **PostFilter:** Implementation of Wimp filters in C modules requires a veneer and specific setup in CMHG.
