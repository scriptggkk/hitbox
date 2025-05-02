-- Functional Hitbox Expansion Script with Damage Redirection
-- This script expands player hitboxes and redirects any hits to the expanded area to the actual player

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
MainFrame.Size = UDim2.new(0, 300, 0, 400)  -- Increased height for target list
MainFrame.Position = UDim2.new(0.5, -150, 0.5, -200)
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

-- Size Input (Text Box instead of slider)
local SizeFrame = Instance.new("Frame")
SizeFrame.Name = "SizeFrame"
SizeFrame.Size = UDim2.new(1, 0, 0, 50)
SizeFrame.Position = UDim2.new(0, 0, 0, 60)
SizeFrame.BackgroundTransparency = 1
SizeFrame.Parent = ContentFrame

local SizeLabel = Instance.new("TextLabel")
SizeLabel.Name = "SizeLabel"
SizeLabel.Size = UDim2.new(0.5, 0, 1, 0)
SizeLabel.BackgroundTransparency = 1
SizeLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
SizeLabel.TextSize = 16
SizeLabel.Font = Enum.Font.SourceSans
SizeLabel.TextXAlignment = Enum.TextXAlignment.Left
SizeLabel.Text = "Hitbox Size:"
SizeLabel.Parent = SizeFrame

local SizeInput = Instance.new("TextBox")
SizeInput.Name = "SizeInput"
SizeInput.Size = UDim2.new(0.5, -10, 0, 30)
SizeInput.Position = UDim2.new(0.5, 0, 0.5, -15)
SizeInput.BackgroundColor3 = Color3.fromRGB(60, 60, 65)
SizeInput.BorderSizePixel = 0
SizeInput.TextColor3 = Color3.fromRGB(255, 255, 255)
SizeInput.TextSize = 16
SizeInput.Font = Enum.Font.SourceSans
SizeInput.Text = "2"
SizeInput.PlaceholderText = "Enter size..."
SizeInput.ClearTextOnFocus = false
SizeInput.Parent = SizeFrame

-- Color Picker
local ColorFrame = Instance.new("Frame")
ColorFrame.Name = "ColorFrame"
ColorFrame.Size = UDim2.new(1, 0, 0, 140)
ColorFrame.Position = UDim2.new(0, 0, 0, 120)
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

-- Target management section
local TargetFrame = Instance.new("Frame")
TargetFrame.Name = "TargetFrame"
TargetFrame.Size = UDim2.new(1, 0, 0, 130)
TargetFrame.Position = UDim2.new(0, 0, 0, 260)
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
TargetLabel.Text = "Select Targets"
TargetLabel.Parent = TargetFrame

local TargetScrollFrame = Instance.new("ScrollingFrame")
TargetScrollFrame.Name = "TargetScrollFrame"
TargetScrollFrame.Size = UDim2.new(1, 0, 0, 80)
TargetScrollFrame.Position = UDim2.new(0, 0, 0, 25)
TargetScrollFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 55)
TargetScrollFrame.BorderSizePixel = 0
TargetScrollFrame.ScrollBarThickness = 4
TargetScrollFrame.ScrollingDirection = Enum.ScrollingDirection.Y
TargetScrollFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
TargetScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
TargetScrollFrame.Parent = TargetFrame

local TargetListLayout = Instance.new("UIListLayout")
TargetListLayout.Name = "TargetListLayout"
TargetListLayout.Padding = UDim.new(0, 2)
TargetListLayout.SortOrder = Enum.SortOrder.LayoutOrder
TargetListLayout.Parent = TargetScrollFrame

