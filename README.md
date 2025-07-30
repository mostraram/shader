-- Script: Realismo Visual no Roblox

local Lighting = game:GetService("Lighting")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Configurações de Iluminação
Lighting.Technology = Enum.Technology.Future
Lighting.Brightness = 2.5
Lighting.GlobalShadows = true
Lighting.OutdoorAmbient = Color3.fromRGB(128, 128, 128)
Lighting.Ambient = Color3.fromRGB(80, 80, 80)
Lighting.EnvironmentDiffuseScale = 0.8
Lighting.EnvironmentSpecularScale = 1
Lighting.ClockTime = 14  -- Meio da tarde
Lighting.FogStart = 200
Lighting.FogEnd = 1000
Lighting.FogColor = Color3.fromRGB(200, 200, 255)

-- Atmosfera realista
local atmosphere = Instance.new("Atmosphere")
atmosphere.Density = 0.4
atmosphere.Offset = 0.25
atmosphere.Color = Color3.fromRGB(199, 215, 255)
atmosphere.Decay = Color3.fromRGB(120, 135, 170)
atmosphere.Parent = Lighting

-- Bloom para brilho suave
local bloom = Instance.new("BloomEffect")
bloom.Intensity = 1.5
bloom.Threshold = 2
bloom.Size = 56
bloom.Parent = Lighting

-- ColorCorrection para saturação e contraste
local colorCorrection = Instance.new("ColorCorrectionEffect")
colorCorrection.TintColor = Color3.fromRGB(255, 240, 230)
colorCorrection.Contrast = 0.1
colorCorrection.Saturation = 0.3
colorCorrection.Brightness = 0.05
colorCorrection.Parent = Lighting

-- DepthOfField para profundidade de campo cinematográfica
local dof = Instance.new("DepthOfFieldEffect")
dof.FarIntensity = 0.15
dof.FocusDistance = 60
dof.InFocusRadius = 30
dof.NearIntensity = 0.3
dof.Parent = Lighting

-- SunRays para iluminação solar suave
local sunrays = Instance.new("SunRaysEffect")
sunrays.Intensity = 0.2
sunrays.Spread = 0.8
sunrays.Parent = Lighting

-- Skybox realista (opcional: substitua IDs por outros que você quiser)
local sky = Instance.new("Sky")
sky.SkyboxBk = "rbxassetid://4608572134"
sky.SkyboxDn = "rbxassetid://4608572781"
sky.SkyboxFt = "rbxassetid://4608572390"
sky.SkyboxLf = "rbxassetid://4608572023"
sky.SkyboxRt = "rbxassetid://4608571649"
sky.SkyboxUp = "rbxassetid://4608572926"
sky.SunAngularSize = 20
sky.CelestialBodiesShown = true
sky.StarCount = 3000
sky.Parent = Lighting
