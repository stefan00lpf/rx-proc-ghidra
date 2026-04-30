# AGENTS.md

## Cursor Cloud specific instructions

This is a **Ghidra processor extension** for the Renesas RX architecture (RX62T / RXv2). It is a single Gradle-built Java project—no web services, databases, or containers.

### Dependencies

- **JDK 21** (system-provided)
- **Gradle 8.12** installed at `/opt/gradle` (add `/opt/gradle/bin` to `PATH`)
- **Ghidra 12.0.4** installed at `/opt/ghidra` (symlink to `/opt/ghidra_12.0.4_PUBLIC`)

### Build

```
export PATH="/opt/gradle/bin:$PATH"
gradle -PGHIDRA_INSTALL_DIR=/opt/ghidra
```

Output zip is in `dist/`.

### Install extension into Ghidra

```
sudo unzip -o dist/ghidra_12.0.4_PUBLIC_*.zip -d /opt/ghidra/Ghidra/Extensions/
sudo chmod -R a+rw /opt/ghidra/Ghidra/Extensions/workspace/data/languages/
```

The `chmod` is required because Ghidra compiles `.slaspec` → `.sla` at runtime and needs write access.

### Test with Ghidra headless

```
/opt/ghidra/support/analyzeHeadless /tmp ghidra_test -import <binary> \
  -processor "rx62t:LE:32:default" -cspec default -loader BinaryLoader
```

The language ID is `rx62t:LE:32:default` (defined in `data/languages/rx62t.ldefs`).

### Key gotchas

- There are **no automated unit tests** in this repo. Validation is done by building the extension and loading binaries in Ghidra headless mode.
- The `.classpath` file references `/opt/ghidra` — this path must match the Ghidra installation location.
- The SLEIGH spec files (`data/languages/rx62t.slaspec`, `rx62t.sinc`) define the processor's instruction set. Ghidra compiles them to `.sla` on first use; ensure write permissions on the languages directory.
- This project has **no lint tool** configured. Code style follows standard Ghidra extension conventions.
