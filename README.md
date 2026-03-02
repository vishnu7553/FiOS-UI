# FRIDAY UI

**Graphical interface for the FRIDAY Orchestrator - Part of the FiOS ecosystem**

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [License](#license)

---

## Overview

FRIDAY UI is a GTK3-based graphical interface that connects to the FRIDAY Orchestrator. It provides a modern chat interface for natural language interactions, with a live terminal view for monitoring command execution. The UI manages the orchestrator lifecycle through built-in start/stop controls.

This component is part of the FiOS ecosystem:

| Component | Repository | Description |
|-----------|------------|-------------|
| **UI** | [fios-ui](https://github.com/vishnu7553/fios-ui) | ⬅️ You are here |
| **Orchestrator** | [fios-orchestrator](https://github.com/vishnu7553/fios-orchestrator) | Core command coordination |
| **Model Engine** | [friday-engine](https://huggingface.co/vishnu7553/friday-engine) | LLM inference with llama.cpp |
| **Meta Repository** | [FiOS](https://github.com/vishnu7553/FiOS) | Complete system integration |

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                       FRIDAY UI                             │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                    MAIN WINDOW                      │    │
│  │              (900x540, Fixed, Floating)             │    │
│  └─────────────────────┬───────────────────────────────┘    │
│                        │                                    │
│        ┌───────────────┴───────────────┐                    │
│        ▼                               ▼                    │
│  ┌─────────────┐                 ┌─────────────┐            │
│  │  CHAT TAB   │                 │ TERMINAL TAB│            │
│  │             │                 │             │            │
│  │ • Bubbles   │                 │ • tmux      │            │
│  │ • Messages  │                 │ • Live View │            │
│  │ • Entry     │                 │ • Output    │            │
│  └──────┬──────┘                 └──────┬──────┘            │
│         │                               │                   │
│         └───────────────┬───────────────┘                   │
│                         │                                   │
│         ┌───────────────▼───────────────┐                   │
│         │        HEADER CONTROLS        │                   │
│         │ • Status Dot                  │                   │
│         │ • Start/Stop Button           │                   │
│         │ • Service Indicator           │                   │
│         └───────────────┬───────────────┘                   │
│                         │                                   │
│         ┌───────────────▼────────────────┐                  │
│         │      SOCKET COMMUNICATION      │                  │
│         │   /tmp/friday_orchestrator.sock│                  │
│         └───────────────┬────────────────┘                  │
└─────────────────────────┼───────────────────────────────────┘
                          │
                          ▼
              ┌─────────────────────┐
              │ FRIDAY Orchestrator │
              │   (Go Service)      │
              └─────────────────────┘
```

---

## Features

- **Modern Chat Interface** – Message bubbles with sender labels (You on right, FRIDAY on left)
- **Live Terminal View** – Attaches directly to orchestrator's tmux session
- **Service Control** – Start/Stop orchestrator with one click
- **Status Indicator** – Green dot when running, red when stopped
- **Fixed Floating Window** – 900x540, stays in bottom-left corner
- **System Messages** – Centered notifications for connection status
- **Auto-scrolling** – Always shows latest messages
- **Escape to Quit** – Quick dismissal when needed

---

## Installation

### Prerequisites

- Python 3.8 or higher
- GTK 3.0 with GObject Introspection
- VTE 2.91 (Virtual Terminal Emulator)
- systemd (for service control)
- tmux 3.0 or higher
- FRIDAY Orchestrator (must be installed separately)

### Install Dependencies

#### Ubuntu/Debian
```bash
sudo apt update
sudo apt install python3 python3-gi python3-gi-cairo \
                 gir1.2-gtk-3.0 gir1.2-vte-2.91 \
                 tmux systemd
```

#### Arch Linux
```bash
sudo pacman -S python python-gobject gtk3 vte3 tmux
```

#### Fedora
```bash
sudo dnf install python3 python3-gobject gtk3 vte291 tmux
```

### Clone and Setup

```bash
# Clone the repository
git clone https://github.com/vishnu7553/fios-ui
cd fios-ui

# Make the script executable
chmod +x Friday
```

---

## Usage

### Launch the UI

```bash
./Friday
```

### Interface Guide

1. **Start the orchestrator** – Click the "Start" button in the header
2. **Send messages** – Type in the input field and press Enter
3. **View responses** – Messages appear in colored bubbles
4. **Monitor execution** – Switch to the "Terminal" tab to see live command output
5. **Stop the orchestrator** – Click "Stop" when done
6. **Close the app** – Press Escape key

### Visual Indicators

- 🟢 **Green dot** – Orchestrator is running and connected
- 🔴 **Red dot** – Orchestrator is stopped
- **Right bubbles** – Your messages
- **Left bubbles** – FRIDAY's responses
- **Centered text** – System notifications

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Enter` | Send message |
| `Escape` | Close application |

---

## Project Structure

```
fios-ui/
├── Friday             # Main application script
├── style.css          # GTK CSS styling
├── README.md          # This documentation
└── LICENSE            # MIT License
```

### Core Components

| File | Purpose |
|------|---------|
| `Friday.sh` | Main GTK application with chat UI, terminal, and socket communication |
| `style.css` | Visual styling including bubbles, colors, and window appearance |

---

## Troubleshooting

| Issue | Likely Cause | Solution |
|-------|--------------|----------|
| `Failed to connect` | Orchestrator not running | Click "Start" button in UI |
| Window won't open | Another instance running | Only one instance allowed |
| Terminal tab blank | tmux session missing | Start orchestrator first |
| No messages appearing | Socket disconnected | Restart orchestrator |
| CSS not loading | style.css missing | Ensure file exists in same directory |



---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

Copyright (c) 2026 @vishnu7553

---

*FRIDAY UI is a graphical frontend for the FiOS platform. For the complete system, visit the [FiOS meta-repository](https://github.com/vishnu7553/FiOS).*
