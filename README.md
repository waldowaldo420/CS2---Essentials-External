Thread in UC: https://www.unknowncheats.me/forum/counter-strike-2-a/743326-essentials-external.html

Preview: https://www.youtube.com/watch?v=-wcEuV0vqLA

Discord for any questions: https://discord.gg/vPh3pYcSXk

Architecture

Memory (`Memory/`)
Low-level cross-process I/O via `NtReadVirtualMemory`. `handle_hijack.h` acquires a privileged handle to the game process through handle pool hijacking.

### Core (`Core/`)
`CGame` (in `MemoryManager.hpp/.cpp`) is the central state manager. It runs **five background threads** — worker, player, items, visibility, and resolver — that continuously scan and update game state. All shared data uses **double-buffered `GameSnapshot` structures** with atomic memory ordering for lock-free reads from the render thread. `bvh.cpp` builds a bounding volume hierarchy over map geometry for wallbang/penetration calculations.

Offsets (`Offsets/`)
`offsets.hpp` and `client_dll.hpp` are **generated files** from [cs2-dumper](https://github.com/a2x/cs2-dumper). When the game updates and breaks offsets, regenerate these files from the dumper rather than editing them by hand. `SchemaOffsets` in `MemoryManager` handles runtime-resolved schema member offsets.

Key conventions

- `xorstr.h` wraps all string literals that should be obfuscated at compile time — use `xorstr_("…")` for any string that should not appear in the binary plaintext.
- Math lives in `Util/Math.hpp` / `MathUtils.h`; prefer those types (`Vector3`, matrix helpers) over rolling new math.
- Config serialization (save/load) is in `Util/Config/`; feature settings are persisted through that system.
- Entry point `main.cpp` spoofs the process name to `RuntimeBroker.exe` on startup via `spoof_name.h`.

List of main features:
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
- Lineup helper
- Grenade prediction
- Weapon group specific settings
- Automatic config creation, saving, loading
- OBS stream proof

How to update:
- Download and run a2x dumper while game is running
- Navigate to "output" folder and copy files: offsets.json & client_dll.json
- Paste offsets into the same folder with .exe

How to use:
- Run cs2 in FULLSCREEN WINDOWED
- Open .exe

Other:
- Works on every resolution
- Program can be opened before or during the game

Disclaimer:
- This project exists purely as a programming exercise. Using memory reading or overlay software in online games violates terms of service and is unfair to other players. This project should never be used in any online game or competitive environment.

<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/9c277d35-3cf4-49f0-a6da-b61b62e6a110" />

<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/3624df8d-dcf9-4eaf-befc-3dadd1a3323b" />

