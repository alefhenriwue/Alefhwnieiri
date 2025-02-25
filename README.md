local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

local ESPEnabled = false
local FOVSize = 100

local function CreateESP(player)
    local Box = Drawing.new("Square")
    Box.Visible = false
    Box.Color = player.Team == LocalPlayer.Team and Color3.fromRGB(0, 0, 255) or Color3.fromRGB(255, 0, 0)
    Box.Thickness = 2
    Box.Filled = false

    local function Update()
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local RootPart = player.Character.HumanoidRootPart
            local Vector, OnScreen = Camera:WorldToViewportPoint(RootPart.Position)

            Box.Visible = OnScreen and ESPEnabled
            Box.Size = Vector2.new(FOVSize, FOVSize)
            Box.Position = Vector2.new(Vector.X - FOVSize / 2, Vector.Y - FOVSize / 2)
        else
            Box.Visible = false
        end
    end

    RunService.RenderStepped:Connect(Update)
end

for _, player in ipairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        CreateESP(player)
    end
end

Players.PlayerAdded:Connect(function(player)
    if player ~= LocalPlayer then
        CreateESP(player)
    end
end)

UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.F then
        ESPEnabled = not ESPEnabled
    end
end)
