# FilePilot Custom Configuration (v0.8.1)

Optimized and permanent default folder view configuration for [FilePilot](https://filepilot.tech) v0.8.1 by Voidstar.

---

##  Overview

By default, FilePilot caches per-folder view configurations and session states, which can make it tricky to enforce a consistent folder view across all directories and new tabs. 

This repository provides pre-configured, tested configuration files (`FPilot-Config.json` and `FPilot-UserData.json`) that permanently configure FilePilot with:

1. **Icons at 65% Zoom**: Balanced icon display size for clean previews.
2. **Grouped by Date Created (Descending)**: Items grouped logically by creation date with recent files at the top.
3. **Sorted by Date Modified (Descending)**: Files inside each group sorted chronologically by most recently modified.
4. **Folders First**: Keeps directories grouped neatly before standalone files.
5. **Universal Default Inheritance**: New tabs and application launches automatically open in the customized default folder view.

---

##  Repository Files

| File | Target Location | Description |
| :--- | :--- | :--- |
| **`FPilot-Config.json`** | `%APPDATA%\Voidstar\FilePilot\` | Application options, visual styling, behaviors (`LoadOnStartup`, `NewTabShow`), and custom keybindings. |
| **`FPilot-UserData.json`** | `%APPDATA%\Voidstar\FilePilot\` | State data, `DefaultFolder` layout definition, column split ratios, context menu bookmarks, and command history. |

---

##  Technical Schema Details

FilePilot v0.8.1 parses folder views using specific JSON properties and enums:

- **Zoom Level**: `"ZoomLevel": 0.650000` (along with the legacy/fallback blank key `"": 0.650000`).
- **Grouping**: Structured descriptor object:
  ```json
  "GroupedBy": {
    "Kind": "DateCreated",
    "Descending": true
  }
  ```
- **Sorting Enum Convention (`SortedBy`)**:
  - Column Kinds: `0` (Name), `1` (Size), `2` (Type), `3` (Date Modified), `5` (Date Created).
  - Ascending sorts use the base column index (`3` for Date Modified Ascending).
  - Descending sorts use an internal offset of `+7`:
    $$\text{SortedBy} = 3 + 7 = 10 \quad (\text{Date Modified, Descending})$$
- **Folder Overrides**:
  - The `"Folders": []` list in `FPilot-UserData.json` is cleared to prevent previously cached folder views from overriding the global `DefaultFolder` settings.
- **Default Tab & Startup Behavior**:
  - In `FPilot-Config.json`:
    - `"LoadOnStartup": "Default"`
    - `"NewTabShow": "Default"`

---

##  Installation & Setup

### Prerequisites
Close FilePilot completely before applying these files to prevent the in-memory state from overwriting the JSON files on shutdown.

### Step 1: Terminate Running FilePilot Instances
Run via PowerShell or Command Prompt:
```powershell
taskkill /F /IM FPilot.exe
```

### Step 2: Copy Configuration Files
Copy both JSON files into your FilePilot roaming AppData directory:

```powershell
# Path: %APPDATA%\Voidstar\FilePilot
Copy-Item "FPilot-Config.json" "$env:APPDATA\Voidstar\FilePilot\" -Force
Copy-Item "FPilot-UserData.json" "$env:APPDATA\Voidstar\FilePilot\" -Force
```

### Step 3: Remove Read-Only Attributes (if applicable)
```powershell
attrib -R "$env:APPDATA\Voidstar\FilePilot\*.json"
```

### Step 4: Launch FilePilot
Open FilePilot. Every directory, panel, and new tab will now open with 65% icon zoom, grouped by Date Created (Descending), and sorted by Date Modified (Descending).

---
