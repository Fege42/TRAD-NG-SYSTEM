# Roblox Server Security & Network Defense Suite

A production-ready, high-performance security layer designed for Roblox servers to mitigate client-side physics exploitation and malicious network flooding (DoS/DDoS). Built entirely using strict Luau (`--!strict`), Object-Oriented Programming (OOP), and automated network interception.

## Core Modules

### 1. Anti-Exploit System (`AntiExploitManager`)
Manages real-time server-side verification of player physics and movement to eliminate common client-side exploits without relying on client-side scripts.
* **Speed Hack & Teleportation Filtering:** Tracks horizontal displacement over high-precision delta time windows, dynamically adjusting for walking speeds and server-authorized exemptions.
* **Fly & Levitation Mitigation:** Implements downwards raycasting logic combined with humanoid state analysis to detect artificial floating or mid-air flight suspension.
* **Rubberband Enforcement:** Automatically forces offending clients back to their last validated server position and handles state resets to minimize false positives during network spikes.

### 2. Anti-DDoS & Payload Filter (`AntiDdosManager`)
Interceptors that sit between incoming client network traffic and the server execution queue to prevent server crashing, memory exhaustion, and remote spamming.
* **Layer 7 Rate Limiting:** Implements sliding-window throttling globally per player and locally per specific `RemoteEvent` node.
* **Deep Payload Inspection:** Recursively checks incoming argument depth to block stack overflow / infinite recursion crash exploits.
* **Data Serialization Guard:** Sanitizes incoming parameters, automatically blocking oversized string payloads designed to allocate massive server heap memory.

## Architecture & Integration

* **Automated Injection:** The initialization scripts automatically crawl `ReplicatedStorage` to wrap existing network nodes, allowing drag-and-drop integration into existing frameworks.
* **OOP State Isolation:** Every joining player receives isolated session allocation tables, preventing cross-contamination of networking counters and physics snapshots.
* **Transactional Defenses:** Heavy use of native `pcall` structures ensures network spikes or corrupt payloads fail gracefully rather than crashing the core server threads.

## Implementation Structure
* `ServerScriptService/AntiExploitSystem/` — Contains the movement tracking loops and physics calculation engines.
* `ServerScriptService/AntiDdosSystem/` — Hosts the network interceptors, depth-check analytics, and memory guards.
