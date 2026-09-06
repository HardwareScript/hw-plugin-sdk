> [!CAUTION]
> ## ⚰️ PROJECT PERMANENTLY TERMINATED — SEPTEMBER 2026
>
> **The HardwareScript Plugin SDK has been permanently shut down and archived.**
>
> The parent project, HardwareScript, was concluded to be a fundamental category error. The Plugin ABI, WASM plugin host, and HPM registry are all **permanently abandoned**. This repository is preserved as a historical artifact only.
>
> - The `hw-plugin-abi` crate is **permanently archived** — do not build new plugins against it.
> - The WASM64 plugin host runtime is **permanently decommissioned**.
> - The `hw plugin publish` registry is **permanently dissolved**.
> - **No further development, issues, or PRs will be accepted.**
>
> For the full technical post-mortem, see [`Docs/End/Final-Status.md`](../Docs/End/Final-Status.md).
>
> *HardwareScript Architecture Team — September 2026*

---

# Hardware Script Plugin SDK

**Permissively Licensed ABI & Developer SDK for Hardware Script Extensions**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

---

## Overview

The **Hardware Script Plugin SDK** is a standalone, permissively licensed toolkit for building **plugins**, **custom routers**, **synthesis engines**, and **EDA extensions** for the [Hardware Script](https://github.com/hwsl-lang/hardwarescript) compiler.

This SDK is intentionally **isolated from the main AGPLv3 compiler repository** to provide:
- ✅ **Zero licensing friction** for proprietary plugin developers
- ✅ **Clean IP provenance** for foundries and EDA vendors
- ✅ **Pure ABI definitions** with no AGPLv3 contamination
- ✅ **Multi-language bindings** (Rust, C/C++, Zig, and more)

---

## Why a Separate SDK Repository?

### The Problem with Monorepo Plugin Development

If plugin developers had to clone the main AGPLv3 compiler repository just to access ABI headers:
- ❌ Corporate legal scanners would flag the AGPLv3 dependency
- ❌ Risk of accidentally linking against internal AGPLv3 compiler crates
- ❌ Confusion about what can be used in proprietary plugins
- ❌ Developer friction and adoption barriers

### The Solution: Isolated, Permissive SDK

This repository contains **only** the stable ABI definitions and bindings needed to write plugins:
- ✅ Licensed under **MIT / Apache 2.0** (dual-licensed for maximum compatibility)
- ✅ No dependencies on the AGPLv3 compiler internals
- ✅ Safe for use in proprietary, closed-source plugins
- ✅ Clean separation of concerns

---

## Repository Structure

```
hw-plugin-sdk/
├── README.md                    ← This file
├── LICENSE-MIT                  ← MIT License (choose either MIT or Apache 2.0)
├── LICENSE-APACHE               ← Apache 2.0 License
│
├── crates/
│   └── hw-plugin-abi/           ← Pure Rust ABI definitions (#[repr(C)])
│       ├── Cargo.toml
│       └── src/
│           ├── lib.rs
│           ├── types.rs         ← Core ABI types (Point3D, NetId, LayerSpec, etc.)
│           ├── router.rs        ← Router plugin ABI
│           ├── synthesis.rs     ← Logic synthesis plugin ABI
│           └── exporter.rs      ← Custom exporter plugin ABI
│
├── include/
│   └── hardwarescript.h         ← C/C++ header with FFI bindings
│
├── zig/
│   └── hardwarescript.zig       ← Zig module for WASM plugin development
│
├── examples/
│   ├── rust-router/             ← Example: Custom router in Rust
│   ├── zig-router/              ← Example: Custom router in Zig
│   ├── cpp-exporter/            ← Example: Custom exporter in C++
│   └── synthesis-plugin/        ← Example: Logic synthesis plugin
│
└── templates/
    ├── rust-plugin-template/    ← Cargo template for Rust plugins
    ├── zig-plugin-template/     ← Zig build template
    └── cpp-plugin-template/     ← CMake template for C++
```

---

## Quick Start

### 1. Clone the SDK (Safe for Proprietary Use)

```bash
git clone https://github.com/hwsl-lang/hw-plugin-sdk.git
cd hw-plugin-sdk
```

### 2. Choose Your Language

#### Rust Plugin Example

```bash
cd examples/rust-router
cargo build --target wasm64-unknown-unknown --release
```

#### Zig Plugin Example

```bash
cd examples/zig-router
zig build-exe router.zig -target wasm64-freestanding -O ReleaseFast
```

#### C++ Plugin Example

```bash
cd examples/cpp-exporter
mkdir build && cd build
cmake .. -DCMAKE_TOOLCHAIN_FILE=wasm64.cmake
make
```

### 3. Publish Your Plugin

```bash
hw plugin publish router.wasm --name my-custom-router --version 1.0.0
```

---

## ABI Stability Guarantee

The Hardware Script Plugin ABI follows **semantic versioning**:

| Version | Stability | Breaking Changes |
|---------|-----------|------------------|
| `0.x.y` | **Unstable** — ABI may change between minor versions | Possible in minor releases |
| `1.x.y` | **Stable** — ABI frozen for entire 1.x series | Only in major releases (2.0, 3.0, etc.) |

Once the ABI reaches **1.0.0**, plugins compiled against `1.x` will work with all future `1.y` compiler versions.

**Current Status**: Pre-1.0 (ABI subject to change)

---

## What Can You Build?

### Custom Routers

Replace or augment the built-in router with your own pathfinding algorithms:
- **Use case**: Proprietary routing for analog RF layouts
- **Example**: Cadence Virtuoso-style interactive router
- **Target**: `wasm64` compiled from Rust, Zig, or C++

### Logic Synthesis Engines

Plug in custom synthesis backends for ASIC/FPGA designs:
- **Use case**: Map Hardware Script logic to Xilinx, Intel, or custom FPGA architectures
- **Example**: ABC logic synthesis integration
- **Target**: `wasm64` plugin dynamically loaded at compile time

### Custom Exporters

Generate output formats beyond the built-in Gerber/GDSII/OBJ exporters:
- **Use case**: Export to proprietary foundry formats (OASIS, IPC-2581, ODB++)
- **Example**: TSMC-specific PDK exporter
- **Target**: Standalone binary or `wasm64` plugin

### Simulation Backends

Integrate custom SPICE engines or physics solvers:
- **Use case**: Proprietary electromagnetic solvers
- **Example**: HSPICE, Spectre, or ADS integration
- **Target**: `wasm64` plugin with async I/O

---

## License

This project is dual-licensed under your choice of:

- **MIT License** ([LICENSE-MIT](LICENSE-MIT) or https://opensource.org/licenses/MIT)
- **Apache License 2.0** ([LICENSE-APACHE](LICENSE-APACHE) or https://opensource.org/licenses/Apache-2.0)

You may use this SDK under **either license** at your option.

### Why Dual-Licensed?

- **MIT**: Simpler, more permissive, preferred by many open-source projects
- **Apache 2.0**: Includes explicit patent grant, preferred by large corporations

Both licenses allow:
- ✅ Commercial use
- ✅ Modification
- ✅ Distribution
- ✅ Private use
- ✅ Use in proprietary, closed-source plugins

**Copyright © 2024-2026 Olowookere Olamide and HardwareScript Contributors**

---

## Relationship to Main Compiler

This SDK is maintained separately from the [main Hardware Script compiler](https://github.com/hwsl-lang/hardwarescript), which is licensed under **AGPLv3 + Commercial License**.

| Repository | License | Purpose |
|------------|---------|---------|
| [`hwsl-lang/hardwarescript`](https://github.com/hwsl-lang/hardwarescript) | AGPLv3 + Commercial | Core compiler engine (`hwc`, `hwc-router`, `hwc-physics`) |
| [`hwsl-lang/hw-plugin-sdk`](https://github.com/hwsl-lang/hw-plugin-sdk) | MIT / Apache 2.0 | Plugin ABI, headers, and developer tools |

**Key principle**: You can write proprietary, closed-source plugins using this SDK without any AGPLv3 obligations.

---

## Plugin Exception Clarity

The Hardware Script compiler includes a **Plugin Exception** to its AGPLv3 license, which explicitly allows:

1. Loading plugins at runtime via the WASM ABI
2. Distributing proprietary plugins without source code disclosure
3. Using this SDK (MIT/Apache 2.0) to develop closed-source extensions

See the main compiler repository's [COMPILER-OUTPUT-EXCEPTION.md](https://github.com/hwsl-lang/hardwarescript/blob/main/COMPILER-OUTPUT-EXCEPTION.md) for full legal text.

---

## Contributing

Contributions to the SDK are welcome! Since this is permissively licensed, there is no CLA requirement.

### Contribution Guidelines

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/zig-bindings`
3. **Make your changes**: Add new language bindings, examples, or documentation
4. **Test your changes**: Ensure ABI compatibility
5. **Submit a pull request**

All contributions will be licensed under MIT / Apache 2.0 (dual-licensed).

---

## Community & Support

- **GitHub Issues**: [hw-plugin-sdk/issues](https://github.com/hwsl-lang/hw-plugin-sdk/issues)
- **Discord**: [Hardware Script Community](https://discord.gg/9zqH8nuCet)
- **Email**: hardwarescript@gmail.com
- **Documentation**: [https://docs.hardwarescript.org](https://docs.hardwarescript.org)

---

## Roadmap

### v0.3.x (Current)
- [x] Core Rust ABI definitions (`hw-plugin-abi`)
- [x] C/C++ header generation
- [ ] Zig module bindings
- [ ] Example: Custom router in Zig
- [ ] Example: GDSII exporter in Rust

### v0.4.0
- [ ] Stable router plugin ABI
- [ ] Python bindings (via `pyo3`)
- [ ] TypeScript/JavaScript bindings (via `wasm-bindgen`)
- [ ] Plugin versioning and compatibility checks

### v1.0.0 (ABI Freeze)
- [ ] Frozen ABI with semantic versioning guarantee
- [ ] Comprehensive test suite for ABI stability
- [ ] Multi-language binding generators
- [ ] Plugin marketplace integration

---

## FAQ

### Can I use this SDK in a proprietary, closed-source project?

**Yes.** This SDK is dual-licensed under MIT / Apache 2.0, which allows commercial use without source code disclosure.

### Do I need a commercial license from Hardware Script to use this SDK?

**No.** The SDK is permissively licensed. You only need a commercial license if you're modifying the core AGPLv3 compiler itself or running it as a hosted service.

### Can I distribute plugins built with this SDK without open-sourcing them?

**Yes.** Plugins are treated as separate works under the Hardware Script Plugin Exception. You own your plugin code completely.

### What if the ABI changes?

Before 1.0.0, the ABI may change between minor versions. After 1.0.0, the ABI will be frozen for the entire 1.x series (breaking changes only in 2.0, 3.0, etc.).

### Can I submit a plugin to the official Hardware Script registry?

**Yes.** Use the `hw plugin publish` command. Your plugin can be open-source (MIT, Apache 2.0) or proprietary—both are allowed.

### How do I report bugs or request features?

Open an issue on [GitHub](https://github.com/hwsl-lang/hw-plugin-sdk/issues) or join the [Discord community](https://discord.gg/9zqH8nuCet).

---

## Acknowledgments

This SDK uses stable ABI patterns inspired by:
- **Rust's `std::ffi`** — Cross-language FFI with `#[repr(C)]`
- **LLVM Plugin API** — Version-stable plugin interfaces
- **WebAssembly Component Model** — Language-agnostic module composition
- **Qt Plugin System** — Runtime extensibility with clean ABI boundaries

---

## Legal Notice

This SDK provides ABI definitions for interoperability with the Hardware Script compiler. It does **not** include any AGPLv3-licensed code from the main compiler.

Plugins built with this SDK are **separate works** under the Hardware Script Plugin Exception and are **not** subject to AGPLv3 copyleft requirements.

**Disclaimer**: This SDK is provided "as-is" without warranty of any kind. See [LICENSE-MIT](LICENSE-MIT) and [LICENSE-APACHE](LICENSE-APACHE) for full terms.

---

**Last Updated**: March 18, 2026  
**Version**: 0.3.0  
**Maintained by**: Olowookere Olamide and the Hardware Script Community
