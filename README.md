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
    hitbox.Size = Vector3.new(5, 5, 5)
    hitbox.Position = player.Character.HumanoidRootPart.Position
    hitbox.Anchored = true
    hitbox.CanCollide = false
    hitbox.Parent = workspace
    Hitboxes[player.Name] = hitbox

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
        if player ~= Player then
            local hitbox = createHitbox(player)
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
