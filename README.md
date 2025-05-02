-- Functional Hitbox Expansion Script (Educational Purposes Only)
-- This script expands player hitboxes so interactions (like shots) will register on the expanded area

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

-- Create the GUI for the hitbox controller
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "HitboxController"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.CoreGui

-- Main Frame
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 300, 0, 350)
MainFrame.Position = UDim2.new(0.5, -150, 0.5, -175)
MainFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

-- Title Bar
local TitleBar = Instance.new("Frame")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, 30)
TitleBar.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
TitleBar.BorderSizePixel = 0
TitleBar.Parent = MainFrame

-- Title Text
local TitleText = Instance.new("TextLabel")
TitleText.Name = "TitleText"
TitleText.Size = UDim2.new(1, -10, 1, 0)
TitleText.Position = UDim2.new(0, 10, 0, 0)
TitleText.BackgroundTransparency = 1
TitleText.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleText.TextSize = 16
TitleText.Font = Enum.Font.SourceSansBold
TitleText.TextXAlignment = Enum.TextXAlignment.Left
TitleText.Text = "Hitbox Expander"
TitleText.Parent = TitleBar

-- Close Button
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -30, 0, 0)
CloseButton.BackgroundTransparency = 1
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 16
CloseButton.Font = Enum.Font.SourceSansBold
CloseButton.Text = "X"
CloseButton.Parent = TitleBar

-- Content Frame
local ContentFrame = Instance.new("Frame")
ContentFrame.Name = "ContentFrame"
ContentFrame.Size = UDim2.new(1, -20, 1, -40)
ContentFrame.Position = UDim2.new(0, 10, 0, 35)
ContentFrame.BackgroundTransparency = 1
ContentFrame.Parent = MainFrame

-- Toggle Button
local ToggleFrame = Instance.new("Frame")
ToggleFrame.Name = "ToggleFrame"
ToggleFrame.Size = UDim2.new(1, 0, 0, 50)
ToggleFrame.BackgroundTransparency = 1
ToggleFrame.Parent = ContentFrame

local ToggleLabel = Instance.new("TextLabel")
ToggleLabel.Name = "ToggleLabel"
ToggleLabel.Size = UDim2.new(0.6, 0, 1, 0)
ToggleLabel.BackgroundTransparency = 1
ToggleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleLabel.TextSize = 16
ToggleLabel.Font = Enum.Font.SourceSans
ToggleLabel.TextXAlignment = Enum.TextXAlignment.Left
ToggleLabel.Text = "Enable Hitbox Expander"
ToggleLabel.Parent = ToggleFrame

local ToggleButton = Instance.new("Frame")
ToggleButton.Name = "ToggleButton"
ToggleButton.Size = UDim2.new(0, 50, 0, 24)
ToggleButton.Position = UDim2.new(1, -50, 0.5, -12)
ToggleButton.BackgroundColor3 = Color3.fromRGB(60, 60, 65)
ToggleButton.BorderSizePixel = 0
ToggleButton.Parent = ToggleFrame

local ToggleIndicator = Instance.new("Frame")
ToggleIndicator.Name = "ToggleIndicator"
ToggleIndicator.Size = UDim2.new(0, 20, 0, 20)
ToggleIndicator.Position = UDim2.new(0, 2, 0.5, -10)
ToggleIndicator.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
ToggleIndicator.BorderSizePixel = 0
ToggleIndicator.Parent = ToggleButton

-- Size Slider
local SizeFrame = Instance.new("Frame")
SizeFrame.Name = "SizeFrame"
SizeFrame.Size = UDim2.new(1, 0, 0, 70)
SizeFrame.Position = UDim2.new(0, 0, 0, 60)
SizeFrame.BackgroundTransparency = 1
SizeFrame.Parent = ContentFrame

local SizeLabel = Instance.new("TextLabel")
SizeLabel.Name = "SizeLabel"
SizeLabel.Size = UDim2.new(1, 0, 0, 20)
SizeLabel.BackgroundTransparency = 1
SizeLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
SizeLabel.TextSize = 16
SizeLabel.Font = Enum.Font.SourceSans
SizeLabel.TextXAlignment = Enum.TextXAlignment.Left
SizeLabel.Text = "Hitbox Size: 2"
SizeLabel.Parent = SizeFrame

