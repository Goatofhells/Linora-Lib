# Linora-Lib
The roblox lib, Linora with documentation! 
## Please star it if you enjoy all the documentation i make for random ui libs
> [!TIP]
> I recommend using my [gitbook](https://stratxgy.gitbook.io/linora-lib), as in my opinion it's better than this.

> [!IMPORTANT]  
> This is not mine. Find the real github [here](https://github.com/violin-suzutsuki/LinoriaLib)

---

## Importing the library
```lua
local Library = loadstring(game:HttpGet('https://raw.githubusercontent.com/Goatofhells/G/refs/heads/main/Gui_Lib_fixed.txt'))()
local ThemeManager = loadstring(game:HttpGet('https://raw.githubusercontent.com/Goatofhells/Linoria-Library-Mobile/refs/heads/main/Gui%20Lib%20%5BThemeManager%5D'))()
local SaveManager = loadstring(game:HttpGet('https://raw.githubusercontent.com/violin-suzutsuki/LinoriaLib/main/addons/SaveManager.lua'))()
```

---

## Creating a window
```lua
local Window = Library:CreateWindow({
    Title = 'Example menu',
    Center = true,       -- centers the window on screen
    AutoShow = true,     -- shows the window immediately on creation
    TabPadding = 8,
    MenuFadeTime = 0.2,
    Size = Vector2.new(600, 400), -- minimum size of the window
})
```

---

## Creating tabs
```lua
local Tab = Window:AddTab('Main')
```

---

## Creating groupboxes
Groupboxes are containers that sit inside a tab. Each tab has a left and right column.
```lua
local LeftGroupBox  = Tab:AddLeftGroupbox('My Group')
local RightGroupBox = Tab:AddRightGroupbox('My Group')

-- Or with full options:
local GroupBox = Tab:AddGroupbox({
    Name = 'My Group',
    Side = 1,  -- 1 = left, 2 = right
})
```

---

## Creating tabboxes
Tabboxes are groupboxes with their own inner tabs.
```lua
local LeftTabBox  = Tab:AddLeftTabbox('My Tabbox')
local RightTabBox = Tab:AddRightTabbox('My Tabbox')

local InnerTab = LeftTabBox:AddTab('Inner Tab')
-- InnerTab supports all the same elements as a groupbox
```

---

# Elements

## Toggle
```lua
local MyToggle = LeftGroupBox:AddToggle('MyToggle', {
    Text = 'This is a toggle',
    Default = false,          -- default value (true / false)
    Tooltip = 'A tooltip',    -- shown on hover
    Risky = false,            -- marks label in red (for dangerous options)

    Callback = function(Value)
        print('[cb] MyToggle changed to:', Value)
    end
})

-- Methods
MyToggle:SetValue(true)
MyToggle:OnChanged(function(Value)
    print('Toggle changed:', Value)
end)

-- Reading value
print(Toggles.MyToggle.Value)
```

---

## Button
```lua
local MyButton = LeftGroupBox:AddButton({
    Text = 'Button',
    Func = function()
        print('Button clicked!')
    end,
    DoubleClick = false,   -- require double click to fire
    Tooltip = 'A tooltip',
})

-- Chaining a sub-button underneath
MyButton:AddButton({
    Text = 'Sub Button',
    Func = function()
        print('Sub button clicked!')
    end,
})
```

---

## Slider
```lua
local MySlider = LeftGroupBox:AddSlider('MySlider', {
    Text = 'This is my slider!',
    Default = 0,
    Min = 0,
    Max = 5,
    Rounding = 1,      -- decimal places (0 = integer)
    Compact = false,   -- hides the text label above, shows value inline
    HideMax = false,   -- hides the max value in the display
    Suffix = '',       -- string appended to the displayed value e.g. '%'
    Tooltip = 'A tooltip',
    BlankSize = 6,     -- spacing after element

    Callback = function(Value)
        print('[cb] MySlider changed:', Value)
    end
})

-- Methods
MySlider:SetValue(3)
MySlider:SetMin(1)
MySlider:SetMax(10)
MySlider:OnChanged(function(Value)
    print('Slider changed:', Value)
end)

-- Reading value
print(Options.MySlider.Value)
```

---

## Dropdown
```lua
local MyDropdown = LeftGroupBox:AddDropdown('MyDropdown', {
    Values = { 'This', 'is', 'a', 'dropdown' },
    Default = 1,           -- index or string value
    Multi = false,         -- allow multiple selections
    Text = 'A dropdown',
    Tooltip = 'A tooltip',
    AllowNull = false,     -- allow no selection
    BlankSize = 5,

    Callback = function(Value)
        print('[cb] Dropdown changed:', Value)
    end
})

-- Methods
MyDropdown:SetValue('This')
MyDropdown:SetValues({ 'New', 'Values' })
MyDropdown:OnChanged(function(Value)
    print('Dropdown changed:', Value)
end)

-- Reading value
print(Options.MyDropdown.Value)
```

---

## Color Picker
Color pickers are chained onto a label.
```lua
LeftGroupBox:AddLabel('Color'):AddColorPicker('ColorPicker', {
    Default = Color3.new(1, 1, 1),
    Title = 'Some color',   -- optional title shown in the picker popup
    Transparency = 0,       -- enables transparency slider (nil to disable)

    Callback = function(Value)
        print('[cb] Color changed!', Value)
    end
})

-- Methods
Options.ColorPicker:SetValueRGB(Color3.fromRGB(255, 0, 128))
Options.ColorPicker:OnChanged(function(Value)
    print('Color changed:', Value)
end)

-- Reading value
print(Options.ColorPicker.Value)         -- Color3
print(Options.ColorPicker.Transparency) -- number
```

---

## Keybind
Keybinds are chained onto a label.
```lua
LeftGroupBox:AddLabel('Keybind'):AddKeyPicker('KeyPicker', {
    Default = 'MB2',          -- key name or 'MB1' / 'MB2' for mouse buttons
    Text = 'Example Keybind', -- label shown in the keybind menu
    NoUI = false,             -- set true to hide from the keybind menu
    SyncToggleState = false,  -- syncs with a parent Toggle's state
    Mode = 'Toggle',          -- 'Always', 'Toggle', or 'Hold'
    Modes = { 'Always', 'Toggle', 'Hold' }, -- which modes to show in the picker

    -- fires when the assigned key is changed
    ChangedCallback = function(New)
        print('[cb] Keybind changed!', New)
    end,

    Callback = function(Value)
        print('[cb] Keybind fired:', Value)
    end
})

-- Methods
Options.KeyPicker:SetValue({ 'LeftShift', 'Toggle' })
Options.KeyPicker:OnChanged(function(Value)
    print('Keybind changed:', Value)
end)

-- Check if currently active
print(Options.KeyPicker:GetKeybindActive()) -- boolean
```

---

## Input (Text Box)
```lua
local MyInput = LeftGroupBox:AddInput('MyInput', {
    Text = 'Label above the box',
    Default = '',              -- default text value
    Placeholder = 'Type here', -- placeholder text
    Numeric = false,           -- only allow numbers
    Finished = false,          -- only fire callback on Enter, not every keystroke
    MaxLength = 100,           -- max character length (optional)
    ClearTextOnFocus = true,   -- clears box when focused
    Tooltip = 'A tooltip',

    Callback = function(Value)
        print('[cb] Input changed:', Value)
    end
})

-- Methods
MyInput:SetValue('hello')
MyInput:OnChanged(function(Value)
    print('Input changed:', Value)
end)

-- Reading value
print(Options.MyInput.Value)
```

---

## Label
```lua
local MyLabel = LeftGroupBox:AddLabel('This is a label', false)
-- second arg: doesWrap (boolean, optional) — wraps long text

-- Methods
MyLabel:SetText('Updated label')
```

---

## Divider
```lua
LeftGroupBox:AddDivider()
```

---

## Blank (Spacer)
```lua
LeftGroupBox:AddBlank(10)  -- adds N pixels of vertical spacing
```

---

## Dependency Box
A dependency box is a container whose visibility is tied to the value of other elements. When the dependencies are not met it collapses completely.
```lua
local DepBox = LeftGroupBox:AddDependencyBox()

-- Add elements to it like any groupbox
DepBox:AddToggle('InnerToggle', { Text = 'Only visible when deps met' })

-- Set which elements + values must match for it to be visible
DepBox:SetupDependencies({
    { Toggles.MyToggle, true },           -- MyToggle must be true
    { Options.MyDropdown, 'SomeValue' },  -- MyDropdown must equal 'SomeValue'
})
```

---

## Notification
```lua
Library:Notify('This is a notification')
Library:Notify('Custom duration', 3)         -- 3 seconds
Library:Notify('With sound', 5, 131961136)   -- text, time, SoundId
```

---

## Toggle the UI
```lua
Library:Toggle()  -- shows/hides the window
```

---

## Unload the library
```lua
Library:Unload()
-- Disconnects all signals, calls Library.OnUnload if set, destroys the ScreenGui

Library:OnUnload(function()
    print('Library unloaded!')
end)
```

---

## ThemeManager
```lua
ThemeManager:SetLibrary(Library)
ThemeManager:SetFolder('MyHub')

-- Attach to a tab
ThemeManager:ApplyToTab(Tab)

-- Or attach to an existing groupbox
ThemeManager:ApplyToGroupbox(LeftGroupBox)
```

---

## SaveManager
```lua
SaveManager:SetLibrary(Library)
SaveManager:SetFolder('MyHub')

-- Attach to a groupbox (not a tab)
local SaveGroupBox = Tab:AddLeftGroupbox('Configs')
SaveManager:BuildConfigSection(SaveGroupBox)

-- Load default config on startup
SaveManager:LoadAutoloadConfig()
```

---

## Full Example Script
```lua
local Library = loadstring(game:HttpGet('https://raw.githubusercontent.com/Goatofhells/G/refs/heads/main/Gui_Lib_fixed.txt'))()
local ThemeManager = loadstring(game:HttpGet('https://raw.githubusercontent.com/Goatofhells/Linoria-Library-Mobile/refs/heads/main/Gui%20Lib%20%5BThemeManager%5D'))()
local SaveManager = loadstring(game:HttpGet('https://raw.githubusercontent.com/Mc4121ban/Linoria-Library-Mobile/refs/heads/main/Gui%20Lib%20%5BSaveManager%5D'))()

local Window = Library:CreateWindow({
    Title = 'My Hub',
    Center = true,
    AutoShow = true,
    TabPadding = 8,
    MenuFadeTime = 0.2,
    Size = Vector2.new(500, 300),
})

local Tabs = {
    Main = Window:AddTab('Main'),
    Settings = Window:AddTab('Settings'),
}

local LeftGroupBox  = Tabs.Main:AddLeftGroupbox('Main')
local RightGroupBox = Tabs.Main:AddRightGroupbox('Misc')

-- Toggle
LeftGroupBox:AddToggle('MyToggle', {
    Text = 'Enable Feature',
    Default = false,
    Callback = function(Value)
        print('Toggle:', Value)
    end
})

-- Slider
LeftGroupBox:AddSlider('MySlider', {
    Text = 'Speed',
    Default = 16,
    Min = 0,
    Max = 100,
    Rounding = 0,
    Suffix = ' studs/s',
    Callback = function(Value)
        print('Slider:', Value)
    end
})

-- Dropdown
LeftGroupBox:AddDropdown('MyDropdown', {
    Text = 'Mode',
    Values = { 'Silent Aim', 'Aimbot', 'Off' },
    Default = 3,
    Callback = function(Value)
        print('Dropdown:', Value)
    end
})

-- Button
LeftGroupBox:AddButton({
    Text = 'Do Something',
    Func = function()
        print('Button clicked!')
    end,
    DoubleClick = false,
})

-- Color Picker
RightGroupBox:AddLabel('Chams Color'):AddColorPicker('ChamColor', {
    Default = Color3.fromRGB(255, 100, 100),
    Callback = function(Value)
        print('Color:', Value)
    end
})

-- Keybind
RightGroupBox:AddLabel('Toggle Key'):AddKeyPicker('MyKeybind', {
    Default = 'MB2',
    Text = 'Activate',
    Mode = 'Hold',
    Callback = function(Value)
        print('Keybind fired:', Value)
    end
})

-- Input
RightGroupBox:AddInput('MyInput', {
    Text = 'Player Name',
    Placeholder = 'Enter name...',
    Finished = true,
    Callback = function(Value)
        print('Input:', Value)
    end
})

-- Dependency Box (only visible when MyToggle is on)
local DepBox = LeftGroupBox:AddDependencyBox()
DepBox:AddSlider('DepSlider', {
    Text = 'Extra Setting',
    Default = 5,
    Min = 0,
    Max = 10,
    Rounding = 0,
})
DepBox:SetupDependencies({
    { Toggles.MyToggle, true },
})

-- Settings tab
local ThemeGroupBox = Tabs.Settings:AddLeftGroupbox('Themes')
local SaveGroupBox  = Tabs.Settings:AddRightGroupbox('Configs')

ThemeManager:SetLibrary(Library)
ThemeManager:SetFolder('MyHub')
ThemeManager:ApplyToGroupbox(ThemeGroupBox)

SaveManager:SetLibrary(Library)
SaveManager:SetFolder('MyHub')
SaveManager:BuildConfigSection(SaveGroupBox)
SaveManager:LoadAutoloadConfig()
```
