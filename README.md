# ✈️ aeronauticauilib

> A sleek, modern Roblox UI library with an **animated neon glow**, **RGB gradients**, a built‑in **config system**, and **multi‑select dropdowns**.
> Built on top of the Mercury UI framework and heavily enhanced.

<p align="center">
  <img alt="lua" src="https://img.shields.io/badge/Language-Luau-000?style=flat-square&logo=lua">
  <img alt="platform" src="https://img.shields.io/badge/Platform-Roblox-00A2FF?style=flat-square">
  <img alt="status" src="https://img.shields.io/badge/Status-Active-8A2BE2?style=flat-square">
</p>

---

## ✨ Features

- 🟣 **Animated neon border + glow** around the window (rounded, iridescent purple by default)
- 🌈 **Flowing RGB gradients** on text and accents
- 🎛️ Full component set: toggles, buttons, sliders, dropdowns, textboxes, keybinds, color pickers
- ✅ **Multi‑select dropdowns** (new!)
- 🧊 **Liquid Glass mode** *(new!)* — an iOS‑style frosted‑glass skin with pill toggles, switchable live from Settings (**UI Style → Liquid Glass**)
- 🪟 **Appearance controls** *(new!)* — window **opacity**, **cursor particles**, window **size**, **element size** & **layout mode** (columns), plus **Reset Appearance**, all in the Settings tab
- 📝 **Auto‑wrapping text** *(new!)* — long descriptions wrap to new lines and grow the row; names never overlap the controls
- 💾 **Config system** — save / load / delete / rewrite + **autoload** on startup
- 🎨 Live **color customization** for window / buttons / tabs (built‑in Settings tab)
- 🔔 Clean notifications & prompts
- 🖱️ Draggable window, toggle keybind, themes

---

## 🚀 Getting Started