local SizeSlider = Instance.new("Frame")
SizeSlider.Name = "SizeSlider"
SizeSlider.Size = UDim2.new(1, 0, 0, 6)
SizeSlider.Position = UDim2.new(0, 0, 0.5, 0)
SizeSlider.BackgroundColor3 = Color3.fromRGB(60, 60, 65)
SizeSlider.BorderSizePixel = 0
SizeSlider.Parent = SizeFrame

local SizeIndicator = Instance.new("Frame")
SizeIndicator.Name = "SizeIndicator"
SizeIndicator.Size = UDim2.new(0, 16, 0, 16)
SizeIndicator.Position = UDim2.new(0.2, -8, 0.5, -8)
SizeIndicator.BackgroundColor3 = Color3.fromRGB(65, 175, 255)
SizeIndicator.BorderSizePixel = 0
SizeIndicator.Parent = SizeSlider

-- Color Picker
local ColorFrame = Instance.new("Frame")
ColorFrame.Name = "ColorFrame"
ColorFrame.Size = UDim2.new(1, 0, 0, 140)
ColorFrame.Position = UDim2.new(0, 0, 0, 140)
ColorFrame.BackgroundTransparency = 1
ColorFrame.Parent = ContentFrame

local ColorLabel = Instance.new("TextLabel")
ColorLabel.Name = "ColorLabel"
ColorLabel.Size = UDim2.new(1, 0, 0, 20)
ColorLabel.BackgroundTransparency = 1
ColorLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
ColorLabel.TextSize = 16
ColorLabel.Font = Enum.Font.SourceSans
ColorLabel.TextXAlignment = Enum.TextXAlignment.Left
ColorLabel.Text = "Hitbox Color"
ColorLabel.Parent = ColorFrame

-- Create color buttons
local colors = {
    {name = "Red", color = Color3.fromRGB(255, 0, 0)},
    {name = "Green", color = Color3.fromRGB(0, 255, 0)},
    {name = "Blue", color = Color3.fromRGB(0, 0, 255)},
    {name = "Yellow", color = Color3.fromRGB(255, 255, 0)},
    {name = "Purple", color = Color3.fromRGB(255, 0, 255)},
    {name = "Cyan", color = Color3.fromRGB(0, 255, 255)},
    {name = "White", color = Color3.fromRGB(255, 255, 255)},
    {name = "Black", color = Color3.fromRGB(0, 0, 0)}
}

-- Set up the grid layout for color buttons
local buttonSize = 50
local padding = 10
local buttonsPerRow = 4

for i, colorInfo in ipairs(colors) do
    local row = math.floor((i-1) / buttonsPerRow)
    local col = (i-1) % buttonsPerRow
    
    local ColorButton = Instance.new("Frame")
    ColorButton.Name = colorInfo.name .. "Button"
    ColorButton.Size = UDim2.new(0, buttonSize, 0, buttonSize)
    ColorButton.Position = UDim2.new(0, col * (buttonSize + padding), 0, 30 + row * (buttonSize + padding))
    ColorButton.BackgroundColor3 = colorInfo.color
    ColorButton.BorderSizePixel = 2
    ColorButton.BorderColor3 = Color3.fromRGB(40, 40, 40)
    ColorButton.Parent = ColorFrame
    
    local ColorButtonLabel = Instance.new("TextLabel")
    ColorButtonLabel.Name = "Label"
    ColorButtonLabel.Size = UDim2.new(1, 0, 0, 15)
    ColorButtonLabel.Position = UDim2.new(0, 0, 1, 2)
    ColorButtonLabel.BackgroundTransparency = 1
    ColorButtonLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    ColorButtonLabel.TextSize = 12
    ColorButtonLabel.Font = Enum.Font.SourceSans
    ColorButtonLabel.Text = colorInfo.name
    ColorButtonLabel.Parent = ColorButton
end

-- Target selection section
local TargetFrame = Instance.new("Frame")
TargetFrame.Name = "TargetFrame"
TargetFrame.Size = UDim2.new(1, 0, 0, 50)
TargetFrame.Position = UDim2.new(0, 0, 0, 290)
TargetFrame.BackgroundTransparency = 1
TargetFrame.Parent = ContentFrame

local TargetLabel = Instance.new("TextLabel")
TargetLabel.Name = "TargetLabel"
TargetLabel.Size = UDim2.new(1, 0, 0, 20)
TargetLabel.BackgroundTransparency = 1
TargetLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TargetLabel.TextSize = 16
TargetLabel.Font = Enum.Font.SourceSans
TargetLabel.TextXAlignment = Enum.TextXAlignment.Left
TargetLabel.Text = "No Target Selected"
TargetLabel.Parent = TargetFrame

