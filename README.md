--// CONFIG
local superMode = false
local speedNormal = 16
local jumpNormal = 50
local speedBoost = 100
local jumpBoost = 150

--// GUI
local gui = Instance.new("ScreenGui", game:GetService("CoreGui"))
gui.Name = "CipherBoost_UI"

-- Título
local title = Instance.new("TextLabel", gui)
title.Size = UDim2.new(0, 250, 0, 35)
title.Position = UDim2.new(0, 20, 0, 20)
title.BackgroundTransparency = 1
title.Text = "⚡ CipherGuard ⚡"
title.Font = Enum.Font.GothamBold
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextStrokeTransparency = 0.4
title.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
title.TextSize = 22

-- Descrição
local desc = Instance.new("TextLabel", gui)
desc.Size = UDim2.new(0, 300, 0, 20)
desc.Position = UDim2.new(0, 20, 0, 55)
desc.BackgroundTransparency = 1
desc.Text = "habilitando velocidade e pulo alto"
desc.Font = Enum.Font.Gotham
desc.TextColor3 = Color3.fromRGB(200, 200, 200)
desc.TextSize = 14
desc.TextXAlignment = Enum.TextXAlignment.Left

-- Botão
local button = Instance.new("TextButton", gui)
button.Size = UDim2.new(0, 220, 0, 40)
button.Position = UDim2.new(0, 20, 0, 80)
button.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
button.BackgroundTransparency = 0.1
button.TextColor3 = Color3.fromRGB(255, 0, 0)
button.Font = Enum.Font.GothamBold
button.TextSize = 20
button.Text = "SUPER: OFF"

local corner = Instance.new("UICorner", button)
corner.CornerRadius = UDim.new(0, 6)

--// Função do modo turbo
function toggleBoost()
	local char = game.Players.LocalPlayer.Character
	if not char or not char:FindFirstChild("Humanoid") then return end

	superMode = not superMode
	if superMode then
		button.Text = "SUPER: ON"
		button.TextColor3 = Color3.fromRGB(0, 255, 0)
		char.Humanoid.WalkSpeed = speedBoost
		char.Humanoid.JumpPower = jumpBoost
	else
		button.Text = "SUPER: OFF"
		button.TextColor3 = Color3.fromRGB(255, 0, 0)
		char.Humanoid.WalkSpeed = speedNormal
		char.Humanoid.JumpPower = jumpNormal
	end
end

-- Botão click
button.MouseButton1Click:Connect(toggleBoost)

-- Caso o personagem morra e respawne
game.Players.LocalPlayer.CharacterAdded:Connect(function(char)
	wait(1)
	if superMode and char:FindFirstChild("Humanoid") then
		char.Humanoid.WalkSpeed = speedBoost
		char.Humanoid.JumpPower = jumpBoost
	end
end)