Load the library with a single line:

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/aeronauticafan4242/aeronauticauilib/refs/heads/main/src.lua"))()
```

> 💡 Method names are **case‑insensitive** — `Library:Create` and `Library:create` both work.

---

## 🪟 Creating a Window

```lua
local gui = Library:Create{
    Name  = "My Script",                 -- window title
    Size  = UDim2.fromOffset(600, 400),  -- window size
    Theme = Library.Themes.Dark,         -- see Themes below
    Link  = "https://github.com/you/repo"-- shown in the fake URL bar
}
```

Every window automatically gets a **Settings** tab (toggle keybind, drag speed, **neon color pickers**, **config manager**) and a **Credits** tab.

### 🎨 Themes
```lua
Library.Themes.Dark
Library.Themes.Serika
-- ...and more in Library.Themes
```

---

## 🗂️ Tabs

```lua
local tab = gui:Tab{
    Name = "Main",
    Icon = "rbxassetid://8569322835"
}
```

---

## 🧩 Components

All components are created on a **tab**.

### 🔘 Toggle
```lua
tab:Toggle{
    Name = "God Mode",
    StartingState = false,
    Description = "Makes you invincible",
    Callback = function(state)
        print("Toggled:", state)
    end
}
```

### 🟪 Button
```lua
tab:Button{
    Name = "Kill All",
    Description = "Removes every enemy",
    Callback = function()
        print("clicked!")
    end
}
```

### 🎚️ Slider
```lua
tab:Slider{
    Name = "Walk Speed",
    Min = 16,
    Max = 200,
    Default = 16,
    Callback = function(value)
        print("Speed:", value)
    end
}
```

### 📋 Dropdown
```lua
tab:Dropdown{
    Name = "Target Part",
    StartingText = "Select...",
    Description = "Where to aim",
    Items = { "Head", "Torso", "Random" },
    Callback = function(item)
        print("Picked:", item)
    end
}
```

Items can also be **label / value** pairs:
```lua
Items = { {"One", 1}, {"Two", 2}, {"Three", 3} }
-- Callback receives the value (1, 2, 3)
```

### ✅ Multi‑Select Dropdown *(new)*
Set `MultiSelect = true`. Selected items are marked with a ✔ and the list stays open.
The callback receives an **array** of all currently selected values.

```lua
local dd = tab:Dropdown{
    Name = "Notify Parts",
    StartingText = "Select parts...",
    Description = "Choose multiple",
    MultiSelect = true,
    Items = { "Wright Flyer", "Sukhoi KR-860", "Bell X-1" },
    Callback = function(selected)
        -- selected = { "Wright Flyer", "Bell X-1" }
        print("Selected count:", #selected)
    end
}

-- read the current selection any time:
local chosen = dd:GetSelected()
```

### ⌨️ Textbox
```lua
tab:Textbox{
    Name = "Webhook URL",
    Description = "Paste a link",
    Callback = function(text)
        print("Entered:", text)
    end
}
```

### 🎹 Keybind
```lua
tab:Keybind{
    Name = "Aimbot Key",
    Description = "Hold to aim",
    Keybind = Enum.KeyCode.E,
    Callback = function()
        print("key pressed")
    end
}
```

### 🎨 Color Picker
```lua
tab:ColorPicker{
    Name = "ESP Color",
    Description = "Click to adjust",
    Style = Library.ColorPickerStyles.Legacy,
    Callback = function(color)
        print(color) -- Color3
    end
}
```

---

## 🔔 Notifications & Prompts

```lua
gui:Notification{
    Title = "Success",
    Text = "Everything works!",
    Duration = 3
}

gui:set_status("Farming...")  -- bottom-left status text
```

Chainable prompt (dialog):
```lua
tab:Prompt{
    Title = "Are you sure?",
    Text = "This cannot be undone.",
    Buttons = {
        Yes = function() print("yes") end,
        No  = function() print("no") end
    }
}
```

---

## 🟣 Neon Customization (API)

The window/button/tab glow colors can be changed live. The Settings tab exposes color pickers for these automatically, but you can also set them from code:

```lua
Library:setWindowNeon(Color3.fromRGB(255, 0, 128)) -- window border & glow
Library:setButtonNeon(Color3.fromRGB(0, 255, 170))  -- buttons, sliders, dropdowns…
Library:setTabNeon(Color3.fromRGB(120, 200, 255))   -- tab buttons
```

---

## 🪟 Appearance & Layout *(new)*

The **Settings** tab now includes four live controls so every user can tailor the UI to their screen and taste. All four are **saved with configs** (and autoload), so a user sets them once and they stick.

| Control | What it does | Default |
|---|---|---|
| **UI Style** | **Legacy** (classic), **Liquid Glass** (iOS‑26 frosted glass: translucent panels, light gradient, specular rim, rounded corners, green iOS pill toggles), or **Liquid Glass V2** (the *clear* iPhone look — **white see‑through frosted toggles**, more transparent panels, and a fully‑rounded transparent "Welcome" card). Switches live. | `Legacy` |
| **Window Opacity** | Transparency of the whole window (100 = solid glass). | `80` (slightly translucent) |
| **Particles** | Little particles that follow your cursor over the window. | `off` |
| **Particle Type** | Style of the cursor particles: `Sparks`, `Neon Trail`, `Bubbles`, `Stars`. | `Sparks` |
| **Window Size** | Overall UI scale — shrink the entire window for laptops / small screens. | `100%` (current size) |
| **Element Size** | Size of every element (buttons, sliders, dropdowns…), independent of window size. | `100%` |
| **Layout Mode** | Arrange elements as a **Vertical (List)**, **2 Columns**, or **3 Columns** — so a single button no longer has to hog a whole row. | `Vertical (List)` |
| **Reset Appearance** | One button to reset all of the above back to defaults. | — |

These are **non‑destructive and independent**: opacity only touches the window fill, window size is a `UIScale` on the window, element size is a per‑component `UIScale`, layout mode reflows each tab, and particles live on their own click‑through overlay. At their defaults the UI looks **exactly** like before — nothing in existing scripts changes unless the user opts in.

**Long descriptions now wrap** onto multiple lines and grow the element's height instead of running off the window, and element names truncate cleanly so text never overlaps toggles/boxes/icons.

You can also drive them from code:

```lua
Library:setWindowOpacity(80)          -- 0..100 (100 = solid)
Library:setWindowScale(90)            -- percent; ~60..115 recommended
Library:setElementScale(110)          -- percent; ~80..130 recommended
Library:setLayoutMode("2 Columns")    -- "Vertical (List)" | "2 Columns" | "3 Columns"
Library:setParticlesEnabled(true)     -- cursor particles on/off
Library:setParticleType("Neon Trail") -- "Sparks" | "Neon Trail" | "Bubbles" | "Stars"
Library:setUIStyle("Liquid Glass V2") -- "Legacy" | "Liquid Glass" | "Liquid Glass V2"
```

> 💡 Columns work with every component — even dropdowns, which resize themselves when opened, keep their column width. The theme picker is kept full‑width automatically so its previews never get squeezed.

---

## 💾 Config System

Save & restore your setup. Managed visually from the **Settings** tab
(**Config Name** box → **Save / Rewrite**, then **Load / Delete / Set as Autoload / Clear Autoload**),
or programmatically:

```lua
Library:saveConfig("MyTheme")   -- save/overwrite current setup
Library:loadConfig("MyTheme")   -- apply a saved config
Library:deleteConfig("MyTheme") -- remove it
Library:listConfigs()           -- -> { "MyTheme", "Neon", ... }

Library:setAutoload("MyTheme")  -- auto-apply this config on every launch
Library:getAutoload()           -- -> "MyTheme" or nil
```

### What gets saved

Every config **always** stores:
- 🎨 The selected **theme**
- 🟣 **Neon colors** (window / buttons / tabs)
- ⚙️ Core **menu settings**: `Toggle Key`, `Lock Dragging`, `UI Drag Speed`
- 🪟 **Appearance**: `Window Opacity`, `Particles`, `Particle Type`, `Window Size`, `Element Size`, `Layout Mode`

### 🗃️ Save all settings

Enable the **`Save all settings`** toggle (in the Settings tab, **off by default**) to also persist
**every component's state across your whole UI** — toggles, sliders, textboxes, keybinds and
**multi‑select dropdowns** included. Perfect for hubs where users don't want to reconfigure each launch.

> - Component states are keyed by their **Name**, so give components unique names.
> - On **autoload / load**, saved states are re‑applied even to components that are created *after* the
>   window (your tabs are built after `Create`) — the library re‑applies them as each component appears.
> - Configs are stored as JSON in the executor's `GrindnauticaConfigs/` folder.
> - **Autoload** applies your chosen config automatically the moment the UI is created — set it once and forget it.

---

## 📝 Full Example

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/aeronauticafan4242/aeronauticauilib/refs/heads/main/src.lua"))()

local gui = Library:Create{ Name = "Demo Hub", Theme = Library.Themes.Dark }
local main = gui:Tab{ Name = "Main", Icon = "rbxassetid://8569322835" }

main:Toggle{ Name = "Auto Farm", Callback = function(on) print("farm:", on) end }

main:Slider{ Name = "Speed", Min = 16, Max = 100, Default = 16,
    Callback = function(v) print("speed", v) end }

main:Dropdown{ Name = "Modes", MultiSelect = true,
    Items = { "A", "B", "C" },
    Callback = function(sel) print("selected", #sel) end }

gui:Notification{ Title = "Loaded", Text = "Demo Hub is ready!", Duration = 4 }
```

---

## 🙏 Credits

- Original **Mercury** UI framework by **Abstract** & **Deity**
- Neon glow, RGB gradients, multi‑select, config system & other enhancements by **Tot Kto Iz Niotkuda Xploits** ([YouTube](https://www.youtube.com/@corrective))

---

<p align="center"><i>Made with 💜 for the Aeronautica community.</i></p>