local SelectTargetButton = Instance.new("TextButton")
SelectTargetButton.Name = "SelectTargetButton"
SelectTargetButton.Size = UDim2.new(1, 0, 0, 30)
SelectTargetButton.Position = UDim2.new(0, 0, 0, 20)
SelectTargetButton.BackgroundColor3 = Color3.fromRGB(65, 175, 255)
SelectTargetButton.BorderSizePixel = 0
SelectTargetButton.TextColor3 = Color3.fromRGB(255, 255, 255)
SelectTargetButton.TextSize = 16
SelectTargetButton.Font = Enum.Font.SourceSansBold
SelectTargetButton.Text = "Select Target"
SelectTargetButton.Parent = TargetFrame

-- Script Variables
local hitboxEnabled = false
local hitboxSize = 2
local hitboxColor = colors[3].color -- Default blue
local targetPlayer = nil
local hitboxPart = nil
local originalCharacterParts = {}
local modifiedParts = {}

-- Handle toggle button click
local function updateToggle()
    if hitboxEnabled then
        ToggleIndicator:TweenPosition(UDim2.new(0, 28, 0.5, -10), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.2, true)
        ToggleButton.BackgroundColor3 = Color3.fromRGB(65, 175, 255)
        
        -- Create hitbox if we have a target
        if targetPlayer and targetPlayer.Character then
            createHitbox()
        end
    else
        ToggleIndicator:TweenPosition(UDim2.new(0, 2, 0.5, -10), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.2, true)
        ToggleButton.BackgroundColor3 = Color3.fromRGB(60, 60, 65)
        
        -- Restore original character settings
        restoreOriginalCharacter()
    end
end

ToggleButton.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        hitboxEnabled = not hitboxEnabled
        updateToggle()
    end
end)

-- Handle size slider
local isDragging = false

SizeSlider.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        isDragging = true
    end
end)

SizeSlider.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        isDragging = false
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if isDragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local sizeFrame = SizeSlider.AbsolutePosition.X
        local maxSize = SizeSlider.AbsoluteSize.X
        local mousePos = input.Position.X
        local relativePos = math.clamp((mousePos - sizeFrame) / maxSize, 0, 1)
        
        SizeIndicator.Position = UDim2.new(relativePos, -8, 0.5, -8)
        hitboxSize = math.floor(relativePos * 10) + 1
        SizeLabel.Text = "Hitbox Size: " .. hitboxSize
        
        -- Update hitbox if it exists
        if hitboxEnabled and targetPlayer then
            updateHitbox()
        end
    end
end)

-- Handle color selection
for _, colorInfo in ipairs(colors) do
    local button = ColorFrame:FindFirstChild(colorInfo.name .. "Button")
    if button then
        button.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                hitboxColor = colorInfo.color
                
                -- Update all button borders
                for _, otherColorInfo in ipairs(colors) do
                    local otherButton = ColorFrame:FindFirstChild(otherColorInfo.name .. "Button")
                    if otherButton then
                        if otherColorInfo.name == colorInfo.name then
                            otherButton.BorderColor3 = Color3.fromRGB(255, 255, 255)
                            otherButton.BorderSizePixel = 3
                        else
                            otherButton.BorderColor3 = Color3.fromRGB(40, 40, 40)
                            otherButton.BorderSizePixel = 2
                        end
                    end
                end
                
                -- Update hitbox if it exists
                if hitboxEnabled and targetPlayer then
                    for _, part in pairs(modifiedParts) do
                        part.Color = hitboxColor
                    end
                end
            end
        end)
    end
end

-- Handle target selection
SelectTargetButton.MouseButton1Click:Connect(function()
    SelectTargetButton.Text = "Click on a Player..."
    
    local connection
    connection = Mouse.Button1Down:Connect(function()
        local target = Mouse.Target
        if target and target:IsDescendantOf(workspace) then
            local character = target:FindFirstAncestorOfClass("Model")
            if character then
                local player = Players:GetPlayerFromCharacter(character)
                if player and player ~= LocalPlayer then
                    -- If we had a previous target, restore their character
                    if targetPlayer and targetPlayer ~= player then
                        restoreOriginalCharacter()
                    end
                    
                    targetPlayer = player
                    TargetLabel.Text = "Target: " .. player.Name
                    
                    -- Save original character state
                    saveOriginalCharacter()
                    
                    -- Create hitbox if enabled
                    if hitboxEnabled then
                        createHitbox()
                    end
                end
            end
        end
        
        SelectTargetButton.Text = "Select Target"
        connection:Disconnect()
    end)
end)