local SelectTargetButton = Instance.new("TextButton")
SelectTargetButton.Name = "SelectTargetButton"
SelectTargetButton.Size = UDim2.new(0.48, 0, 0, 25)
SelectTargetButton.Position = UDim2.new(0, 0, 1, -25)
SelectTargetButton.BackgroundColor3 = Color3.fromRGB(65, 175, 255)
SelectTargetButton.BorderSizePixel = 0
SelectTargetButton.TextColor3 = Color3.fromRGB(255, 255, 255)
SelectTargetButton.TextSize = 14
SelectTargetButton.Font = Enum.Font.SourceSansBold
SelectTargetButton.Text = "Add Target"
SelectTargetButton.Parent = TargetFrame

local SelectAllButton = Instance.new("TextButton")
SelectAllButton.Name = "SelectAllButton"
SelectAllButton.Size = UDim2.new(0.48, 0, 0, 25)
SelectAllButton.Position = UDim2.new(0.52, 0, 1, -25)
SelectAllButton.BackgroundColor3 = Color3.fromRGB(75, 165, 75)
SelectAllButton.BorderSizePixel = 0
SelectAllButton.TextColor3 = Color3.fromRGB(255, 255, 255)
SelectAllButton.TextSize = 14
SelectAllButton.Font = Enum.Font.SourceSansBold
SelectAllButton.Text = "Select All Players"
SelectAllButton.Parent = TargetFrame

-- Script Variables
local hitboxEnabled = false
local hitboxSize = 2
local hitboxColor = colors[3].color -- Default blue
local targetPlayers = {}
local originalCharacterParts = {}
local modifiedParts = {}

-- Create target entry in the list
local function createTargetEntry(player)
    local entry = Instance.new("Frame")
    entry.Name = player.Name .. "_Entry"
    entry.Size = UDim2.new(1, -10, 0, 25)
    entry.BackgroundColor3 = Color3.fromRGB(60, 60, 65)
    entry.BorderSizePixel = 0
    entry.Parent = TargetScrollFrame
    
    local playerName = Instance.new("TextLabel")
    playerName.Name = "PlayerName"
    playerName.Size = UDim2.new(0.7, 0, 1, 0)
    playerName.Position = UDim2.new(0, 5, 0, 0)
    playerName.BackgroundTransparency = 1
    playerName.TextColor3 = Color3.fromRGB(255, 255, 255)
    playerName.TextSize = 14
    playerName.Font = Enum.Font.SourceSans
    playerName.TextXAlignment = Enum.TextXAlignment.Left
    playerName.Text = player.Name
    playerName.Parent = entry
    
    local removeButton = Instance.new("TextButton")
    removeButton.Name = "RemoveButton"
    removeButton.Size = UDim2.new(0.3, -10, 1, -6)
    removeButton.Position = UDim2.new(0.7, 5, 0, 3)
    removeButton.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
    removeButton.BorderSizePixel = 0
    removeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    removeButton.TextSize = 14
    removeButton.Font = Enum.Font.SourceSansBold
    removeButton.Text = "Remove"
    removeButton.Parent = entry
    
    removeButton.MouseButton1Click:Connect(function()
        -- Restore character if needed
        if targetPlayers[player] and hitboxEnabled then
            restoreOriginalCharacter(player)
        end
        targetPlayers[player] = nil
        entry:Destroy()
    end)
    
    return entry
end

-- Handle toggle button click
local function updateToggle()
    if hitboxEnabled then
        ToggleIndicator:TweenPosition(UDim2.new(0, 28, 0.5, -10), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.2, true)
        ToggleButton.BackgroundColor3 = Color3.fromRGB(65, 175, 255)
        
        -- Apply hitbox to all targets
        for player, _ in pairs(targetPlayers) do
            if player and player.Character then
                createHitbox(player)
            end
        end
    else
        ToggleIndicator:TweenPosition(UDim2.new(0, 2, 0.5, -10), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.2, true)
        ToggleButton.BackgroundColor3 = Color3.fromRGB(60, 60, 65)
        
        -- Restore original character settings for all targets
        for player, _ in pairs(targetPlayers) do
            restoreOriginalCharacter(player)
        end
    end
end

