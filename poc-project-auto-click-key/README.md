# AutoClickKey - Data Workflow Diagram

A comprehensive functional and data workflow diagram for the AutoClickKey Windows automation application.

## Source Repository

**GitHub:** [https://github.com/MrParkerZ7/app-auto-key-click-x-claude](https://github.com/MrParkerZ7/app-auto-key-click-x-claude)

## Application Overview

**AutoClickKey** is a Windows automation tool built with C# WPF (.NET 8.0) that enables users to automate mouse clicking, keyboard input, and create complex automation workflows.

### Tech Stack

- **Framework:** .NET 8.0
- **UI:** WPF (Windows Presentation Foundation)
- **Architecture:** MVVM (Model-View-ViewModel)
- **Windows API:** user32.dll (SendInput, RegisterHotKey)

### Key Features

- Auto Clicker (left/right/middle mouse buttons, single/double click)
- Auto Keyboard (text typing, key combinations)
- Recording and Playback
- Profile Management (save/load/export/import)
- Workspace Management (organize multiple profiles into jobs)
- Global Hotkeys (F6: start/stop, F8: emergency stop)

## Diagram Contents

The diagram shows the complete data flow from user input to system execution:

| Layer | Description |
|-------|-------------|
| **UI Layer** | WPF XAML components (MainWindow, tabs, editors, controls) |
| **ViewModel Layer** | MVVM state management (MainViewModel, WorkspaceViewModel) |
| **Model Layer** | Data classes (ActionItem, Profile, Job, Workspace, AppSettings) |
| **Service Layer** | Business logic (Persistence, Execution, Utility services) |
| **Persistence Layer** | File storage (%APPDATA%\AutoClickKey\*.json) |
| **Win32 API Layer** | P/Invoke (SendInput, RegisterHotKey, GetCursorPos) |
| **Execution Flow** | Runtime flowchart (Load → Loop → Execute → Win32) |

## Data Model Hierarchy

```
AppSettings
├── LastProfileName
├── LastWorkspaceName
└── LastHotkey

Workspace
└── Jobs (1:N)
    └── Job
        └── ProfileNames (N:M reference)
            └── Profile
                └── Actions (1:N)
                    └── ActionItem
                        ├── Type: Click | KeyPress | Delay
                        ├── MouseButton, ClickType, Position
                        └── Key, Modifiers
```

## Storage Locations

| Data | Location |
|------|----------|
| Profiles | `%APPDATA%\AutoClickKey\Profiles\*.json` |
| Workspaces | `%APPDATA%\AutoClickKey\Workspaces\*.json` |
| Settings | `%APPDATA%\AutoClickKey\settings.json` |

## Files

| File | Description |
|------|-------------|
| [data-workflow-diagram.drawio](./data-workflow-diagram.drawio) | DrawIO source file |

## Diagram Standards

This diagram follows the guidelines defined in [CLAUDE-DIAGRAMS-STANDARD-FORMAT.md](../CLAUDE-DIAGRAMS-STANDARD-FORMAT.md):

- Swimlane format for data models with property details
- Protocol-based arrow colors (gray: data flow, green: storage, red: execution)
- Shadow on all elements
- Hierarchical layer organization
- Legend with color coding
