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
- [ ] **DNS Resolution:** Original uses `Resolver_GetHost` (likely asynchronous). Current implementation uses `gethostbyname` (blocking).
- [ ] **Workspace:** Original uses a fixed workspace block for some global state.