ToggleButton.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        hitboxEnabled = not hitboxEnabled
        updateToggle()
    end
end)

-- Handle size input change
SizeInput.FocusLost:Connect(function(enterPressed)
    local inputText = SizeInput.Text
    local newSize = tonumber(inputText)
    
    if newSize and newSize > 0 then
        hitboxSize = newSize
        SizeInput.Text = tostring(hitboxSize)
        
        -- Update hitboxes if enabled
        if hitboxEnabled then
            for player, _ in pairs(targetPlayers) do
                updateHitbox(player)
            end
        end
    else
        -- Reset to previous value if invalid input
        SizeInput.Text = tostring(hitboxSize)
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
                
                -- Update hitbox color for all targets
                if hitboxEnabled then
                    for player, parts in pairs(modifiedParts) do
                        for _, part in pairs(parts) do
                            part.Color = hitboxColor
                        end
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
                if player and player ~= LocalPlayer and not targetPlayers[player] then
                    -- Add player to targets
                    targetPlayers[player] = true
                    createTargetEntry(player)
                    
                    -- Save original character state
                    saveOriginalCharacter(player)
                    
                    -- Create hitbox if enabled
                    if hitboxEnabled then
                        createHitbox(player)
                    end
                end
            end
        end
        
        SelectTargetButton.Text = "Add Target"
        connection:Disconnect()
    end)
end)

-- Handle select all players
SelectAllButton.MouseButton1Click:Connect(function()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and not targetPlayers[player] then
            -- Add player to targets
            targetPlayers[player] = true
            createTargetEntry(player)
            
            -- Save original character state
            saveOriginalCharacter(player)
            
            -- Create hitbox if enabled
            if hitboxEnabled then
                createHitbox(player)
            end
        end
    end
end)

