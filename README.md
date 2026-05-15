Thread in UC: https://www.unknowncheats.me/forum/counter-strike-2-a/743326-essentials-external.html

Architecture

This is a Windows x64 CS2 game overlay with five main layers:

Memory (`Memory/`)
Low-level cross-process I/O via `NtReadVirtualMemory`. `handle_hijack.h` acquires a privileged handle to the game process through handle pool hijacking.

### Core (`Core/`)
`CGame` (in `MemoryManager.hpp/.cpp`) is the central state manager. It runs **five background threads** — worker, player, items, visibility, and resolver — that continuously scan and update game state. All shared data uses **double-buffered `GameSnapshot` structures** with atomic memory ordering for lock-free reads from the render thread. `bvh.cpp` builds a bounding volume hierarchy over map geometry for wallbang/penetration calculations.

Features (`Features/`)
Self-contained modules that consume `GameSnapshot` data:
- **Aimbot** — target acquisition and aim correction
- **Chams** — custom player model rendering; uses a 28-bone skeleton and 3×4 bone matrices; parses VPK assets for agent models
- **RCS** — recoil compensation
- **Triggerbot** — auto-fire with body-segment hit detection
- **BulletTracer** — visual bullet path rendering

Renderer (`Renderer/`)
D3D11 overlay rendered as a transparent `DirectComposition` window on top of the game. `zdraw/` is the internal drawing library. FreeType handles font rasterization. ImGui (two versions: `imgui/` custom and `imguiMASTER/` with D3D11/Win32 backends) drives the configuration menu.

Offsets (`Offsets/`)
`offsets.hpp` and `client_dll.hpp` are **generated files** from [cs2-dumper](https://github.com/a2x/cs2-dumper). When the game updates and breaks offsets, regenerate these files from the dumper rather than editing them by hand. `SchemaOffsets` in `MemoryManager` handles runtime-resolved schema member offsets.

Key conventions

- `xorstr.h` wraps all string literals that should be obfuscated at compile time — use `xorstr_("…")` for any string that should not appear in the binary plaintext.
- Math lives in `Util/Math.hpp` / `MathUtils.h`; prefer those types (`Vector3`, matrix helpers) over rolling new math.
- Config serialization (save/load) is in `Util/Config/`; feature settings are persisted through that system.
- Entry point `main.cpp` spoofs the process name to `RuntimeBroker.exe` on startup via `spoof_name.h`.

- List of main features:
- Features:
- Aimbot
- Triggerbot
- Magnetic triggerbot
- Autowall
- Standalone RCS
- Player ESP
- Item ESP
- Chams
- Kill effect
- Bullet tracers
- World particles
- Bomb timer
- Defusing info
- Spectator list
- 2D Radar/minimap
- Weapon group specific settings
- Automatic config creation, saving, loading
- OBS stream proof

<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/9c277d35-3cf4-49f0-a6da-b61b62e6a110" />
