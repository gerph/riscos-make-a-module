# Work Log - EasySocket Implementation

## Progress
- [x] Reverse-engineered `EasySocket,ffa` to identify SWIs and commands.
- [x] Documented findings in `FINDINGS.md`.
- [x] Created C module skeleton.
- [x] Added SWI and command handlers (stubs).
- [x] Successfully built the module.
- [x] Implemented `ESocket_ConnectToHost`, `ESocket_CheckState`, `ESocket_Forget`.
- [x] Implemented `ESocket_SendLine`, `ESocket_ReadLine`, `ESocket_SendData`, `ESocket_ReadData`, `ESocket_Closed`.
- [x] Implemented `ESocket_Listen`, `ESocket_Accept`, `ESocket_ConnectionName`.
- [x] Implemented `*ESockets` command.
- [x] Linked with `TCPIPLibs` (zm versions).

## Features Added
- Full SWI set for EasySocket &58000.
- Socket state management with internal buffering for line reading.
- Support for listening and accepting connections.
- Command-line interface for connection monitoring.

- [x] **Event Handling:** Module claims `EventV` and enables events 19 and 2.
- [x] **Non-blocking I/O:** Sockets are set to non-blocking mode using `ioctl`.
- [x] **WimpCloseDown Logic:** Implemented task-specific cleanup using Wimp task handles.
- [x] **SWI DecodeState (&0D):** Implemented conversion of state numbers to strings.
- [x] **SWI OurAddress (&0E):** Implemented local address/port retrieval.
- [x] **SWI TheirAddress (&0F):** Implemented remote address/port retrieval.
- [x] **SendLine Flags:** Implemented CR/LF control and 13-terminated string support.
- [x] **ReadLine Flags:** Implemented terminator types, 13-term, delete processing, and buffer small handling.
- [x] **ReadData available:** Implemented return of available bytes when R1=0.
- [x] **Listen Allocation:** Port 0 now allocates a new port.
- [x] **Accept Flags:** Added "Close listener" flag support.
- [x] **Closed Flags:** Added "Ignore buffer" flag support.
- [x] **Monitor Integration:** Monitors trigger based on socket events (pollword logic).