-- Save original character state
function saveOriginalCharacter()
    originalCharacterParts = {}
    if targetPlayer and targetPlayer.Character then
        for _, part in pairs(targetPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                originalCharacterParts[part] = {
                    size = part.Size,
                    transparency = part.Transparency,
                    collisionGroupId = part.CollisionGroupId
                }
            end
        end
    end
end

-- Create functional hitbox
function createHitbox()
    modifiedParts = {}
    
    if targetPlayer and targetPlayer.Character then
        -- First, find the HumanoidRootPart to determine character center
        local hrp = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
        if not hrp then return end
        
        -- Find all collision parts
        for _, part in pairs(targetPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") and part.CanCollide then
                -- Create a hitbox expansion for this part
                expandPart(part)
                table.insert(modifiedParts, part)
            end
        end
        
        -- Create a central collision part (main hitbox)
        local mainHitbox = Instance.new("Part")
        mainHitbox.Name = "MainHitbox"
        mainHitbox.Transparency = 0.8
        mainHitbox.Color = hitboxColor
        mainHitbox.Material = Enum.Material.ForceField
        mainHitbox.CanCollide = true
        mainHitbox.CanTouch = true
        mainHitbox.CanQuery = true
        mainHitbox.Size = Vector3.new(hitboxSize, hitboxSize, hitboxSize)
        mainHitbox.CFrame = hrp.CFrame
        mainHitbox.Anchored = false
        
        -- Create a weld to attach the hitbox to the character
        local weld = Instance.new("WeldConstraint")
        weld.Part0 = mainHitbox
        weld.Part1 = hrp
        weld.Parent = mainHitbox
        
        -- Ensure shots can hit this part
        mainHitbox.Parent = targetPlayer.Character
        table.insert(modifiedParts, mainHitbox)
        
        -- Set collision properties
        -- This ensures the hitbox interacts with weapons/projectiles
        for _, part in pairs(modifiedParts) do
            -- Make sure the hitbox can be detected by raycasts (for weapons)
            part.CanQuery = true
            -- Make it interact with projectiles/weapons
            part.CanTouch = true
        end
    end
end

-- Expand a single part
function expandPart(part)
    -- Make part slightly larger
    local expansion = hitboxSize * 0.2 -- Scale factor based on slider
    part.Size = part.Size * (1 + expansion)
    
    -- Make it partially transparent so we can see the visual effect
    part.Transparency = 0.7
    part.Color = hitboxColor
end

-- Update all hitbox parts
function updateHitbox()
    restoreOriginalCharacter()
    createHitbox()
end

-- Restore original character
function restoreOriginalCharacter()
    for part, props in pairs(originalCharacterParts) do
        if part and part:IsA("BasePart") and part.Parent then
            part.Size = props.size
            part.Transparency = props.transparency
            part.CollisionGroupId = props.collisionGroupId
        end
    end
    
    -- Remove any created parts
    for _, part in pairs(modifiedParts) do
        if part and part.Name == "MainHitbox" then
            part:Destroy()
        end 
    end
    
    modifiedParts = {}
end

-- Handle player leaving
Players.PlayerRemoving:Connect(function(player)
    if player == targetPlayer then
        targetPlayer = nil
        TargetLabel.Text = "No Target Selected"
        restoreOriginalCharacter()
        originalCharacterParts = {}
    end
end)

-- Handle character changes
game.Workspace.DescendantAdded:Connect(function(descendant)
    if hitboxEnabled and targetPlayer and descendant:IsA("Model") and descendant:FindFirstChild("Humanoid") then
        local player = Players:GetPlayerFromCharacter(descendant)
        if player and player == targetPlayer then
            -- Character respawned or changed, update the hitbox
            wait(0.5) -- Wait for character to fully load
            saveOriginalCharacter()
            createHitbox()
        end
    end
end)

-- Close button
CloseButton.MouseButton1Click:Connect(function()
    restoreOriginalCharacter()
    ScreenGui:Destroy()
end)

-- Initialize with blue selected
local blueButton = ColorFrame:FindFirstChild("BlueButton")
if blueButton then
    blueButton.BorderColor3 = Color3.fromRGB(255, 255, 255)
    blueButton.BorderSizePixel = 3
end

print("Hitbox Expander loaded successfully!")
