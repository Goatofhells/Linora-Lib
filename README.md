# Linoria-Lib
The Roblox UI library Linoria, with full documentation.

> [!TIP]
> I recommend using my [gitbook](https://stratxgy.gitbook.io/linora-lib) as it may be easier to read.

> [!IMPORTANT]
> This library is not mine. Find the original github [here](https://github.com/violin-suzutsuki/LinoriaLib)

---

## Importing

Load the library, ThemeManager and SaveManager at the top of your script.

```lua
local Library = loadstring(game:HttpGet('https://raw.githubusercontent.com/Goatofhells/G/refs/heads/main/Gui_Lib_fixed.txt'))()
local ThemeManager = loadstring(game:HttpGet('https://raw.githubusercontent.com/Goatofhells/Linoria-Library-Mobile/refs/heads/main/Gui%20Lib%20%5BThemeManager%5D'))()
local SaveManager = loadstring(game:HttpGet('https://raw.githubusercontent.com/Mc4121ban/Linoria-Library-Mobile/refs/heads/main/Gui%20Lib%20%5BSaveManager%5D'))()
```

---

## Window

Creates the main window. `Title` is the text shown at the top. `Center` places the window in the middle of the screen. `AutoShow` makes it visible immediately on creation. `TabPadding` controls the spacing between tab buttons. `MenuFadeTime` is how long the open/close fade animation takes in seconds. `Size` is required and sets the window dimensions.

```lua
local Window = Library:CreateWindow({
    Title = 'My Hub',
    Center = true,
    AutoShow = true,
    TabPadding = 8,
    MenuFadeTime = 0.2,
    Size = Vector2.new(500, 300),
})
```

---

## Tabs

Tabs are the top-level pages inside your window. Each tab holds groupboxes on the left and right side.

```lua
local Tabs = {
    Main = Window:AddTab('Main'),
    ['UI Settings'] = Window:AddTab('UI Settings'),
}
```

---

## Groupboxes

Groupboxes are containers that sit inside a tab. Use `AddLeftGroupbox` or `AddRightGroupbox` to place them on either side. The string you pass in is the title shown at the top of the box.

```lua
local LeftGroupBox  = Tabs.Main:AddLeftGroupbox('Features')
local RightGroupBox = Tabs.Main:AddRightGroupbox('Misc')
```

---

## Tabboxes

Tabboxes work like groupboxes but have their own inner tabs, useful for organising many elements in a small space. Everything that can be added to a groupbox can also be added to a tabbox tab.

```lua
local LeftTabBox  = Tabs.Main:AddLeftTabbox()
local RightTabBox = Tabs.Main:AddRightTabbox()

local Tab1 = LeftTabBox:AddTab('Tab 1')
local Tab2 = LeftTabBox:AddTab('Tab 2')
```

---

## Elements

### Toggle

Adds an on/off toggle. `Text` is the label shown next to it. `Default` is the starting state. `Tooltip` shows a small popup on hover. Setting `Risky` to true colours the label red to warn users it's a dangerous option. Access the current state at any time with `Toggles.MyToggle.Value`. Using `OnChanged` is the recommended way to react to changes rather than `Callback`.

```lua
LeftGroupBox:AddToggle('MyToggle', {
    Text = 'Enable Feature',
    Default = false,
    Tooltip = 'Turns the feature on or off',
    Risky = false,
})

Toggles.MyToggle:OnChanged(function()
    print(Toggles.MyToggle.Value)
end)

Toggles.MyToggle:SetValue(true)
```

---

### Button

Adds a clickable button. `Text` is the label. `Func` is the function that fires on click. Setting `DoubleClick` to true requires two clicks to trigger it. You can chain `AddButton` on an existing button to add a sub-button directly beneath it.

```lua
local MyButton = LeftGroupBox:AddButton({
    Text = 'Do Something',
    Func = function()
        print('clicked!')
    end,
    DoubleClick = false,
    Tooltip = 'Click me',
})

MyButton:AddButton({
    Text = 'Sub Button',
    Func = function()
        print('sub clicked!')
    end,
    DoubleClick = true,
})
```

---

### Slider

Adds a draggable slider. `Text`, `Default`, `Min`, `Max` and `Rounding` are all required. `Rounding` is the number of decimal places — use `0` for whole numbers. `Suffix` is a string appended to the displayed value such as `%` or ` studs/s`. Setting `Compact` to true hides the title label and shows the value inline instead. `HideMax` only shows the current value without the max. Access the value with `Options.MySlider.Value`.

```lua
LeftGroupBox:AddSlider('MySlider', {
    Text = 'Speed',
    Default = 16,
    Min = 0,
    Max = 100,
    Rounding = 0,
    Suffix = ' studs/s',
    Compact = false,
    HideMax = false,
})

Options.MySlider:OnChanged(function()
    print(Options.MySlider.Value)
end)

Options.MySlider:SetValue(50)
Options.MySlider:SetMin(10)
Options.MySlider:SetMax(200)
```

---

### Dropdown

Adds a dropdown menu. `Values` is the list of options. `Default` is either a number index or a string matching one of the values. Setting `Multi` to true allows multiple selections — in that case `Value` will be a table of `{ option = true }` pairs. `AllowNull` allows the dropdown to have no selection. Access the value with `Options.MyDropdown.Value`.

```lua
LeftGroupBox:AddDropdown('MyDropdown', {
    Text = 'Mode',
    Values = { 'Option A', 'Option B', 'Option C' },
    Default = 1,
    Multi = false,
    AllowNull = false,
    Tooltip = 'Pick a mode',
})

Options.MyDropdown:OnChanged(function()
    print(Options.MyDropdown.Value)
end)

Options.MyDropdown:SetValue('Option A')
Options.MyDropdown:SetValues({ 'New', 'List' })
```

Using `SpecialType = 'Player'` creates a dropdown that automatically lists all players in the server and updates as players join or leave.

```lua
LeftGroupBox:AddDropdown('PlayerDropdown', {
    SpecialType = 'Player',
    Text = 'Select Player',
})
```

---

### Color Picker

Color pickers are chained onto a label with `:AddColorPicker`. `Default` is the starting `Color3` value. `Title` is shown at the top of the picker popup. Setting `Transparency` to a number enables a transparency slider below the color picker — leave it as `nil` to disable. Access the color with `Options.ColorPicker.Value` and the transparency with `Options.ColorPicker.Transparency`.

```lua
LeftGroupBox:AddLabel('Chams Color'):AddColorPicker('ColorPicker', {
    Default = Color3.fromRGB(255, 100, 100),
    Title = 'Chams Color',
    Transparency = 0,
})

Options.ColorPicker:OnChanged(function()
    print(Options.ColorPicker.Value)
    print(Options.ColorPicker.Transparency)
end)

Options.ColorPicker:SetValueRGB(Color3.fromRGB(0, 255, 140))
```

---

### Keybind

Keybinds are chained onto a label with `:AddKeyPicker`. `Default` is the starting key — use `'MB1'` or `'MB2'` for mouse buttons, or a key name like `'E'`. `Mode` controls the behaviour: `Always` fires every frame the key is held, `Toggle` switches on and off each press, `Hold` only fires while the key is actively held down. `Modes` restricts which modes appear in the picker. Setting `NoUI` to true hides it from the keybind overlay. `SyncToggleState` links the keybind state to a parent toggle so they stay in sync. `ChangedCallback` fires when the user rebinds the key. Use `GetState()` to check if the keybind is currently active and `OnClick` to run something each time it is triggered.

```lua
LeftGroupBox:AddLabel('Activate Key'):AddKeyPicker('MyKeybind', {
    Default = 'MB2',
    Text = 'Activate',
    Mode = 'Toggle',
    Modes = { 'Toggle', 'Hold' },
    NoUI = false,
    SyncToggleState = false,
    ChangedCallback = function(New)
        print('Rebound to:', New)
    end,
})

Options.MyKeybind:OnClick(function()
    print('State:', Options.MyKeybind:GetState())
end)

Options.MyKeybind:OnChanged(function()
    print(Options.MyKeybind.Value)
end)

Options.MyKeybind:SetValue({ 'E', 'Toggle' })
```

---

### Input

Adds a text input box. `Text` is the label shown above it. `Placeholder` is the greyed-out hint when the box is empty. Setting `Numeric` to true only allows numbers. Setting `Finished` to true only fires the callback when the user presses Enter rather than on every keystroke. `MaxLength` caps how many characters can be entered. `ClearTextOnFocus` clears the box when clicked. Access the value with `Options.MyInput.Value`.

```lua
LeftGroupBox:AddInput('MyInput', {
    Text = 'Player Name',
    Placeholder = 'Enter a name...',
    Default = '',
    Numeric = false,
    Finished = true,
    MaxLength = 50,
    ClearTextOnFocus = true,
    Tooltip = 'Type a player name',
})

Options.MyInput:OnChanged(function()
    print(Options.MyInput.Value)
end)

Options.MyInput:SetValue('Roblox')
```

---

### Label

Adds a static text label. Pass `true` as the second argument to allow the text to wrap onto multiple lines.

```lua
local MyLabel = LeftGroupBox:AddLabel('Hello world')
local WrappedLabel = LeftGroupBox:AddLabel('This is long text\nthat wraps.', true)

MyLabel:SetText('Updated text')
```

---

### Divider

Adds a horizontal line to visually separate elements inside a groupbox.

```lua
LeftGroupBox:AddDivider()
```

---

### Blank

Adds empty vertical spacing. The number is the height in pixels.

```lua
LeftGroupBox:AddBlank(10)
```

---

### Dependency Box

A dependency box is a container that shows or hides itself based on the state of other elements. When conditions are not met it collapses entirely, hiding everything inside it. Dependency boxes can be nested — a child box inherits the visibility rules of its parent on top of its own. Add elements to it the same way as a groupbox, then call `SetupDependencies` with a list of `{ element, expectedValue }` pairs.

```lua
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
    { Options.MyDropdown, 'Option A' },
})
```

---

## Library Methods

### Notify

Shows a notification. The second argument is how many seconds it stays on screen, defaulting to 5. The third argument is an optional Roblox asset sound ID to play when it appears.

```lua
Library:Notify('Something happened')
Library:Notify('Watch out!', 3)
Library:Notify('Done', 5, 131961136)
```

---

### Toggle UI

Shows or hides the window. Assign a keybind via `Library.ToggleKeybind` to let users toggle it with a key.

```lua
Library:Toggle()
```

---

### Unload

Destroys the UI and disconnects all signals. `OnUnload` lets you run cleanup code beforehand. Set `Library.Unloaded = true` inside the callback if you need running loops to stop.

```lua
Library:OnUnload(function()
    print('Unloaded!')
    Library.Unloaded = true
end)

Library:Unload()
```

---

## ThemeManager

Gives users a built-in theme picker. Call `SetLibrary` and `SetFolder` before using it. Use `ApplyToTab` to attach the theme UI to a full tab, or `ApplyToGroupbox` to attach it to a groupbox you created yourself.

```lua
ThemeManager:SetLibrary(Library)
ThemeManager:SetFolder('MyHub')

ThemeManager:ApplyToTab(Tabs['UI Settings'])

local ThemeBox = Tabs['UI Settings']:AddLeftGroupbox('Themes')
ThemeManager:ApplyToGroupbox(ThemeBox)
```

---

## SaveManager

Gives users a config save and load system. Call `SetLibrary` and `SetFolder` first. `IgnoreThemeSettings` prevents theme state from being saved into configs. `SetIgnoreIndexes` excludes specific elements from being saved — commonly used for the menu keybind so each config doesn't override it. Pass your UI Settings tab into `BuildConfigSection` to build the config UI. Call `LoadAutoloadConfig` at the end to load the user's autoload config on startup.

```lua
SaveManager:SetLibrary(Library)
SaveManager:SetFolder('MyHub')

SaveManager:IgnoreThemeSettings()
SaveManager:SetIgnoreIndexes({ 'MenuKeybind' })

SaveManager:BuildConfigSection(Tabs['UI Settings'])
SaveManager:LoadAutoloadConfig()
```

---

## Example Script

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
    ['UI Settings'] = Window:AddTab('UI Settings'),
}

local LeftGroupBox  = Tabs.Main:AddLeftGroupbox('Features')
local RightGroupBox = Tabs.Main:AddRightGroupbox('Misc')

LeftGroupBox:AddToggle('MyToggle', {
    Text = 'Enable Feature',
    Default = false,
    Tooltip = 'Turns the feature on or off',
})
Toggles.MyToggle:OnChanged(function()
    print('Toggle:', Toggles.MyToggle.Value)
end)

LeftGroupBox:AddSlider('MySlider', {
    Text = 'Speed',
    Default = 16,
    Min = 0,
    Max = 100,
    Rounding = 0,
    Suffix = ' studs/s',
})
Options.MySlider:OnChanged(function()
    print('Slider:', Options.MySlider.Value)
end)

LeftGroupBox:AddDropdown('MyDropdown', {
    Text = 'Mode',
    Values = { 'Option A', 'Option B', 'Option C' },
    Default = 1,
})
Options.MyDropdown:OnChanged(function()
    print('Dropdown:', Options.MyDropdown.Value)
end)

LeftGroupBox:AddButton({
    Text = 'Do Something',
    Func = function()
        print('Button clicked!')
    end,
    DoubleClick = false,
})

LeftGroupBox:AddDivider()

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

RightGroupBox:AddLabel('Chams Color'):AddColorPicker('MyColor', {
    Default = Color3.fromRGB(255, 100, 100),
    Transparency = 0,
})
Options.MyColor:OnChanged(function()
    print('Color:', Options.MyColor.Value)
end)

RightGroupBox:AddLabel('Activate Key'):AddKeyPicker('MyKeybind', {
    Default = 'MB2',
    Text = 'Activate',
    Mode = 'Toggle',
})
Options.MyKeybind:OnChanged(function()
    print('Keybind:', Options.MyKeybind.Value)
end)

RightGroupBox:AddInput('MyInput', {
    Text = 'Player Name',
    Placeholder = 'Enter name...',
    Finished = true,
})
Options.MyInput:OnChanged(function()
    print('Input:', Options.MyInput.Value)
end)

local MenuGroup = Tabs['UI Settings']:AddLeftGroupbox('Menu')
MenuGroup:AddButton('Unload', function() Library:Unload() end)
MenuGroup:AddLabel('Menu bind'):AddKeyPicker('MenuKeybind', {
    Default = 'End',
    NoUI = true,
    Text = 'Menu keybind',
})
Library.ToggleKeybind = Options.MenuKeybind

ThemeManager:SetLibrary(Library)
SaveManager:SetLibrary(Library)
ThemeManager:SetFolder('MyHub')
SaveManager:SetFolder('MyHub')
SaveManager:IgnoreThemeSettings()
SaveManager:SetIgnoreIndexes({ 'MenuKeybind' })
ThemeManager:ApplyToTab(Tabs['UI Settings'])
SaveManager:BuildConfigSection(Tabs['UI Settings'])
SaveManager:LoadAutoloadConfig()
```
