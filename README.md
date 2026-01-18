-- 🌧️ CINEMATIC RAIN + GROUND REFLECTION (NO SOUND)
-- By ChatGPT

local Lighting = game:GetService("Lighting")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer

-------------------------------------------------
-- 🎮 CINEMATIC LIGHTING
-------------------------------------------------
Lighting.Technology = Enum.Technology.Future
Lighting.ClockTime = 16.8
Lighting.Brightness = 2.5
Lighting.ExposureCompensation = 0.35
Lighting.GlobalShadows = true

Lighting.Ambient = Color3.fromRGB(120,120,140)
Lighting.OutdoorAmbient = Color3.fromRGB(150,150,170)

-------------------------------------------------
-- 🌫️ ATMOSPHERE
-------------------------------------------------
local atm = Instance.new("Atmosphere", Lighting)
atm.Density = 0.3
atm.Offset = 0.15
atm.Color = Color3.fromRGB(170,190,230)
atm.Decay = Color3.fromRGB(90,110,160)
atm.Glare = 0.25
atm.Haze = 1.5

-------------------------------------------------
-- 🎥 CINEMATIC COLOR
-------------------------------------------------
local cc = Instance.new("ColorCorrectionEffect", Lighting)
cc.Contrast = 0.25
cc.Saturation = -0.05
cc.Brightness = 0.05
cc.TintColor = Color3.fromRGB(220,230,255)

-------------------------------------------------
-- ✨ BLOOM HD
-------------------------------------------------
local bloom = Instance.new("BloomEffect", Lighting)
bloom.Intensity = 0.6
bloom.Size = 30
bloom.Threshold = 1

-------------------------------------------------
-- ☁️ CLOUDS
-------------------------------------------------
local clouds = Instance.new("Clouds", Lighting)
clouds.Cover = 0.7
clouds.Density = 0.9
clouds.Color = Color3.fromRGB(200,200,210)

-------------------------------------------------
-- 🌧️ CINEMATIC 3D RAIN (NO SOUND)
-------------------------------------------------
local function createRain()
    local char = player.Character or player.CharacterAdded:Wait()
    local root = char:WaitForChild("HumanoidRootPart")

    local rainPart = Instance.new("Part", workspace)
    rainPart.Size = Vector3.new(300, 1, 300)
    rainPart.Transparency = 1
    rainPart.Anchored = true
    rainPart.CanCollide = false

    local att = Instance.new("Attachment", rainPart)

    local rain = Instance.new("ParticleEmitter", att)
    rain.Texture = "rbxassetid://4927982270"
    rain.Rate = 2800
    rain.Lifetime = NumberRange.new(1, 1.4)
    rain.Speed = NumberRange.new(90, 130)
    rain.Size = NumberSequence.new(0.15)
    rain.Transparency = NumberSequence.new(0.25)
    rain.Acceleration = Vector3.new(0, -350, 0)
    rain.SpreadAngle = Vector2.new(6, 6)

    RunService.RenderStepped:Connect(function()
        if root then
            rainPart.Position = root.Position + Vector3.new(0, 65, 0)
        end
    end)
end

-------------------------------------------------
-- 💎 WET REFLECTIVE FLOOR SYSTEM
-------------------------------------------------
for _, obj in pairs(workspace:GetDescendants()) do
    if obj:IsA("BasePart") and obj.Material ~= Enum.Material.Glass then
        pcall(function()
            obj.Material = Enum.Material.SmoothPlastic
            obj.Reflectance = 0.35
            obj.Color = obj.Color:Lerp(Color3.fromRGB(120,120,130), 0.12)
        end)
    end
end

-------------------------------------------------
createRain()
print("🌧️ Chuva cinematográfica + reflexo no chão ativados (sem som).")
