-- Aviso: Este script é puramente visual e só funciona no seu cliente.
-- Não altera gráficos reais como RTX, SSR ou Ambient Occlusion.

local Lighting = game:GetService("Lighting")

-- Resetar
Lighting:ClearAllChildren()

-- 🌞 Ativar iluminação futura
Lighting.Technology = Enum.Technology.Future
Lighting.Brightness = 3
Lighting.EnvironmentDiffuseScale = 1
Lighting.EnvironmentSpecularScale = 1
Lighting.GlobalShadows = true
Lighting.OutdoorAmbient = Color3.fromRGB(127, 127, 127)
Lighting.Ambient = Color3.fromRGB(100, 100, 100)

-- 💡 Bloom (efeito de luz suave)
local bloom = Instance.new("BloomEffect")
bloom.Intensity = 1.5
bloom.Size = 56
bloom.Threshold = 0.8
bloom.Parent = Lighting

-- ☀️ Sun Rays
local sunRays = Instance.new("SunRaysEffect")
sunRays.Intensity = 0.15
sunRays.Spread = 0.8
sunRays.Parent = Lighting

-- 🎨 Color Correction (cores vivas)
local colorCorrection = Instance.new("ColorCorrectionEffect")
colorCorrection.Saturation = 0.3
colorCorrection.Contrast = 0.1
colorCorrection.Brightness = 0.05
colorCorrection.TintColor = Color3.fromRGB(255, 240, 230)
colorCorrection.Parent = Lighting

-- 🌫️ Depth of Field
local dof = Instance.new("DepthOfFieldEffect")
dof.FarIntensity = 0.05
dof.FocusDistance = 50
dof.InFocusRadius = 30
dof.NearIntensity = 0.2
dof.Parent = Lighting

-- 🎥 Tonemap (HDR simulado)
local tonemap = Instance.new("ColorCorrectionEffect")
tonemap.Brightness = 0.05
tonemap.Contrast = 0.25
tonemap.Saturation = 0.1
tonemap.TintColor = Color3.fromRGB(240, 240, 255)
tonemap.Parent = Lighting

-- ✔️ Feedback
print("✅ Shader visual aplicado com sucesso (efeitos simulados)")
