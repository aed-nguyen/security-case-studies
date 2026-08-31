# OverTheWire

[OverTheWire](https://overthewire.org/wargames/) is a collection of security wargames covering web security, cryptography, reverse engineering, binary exploitation, networking, and Linux behaviour.

[Code from the solved labs](https://github.com/aed-nguyen/security-boundary-demos#lab-tooling)

## Progress

| Game | Progress | Transitions |
| --- | --- | ---: |
| Natas | Levels 0 through 25 | 26 |
| Leviathan | Complete | 7 |
| Krypton | Complete | 7 |
| Narnia | Complete | 9 |
| Behemoth | Complete | 8 |
| Utumno | Complete through the current final account | 8 |
| Maze | Complete | 9 |
| Vortex | Levels 0 through 22 | 23 |
| Manpage | Levels 0 through 5 | 6 |
| **Total** |  | **103** |

## Strategies

### Natas

The Natas levels moved from source inspection and client-controlled state into path traversal, command injection, SQL injection, blind extraction, session handling, upload validation, type confusion, and log poisoning.

I wrote helpers for XOR cookie manipulation, boolean and timing-based SQL extraction, command-side blind extraction, session ID enumeration, and log poisoning. Request headers, cookies, query strings, POST bodies, redirects, and serialized values all needed to match the application code exactly.

### Leviathan

Leviathan focused on small privileged programs that trusted hard-coded values, filenames, shell command strings, and predictable temporary paths. `ltrace` exposed normal string comparisons, while `objdump` was more useful than brute force when a numeric check was visible in the binary.

The main pattern was a gap between the security check and the action that followed it. A filename could be checked as one path and later split by a shell, or a shared temporary name could be replaced before a privileged program opened it.

### Krypton

Krypton started with Base64 and fixed rotations, then moved into chosen plaintext, monoalphabetic substitution, Vigenere analysis, period discovery, and deterministic stream reuse.

Word shapes, English letter frequencies, index of coincidence, and known plaintext reduced the amount of guessing. The final cipher didn't require recovering every internal component because a chosen input exposed the combined shift stream directly.

### Narnia

Narnia covered stack layout, executable environment data, return-address control, overlapping path buffers, format strings, function pointers, and a copy loop that corrupted its own source pointer.

Useful tools included `objdump`, `readelf`, `checksec`, `ldd`, debugger breakpoints, and byte-level payload builders. Several levels depended on calculating the live layout for the final payload length instead of trusting an address measured under a debugger.

### Behemoth

Behemoth combined traced secrets, stack control, unsafe executable lookup, format-string writes, predictable temporary files, loopback UDP interception, weak shellcode filtering, and incomplete input validation.

One level sent sensitive data to a fixed UDP port on localhost, so a one-datagram listener could receive it first. Another blocked one syscall byte but still allowed enough machine code to satisfy the parent program's trust check.

### Utumno

Utumno used execute-only mappings, filename bytes copied into executable memory, unusual argument layouts, constrained writes, integer-width mistakes, signed arithmetic wraparound, and saved process state.

I recovered an execute-only mapped image through `ptrace`, used small 32-bit launchers to control argument and environment layout, and tracked the exact width and signedness of values through each check and later memory operation.

### Maze

Maze covered check-then-open races, relative library loading, tiny control-flow trampolines, self-decrypting code, minimal ELF construction, reversible key arithmetic, buffered stream structures, unsafe binary parsing, and a network format string.

The work required short-lived local clients and servers, exact ELF layouts, raw-address disassembly, and post-decryption memory inspection. A 120-byte executable and an eight-byte trampoline were enough for two of the levels because each file only needed the minimum structure required by the next operation.

### Vortex

For Vortex, I adapted public techniques to the current binaries and rebuilt the working parts against the live service.

The work included little-endian network exchanges, pointer walking, process-start layout, MD5 brute force, return chains, signal delivery between threads, predictable random streams, heap metadata, RC4 traffic recovery, restricted-key analysis, a Hamming-score oracle, signed arithmetic, negative array indexes, and process-dependent key generation.

For level 16, a custom Metal MD5 kernel on an M4 tested about 200 million candidates per second until the service accepted a 102-bit match against a recovered target digest.

### Manpage

Manpage focused on Linux and C behaviour that changes the outcome of otherwise familiar memory bugs. The first six transitions used stable loader control transfers, signal inheritance, file descriptors surviving `exec`, FIFO timing, a patched Hunt the Wumpus build, and uninitialized stack data.

Manpage 4 used a 2,035-byte input spray and a measured 1,304-byte distance from the parser buffer to an uninitialized surname field. Manpage 5 used raw bytes in a loader variable to line up `0xdeadbeef` with an uninitialized local after recursive stack frames advanced in 20-byte steps.

## Tools

- `file`, `readelf`, `objdump`, `gdb`, `ltrace`, `strace`, `checksec`, and live process inspection
- Python, C, shell, PHP, Objective-C, and Metal tooling
- Binary-safe TCP and UDP helpers
- Source-led HTTP requests and small extraction scripts
- Classical cipher analysis, pseudo-random generator recovery, MD5 search, and RC4 traffic analysis

## Lab Learnings

- A check only matters if the next operation uses the same bytes, path, identity, type, and length.
- Debugger addresses can be wrong for privileged execution. Stable control transfers and live measurements were more reliable.
- File descriptors, signal state, argument layout, environment layout, and process identity can matter as much as the vulnerable line of code.
- A chosen input can expose a cipher shift, stream, parser difference, or validation gap without searching the full space.
- Binary-safe tooling removes ambiguity when terminal buffering, null bytes, timing, or byte order affect the result.
- Small flaws combine. A short write, one unchecked suffix, or a predictable process value can be enough when it reaches control data.