-- Save original character state
function saveOriginalCharacter(player)
    if not originalCharacterParts[player] then
        originalCharacterParts[player] = {}
    end
    
    if player and player.Character then
        for _, part in pairs(player.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                originalCharacterParts[player][part] = {
                    size = part.Size,
                    transparency = part.Transparency,
                    collisionGroupId = part.CollisionGroupId
                }
            end
        end
    end
end

-- Setup hitbox collision detection
function setupHitboxCollision(part, player)
    part.Touched:Connect(function(hit)
        -- Check if the hit is from a damage source
        local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
        if not humanoid then return end
        
        local damageSource = nil
        
        -- Case 1: Hit is a bullet/projectile
        if hit:FindFirstAncestorOfClass("Tool") or hit:IsA("BasePart") and hit.Name:lower():match("bullet") then
            damageSource = hit
            
        -- Case 2: Hit is part of a weapon/tool
        elseif hit:IsA("BasePart") and hit.Parent:IsA("Tool") then
            damageSource = hit.Parent
        end
        
        -- If we found a damage source, apply damage to player
        if damageSource then
            local damage = 10 -- Default damage value
            
            -- Get damage value from tool if available
            if damageSource:FindFirstChild("Damage") then
                damage = damageSource.Damage.Value
            end
            
            -- Apply damage to player
            humanoid:TakeDamage(damage)
            
            -- Visual feedback
            local hitEffect = Instance.new("Part")
            hitEffect.Size = Vector3.new(0.2, 0.2, 0.2)
            hitEffect.Position = hit.Position
            hitEffect.Anchored = true
            hitEffect.CanCollide = false
            hitEffect.Transparency = 0.5
            hitEffect.Color = Color3.fromRGB(255, 0, 0)
            hitEffect.Material = Enum.Material.Neon
            hitEffect.Parent = workspace
            game:GetService("Debris"):AddItem(hitEffect, 0.5)
        end
    end)
end

-- Create functional hitbox
function createHitbox(player)
    if not modifiedParts[player] then
        modifiedParts[player] = {}
    end
    
    if player and player.Character then
        -- First, find the HumanoidRootPart to determine character center
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if not hrp then return end
        
        -- Find all collision parts
        for _, part in pairs(player.Character:GetDescendants()) do
            if part:IsA("BasePart") and part.CanCollide then
                -- Create a hitbox expansion for this part
                expandPart(part)
                setupHitboxCollision(part, player) -- Add collision detection
                table.insert(modifiedParts[player], part)
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
        
        -- Setup collision detection for the main hitbox
        setupHitboxCollision(mainHitbox, player)
        
        -- Ensure shots can hit this part
        mainHitbox.Parent = player.Character
        table.insert(modifiedParts[player], mainHitbox)
        
        -- Set collision properties
        for _, part in pairs(modifiedParts[player]) do
            part.CanQuery = true
            part.CanTouch = true
        end
    end
end

-- Expand a single part
function expandPart(part)
    -- Make part slightly larger
    local expansion = hitboxSize * 0.2 -- Scale factor based on size input
    part.Size = part.Size * (1 + expansion)
    
    -- Make it partially transparent so we can see the visual effect
    part.Transparency = 0.7
    part.Color = hitboxColor
end

-- Update hitbox for a specific player
function updateHitbox(player)
    restoreOriginalCharacter(player)
    createHitbox(player)
end

-- Restore original character for a specific player
function restoreOriginalCharacter(player)
    if originalCharacterParts[player] then
        for part, props in pairs(originalCharacterParts[player]) do
            if part and part:IsA("BasePart") and part.Parent then
                part.Size = props.size
                part.Transparency = props.transparency
                part.CollisionGroupId = props.collisionGroupId
            end
        end
    end
    
    -- Remove any created parts
    if modifiedParts[player] then
        for _, part in pairs(modifiedParts[player]) do
            if part and part.Name == "MainHitbox" then
                part:Destroy()
            end 
        end
        
        modifiedParts[player] = {}
    end
end

-- Handle player leaving
Players.PlayerRemoving:Connect(function(player)
    if targetPlayers[player] then
        targetPlayers[player] = nil
        
        -- Find and remove entry from the list
        local entry = TargetScrollFrame:FindFirstChild(player.Name .. "_Entry")
        if entry then
            entry:Destroy()
        end
        
        -- Clean up saved data
        originalCharacterParts[player] = nil
        modifiedParts[player] = nil
    end
end)

-- Handle character changes
game.Workspace.DescendantAdded:Connect(function(descendant)
    if hitboxEnabled and descendant:IsA("Model") and descendant:FindFirstChild("Humanoid") then
        local player = Players:GetPlayerFromCharacter(descendant)
        if player and targetPlayers[player] then
            -- Character respawned or changed, update the hitbox
            wait(0.5) -- Wait for character to fully load
            saveOriginalCharacter(player)
            createHitbox(player)
        end
    end
end)

-- Handle new players joining
Players.PlayerAdded:Connect(function(player)
    -- Check if "select all" is active, add them automatically
    local selectAllActive = false
    
    if selectAllActive and player ~= LocalPlayer then
        targetPlayers[player] = true
        createTargetEntry(player)
        
        -- When their character loads
        player.CharacterAdded:Connect(function(character)
            if targetPlayers[player] then
                wait(0.5) -- Wait for character to load
                saveOriginalCharacter(player)
                if hitboxEnabled then
                    createHitbox(player)
                end
            end
        end)
    end
end)

-- Close button
CloseButton.MouseButton1Click:Connect(function()
    -- Restore all characters
    for player, _ in pairs(targetPlayers) do
        restoreOriginalCharacter(player)
    end
    ScreenGui:Destroy()
end)

-- Initialize with blue selected
local blueButton = ColorFrame:FindFirstChild("BlueButton")
if blueButton then
    blueButton.BorderColor3 = Color3.fromRGB(255, 255, 255)
    blueButton.BorderSizePixel = 3
end

print("Hitbox Expander with Damage Redirection loaded successfully!")
