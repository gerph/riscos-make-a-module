# EasySocket

A high-level RISC OS networking module that provides a simplified interface to the TCP/IP stack.

## Overview

EasySocket (originally developed by Justin Fletcher) abstracts the complexities of the standard RISC OS Socket SWIs. It is designed for ease of use in multitasking environments and high-level languages like BBC BASIC.

This repository contains a modern C reimplementation of the module, maintaining full binary and API compatibility with version 1.16a of the original assembly module while addressing historical deficiencies such as memory leaks.

## Key Features

- **Asynchronous Operations:** Non-blocking connection establishment and DNS resolution.
- **Socket Monitors:** Simplified event notification via pollwords for Wimp applications.
- **String Handling:** Specialized SWIs for line-based reading and writing with support for various terminators.
- **Task-Aware Cleanup:** Automatically closes sockets when the owning Wimp task terminates.
- **Robustness:** Built-in magic number validation and safe handle management.

## Components

- `EasySocket`: The main module (&51C00 chunk).
- `*ESockets`: CLI command to list active connections.
- `*EMonitors`: CLI command to list active socket monitors.

## Build Instructions

To build the module, ensure you have the RISC OS Build System tools installed:

```bash
riscos-amu
```

The built module will be located in `rm32/EasySocket,ffa`.

## Documentation

Comprehensive API documentation is available in `EasySocket.xml` (PRM-in-XML format).

## License

This project is licensed under the MIT License - see the `LICENSE` file for details.

Copyright (c) 2026 Justin Fletcher.
