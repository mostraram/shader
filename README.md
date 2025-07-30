-- LocalScript (ex: em StarterPlayerScripts ou StarterCharacterScripts)
local Players = game:GetService("Players")
local Lighting = game:GetService("Lighting")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

local Vignette = true -- Ativar/desativar Vignette

-- Limpar efeitos anteriores
for _, v in pairs(Lighting:GetChildren()) do
	if v:IsA("PostEffect") or v:IsA("Atmosphere") or v:IsA("Sky") then
		v:Destroy()
	end
end

-- Aplicar efeitos de shader
local function applyEffects()
	local Bloom = Instance.new("BloomEffect")
	Bloom.Intensity = 0.3
	Bloom.Size = 10
	Bloom.Threshold = 0.8
	Bloom.Name = "CustomBloom"
	Bloom.Parent = Lighting

	local Blur = Instance.new("BlurEffect")
	Blur.Size = 5
	Blur.Name = "CustomBlur"
	Blur.Parent = Lighting

	local ColorCor = Instance.new("ColorCorrectionEffect")
	ColorCor.Brightness = 0.1
	ColorCor.Contrast = 0.5
	ColorCor.Saturation = -0.3
	ColorCor.TintColor = Color3.fromRGB(255, 235, 203)
	ColorCor.Name = "CustomColorCorrection"
	ColorCor.Parent = Lighting

	local SunRays = Instance.new("SunRaysEffect")
	SunRays.Intensity = 0.075
	SunRays.Spread = 0.727
	SunRays.Name = "CustomSunRays"
	SunRays.Parent = Lighting

	local Sky = Instance.new("Sky")
	Sky.Name = "CustomSky"
	Sky.SkyboxBk = "rbxassetid://151165214"
	Sky.SkyboxDn = "rbxassetid://151165197"
	Sky.SkyboxFt = "rbxassetid://151165224"
	Sky.SkyboxLf = "rbxassetid://151165191"
	Sky.SkyboxRt = "rbxassetid://151165206"
	Sky.SkyboxUp = "rbxassetid://151165227"
	Sky.SunAngularSize = 10
	Sky.Parent = Lighting

	local Atm = Instance.new("Atmosphere")
	Atm.Name = "CustomAtmosphere"
	Atm.Density = 0.364
	Atm.Offset = 0.556
	Atm.Color = Color3.fromRGB(199, 175, 166)
	A
