# NetPulse Gamer

Unified Windows desktop utility for Wi-Fi benchmarking, game server diagnostics, and real-time bandwidth prioritization.

## Build

1. Install [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
2. Double-click `BUILD.bat`
3. Output: `publish\NetPulseGamer.exe` (~65 MB, fully self-contained)

## Run

**Right-click → Run as Administrator** (required for registry writes, QoS policies, adapter changes).

## Modules

| Module | What it does |
|---|---|
| **Servers & DNS** | ICMP ping to Riot/Valve/Epic/Blizzard servers. DNS benchmark (Cloudflare/Google/Quad9). One-click fastest DNS apply via WMI. |
| **QoS Engine** | 5-second scan loop detects running games. Applies DSCP 46 (Expedited Forwarding) per-exe. Throttles Chrome/OneDrive/Windows Update via firewall rules + service stop. Auto-reverts when game exits. |
| **TCP Tweaks** | Disables Nagle (TCPNoDelay=1), delayed ACK (TcpAckFrequency=1). Locks TCP window to 64KB. Disables RFC 1323 timestamps. Full backup/revert. |
| **Wi-Fi Tweaks** | Roaming aggressiveness → lowest. Power saving → off. Interrupt moderation → off. 5 GHz preference. WMM enabled. MIMO all-antennas mode. Auto-tuning disabled. Adapter restart to apply. Full backup/revert. |

## Safety

- Every registry/adapter change is backed up before modification
- "Revert to defaults" restores exact original values
- UAC elevation is mandatory at launch
- QoS policies and firewall rules are cleaned up on exit

## Multi-language Windows (German, French, etc.)

The tweaks themselves work identically on every Windows language — registry values and
`netsh` commands are language-neutral. NetPulse reads current state from the registry and
from non-localized value tokens (enabled/disabled/numbers) rather than parsing translated
UI labels, so the "Current" and "Status" columns display correctly on any locale.

## "Not supported by this adapter"

Wi-Fi adapter tweaks use vendor-specific property names. Intel, Realtek, Qualcomm,
MediaTek and Broadcom each expose a different subset. NetPulse only writes properties your
driver actually advertises — anything your adapter doesn't support is logged as
"Not supported by this adapter" and skipped, never written as a dead key. This is expected
and harmless; the applicable tweaks still apply.

## Settings

A Settings page (last sidebar tab) lets you change:
- **Language** — English, German, French, Spanish. Auto-detected from Windows on first run, switches the interface instantly. Tweak descriptions stay in English on purpose (they contain registry names and netsh tokens you'd search for online).
- **Start QoS engine on launch**
- **Confirm before applying tweaks**
- **QoS scan interval** (1–60 seconds)

Settings persist to `%AppData%\NetPulseGamer\settings.json`.

## Building the installer

1. Run `BUILD.bat` to produce the exe.
2. Install [Inno Setup 6](https://jrsoftware.org/isdl.php) (free).
3. Either run `BUILD.bat installer`, or open `installer.iss` in the Inno Setup Compiler and press F9.
4. Output: `installer\NetPulseGamer-Setup.exe` — a proper multi-language installer with Start Menu shortcut, optional desktop icon, and clean uninstall.
