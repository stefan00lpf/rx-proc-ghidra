# rx-proc-ghidra

This Ghidra module adds support for disassembling and decompiling Renesas RX architecture binaries.

This module is a work in progress, but currently implements disassembly and decompilation rules for
most of the instructions supported by the RX62T processor.

## Installation

`unzip ghidra_12.0.3_PUBLIC_20250415_RX62T.zip -d ${ghidra_install_dir}/Ghidra/Extensions`

## Building

`gradle -PGHIDRA_INSTALL_DIR=<ghidra_install_dir>`
