-- Variables
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local Camera = workspace.CurrentCamera

local Aimbot = {
    Enabled = false,
    Sensitivity = 1,
    Smoothness = 1,
    Target = nil
}

local ESP = {
    Enabled = false,
    Color = Color3.new(1, 0, 0),
    Style = "Box",
    Players = {}
}

-- GUI Creation
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "Skibidi UI"
ScreenGui.Parent = game.CoreGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 200, 0, 250)
MainFrame.Position = UDim2.new(0.5, -100, 0.5, -125)
MainFrame.BackgroundTransparency = 0.5
MainFrame.Visible = false
MainFrame.Parent = ScreenGui

-- Aimbot Controls
local AimbotToggle = Instance.new("TextButton")
AimbotToggle.Size = UDim2.new(0, 180, 0, 30)
AimbotToggle.Position = UDim2.new(0, 10, 0, 10)
AimbotToggle.Text = "Aimbot: OFF"
AimbotToggle.Parent = MainFrame

local SensitivitySlider = Instance.new("TextLabel")
SensitivitySlider.Size = UDim2.new(0, 180, 0, 30)
SensitivitySlider.Position = UDim2.new(0, 10, 0, 50)
SensitivitySlider.Text = "Sensitivity: 1"
SensitivitySlider.Parent = MainFrame

local Slider = Instance.new("TextButton")
Slider.Size = UDim2.new(0, 150, 0, 15)
Slider.Position = UDim2.new(0, 15, 0, 80)
Slider.Text = ""
Slider.BackgroundColor3 = Color3.new(1, 1, 1)
Slider.Parent = MainFrame

-- ESP Controls
local ESPToggle = Instance.new("TextButton")
ESPToggle.Size = UDim2.new(0, 180, 0, 30)
ESPToggle.Position = UDim2.new(0, 10, 0, 110)
ESPToggle.Text = "ESP: OFF"
ESPToggle.Parent = MainFrame

local ColorPicker = Instance.new("TextButton")
ColorPicker.Size = UDim2.new(0, 80, 0, 30)
ColorPicker.Position = UDim2.new(0, 10, 0, 150)
ColorPicker.Text = "Color"
ColorPicker.Parent = MainFrame

local StylePicker = Instance.new("TextButton")
StylePicker.Size = UDim2.new(0, 80, 0, 30)
StylePicker.Position = UDim2.new(0, 100, 0, 150)
StylePicker.Text = "Style: Box"
StylePicker.Parent = MainFrame

-- GUI Toggle
Mouse.KeyDown:Connect(function(Key)
    if Key == "." then
        MainFrame.Visible = not MainFrame.Visible
    end
end)

-- Aimbot Functions
function GetClosestPlayer()
    local Closest = nil
    local MaxDist = math.huge
    
    for _, Player in pairs(Players:GetPlayers()) do
        if Player ~= LocalPlayer and Player.Character and Player.Character:FindFirstChild("Head") then
            local Distance = (Player.Character.Head.Position - LocalPlayer.Character.Head.Position).Magnitude
            if Distance < MaxDist then
                MaxDist = Distance
                Closest = Player
            end
        end
    end
    
    return Closest
end

function AimAt()
    while Aimbot.Enabled and wait() do
        local Target = GetClosestPlayer()
        if Target and Target.Character and Target.Character:FindFirstChild("Head") then
            local Head = Target.Character.Head
            local Position = Camera:WorldToViewportPoint(Head.Position)
            local Smoothness = 1/Aimbot.Sensitivity
            
            mousemoverel(
                (Position.X - Mouse.X) * Aimbot.Sensitivity * Smoothness,
                (Position.Y - Mouse.Y) * Aimbot.Sensitivity * Smoothness
            )
        end
    end
end

-- ESP Functions
function UpdateESP()
    for _, Player in pairs(Players:GetPlayers()) do
        if Player ~= LocalPlayer and Player.Character then
            if not ESP.Players[Player] then
                local Highlight = Instance.new("Highlight")
                Highlight.Adornee = Player.Character
                Highlight.Parent = Player.Character
                ESP.Players[Player] = Highlight
            end
            ESP.Players[Player].OutlineColor = ESP.Color
            ESP.Players[Player].FillTransparency = ESP.Style == "Box" and 0.5 or 1
        end
    end
end

-- GUI Handlers
AimbotToggle.MouseButton1Click:Connect(function()
    Aimbot.Enabled = not Aimbot.Enabled
    AimbotToggle.Text = "Aimbot: " .. (Aimbot.Enabled and "ON" or "OFF")
    if Aimbot.Enabled then
        AimAt()
    end
end)

Slider.MouseButton1Down:Connect(function()
    local X = (Mouse.X - Slider.AbsolutePosition.X) / Slider.AbsoluteSize.X
    Aimbot.Sensitivity = math.clamp(X * 10, 0.1, 10)
    SensitivitySlider.Text = "Sensitivity: " .. math.floor(Aimbot.Sensitivity * 10)/10
end)

ESPToggle.MouseButton1Click:Connect(function()
    ESP.Enabled = not ESP.Enabled
    ESPToggle.Text = "ESP: " .. (ESP.Enabled and "ON" or "OFF")
    UpdateESP()
end)

ColorPicker.MouseButton1Click:Connect(function()
    ESP.Color = Color3.new(math.random(), math.random(), math.random())
    UpdateESP()
end)

StylePicker.MouseButton1Click:Connect(function()
    ESP.Style = ESP.Style == "Box" and "Outline" or "Box"
    StylePicker.Text = "Style: " .. ESP.Style
    UpdateESP()
end)
