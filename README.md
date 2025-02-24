local Player = game.Players.LocalPlayer
local Mouse = Player:GetMouse()
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local CloseButton = Instance.new("TextButton")
local HitboxToggle = Instance.new("TextButton")
local Hitboxes = {}

ScreenGui.Parent = Player:WaitForChild("PlayerGui")
ScreenGui.Enabled = true

Frame.Size = UDim2.new(0, 300, 0, 200)
Frame.Position = UDim2.new(0.5, -150, 0.5, -100)
Frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Frame.Parent = ScreenGui
Frame.Active = true
Frame.Draggable = true

CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -40, 0, 10)
CloseButton.Text = "X"
CloseButton.Parent = Frame

HitboxToggle.Size = UDim2.new(0, 100, 0, 50)
HitboxToggle.Position = UDim2.new(0.5, -50, 0.5, -25)
HitboxToggle.Text = "Toggle Hitbox"
HitboxToggle.Parent = Frame

local function createHitbox(player)
    local hitbox = Instance.new("Part")
    hitbox.Size = Vector3.new(10, 10, 10) -- Increased size
    hitbox.Anchored = false
    hitbox.CanCollide = false
    hitbox.Parent = workspace
    hitbox:SetNetworkOwner(nil)

    local function updatePosition()
        while hitbox and hitbox.Parent do
            wait(0.1)
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                hitbox.Position = player.Character.HumanoidRootPart.Position
            end
        end
    end

    coroutine.wrap(updatePosition)()

    player.AncestryChanged:Connect(function(_, parent)
        if not parent then
            hitbox:Destroy()
            Hitboxes[player.Name] = nil
        end
    end)
    
    return hitbox
end

HitboxToggle.MouseButton1Click:Connect(function()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= Player and not Hitboxes[player.Name] then
            local hitbox = createHitbox(player)
            Hitboxes[player.Name] = hitbox

            hitbox.Touched:Connect(function(hit)
                if hit and hit.Parent then
                    local humanoid = hit.Parent:FindFirstChildOfClass("Humanoid")
                    if humanoid then
                        humanoid:TakeDamage(10)
                    end
                end
            end)
        end
    end
end)

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui.Enabled = false
end)
