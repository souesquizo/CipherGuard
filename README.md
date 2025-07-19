--// CONFIG
local teamCheck = true
local espEnabled = false

--// UI
local gui = Instance.new("ScreenGui", game:GetService("CoreGui"))
gui.Name = "CipherGuardESP_UI"

--// Título estilizado
local title = Instance.new("TextLabel", gui)
title.Size = UDim2.new(0, 200, 0, 40)
title.Position = UDim2.new(0, 20, 0, 20)
title.BackgroundTransparency = 1
title.Text = "☠ CipherGuard ☠"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextSize = 22
title.TextStrokeTransparency = 0.6
title.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)

--// Descrição
local desc = Instance.new("TextLabel", gui)
desc.Size = UDim2.new(0, 300, 0, 20)
desc.Position = UDim2.new(0, 20, 0, 50)
desc.BackgroundTransparency = 1
desc.Text = "testando essa bst de linguagem lua"
desc.TextColor3 = Color3.fromRGB(200, 200, 200)
desc.Font = Enum.Font.Gotham
desc.TextSize = 14
desc.TextXAlignment = Enum.TextXAlignment.Left
desc.TextStrokeTransparency = 0.8

--// Botão de ativar/desativar
local toggle = Instance.new("TextButton", gui)
toggle.Size = UDim2.new(0, 200, 0, 40)
toggle.Position = UDim2.new(0, 20, 0, 75)
toggle.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
toggle.BackgroundTransparency = 0.2
toggle.TextColor3 = Color3.fromRGB(255, 0, 0)
toggle.Font = Enum.Font.GothamBold
toggle.TextSize = 20
toggle.Text = "ESP: OFF"
toggle.BorderSizePixel = 0
toggle.AutoButtonColor = false
toggle.ZIndex = 2
toggle.ClipsDescendants = true
toggle.TextStrokeTransparency = 0.6
toggle.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
toggle.UICorner = Instance.new("UICorner", toggle)

--// Criar ESP
function createESP(player)
	if player == game.Players.LocalPlayer then return end
	if teamCheck and player.Team == game.Players.LocalPlayer.Team then return end
	if not player.Character or not player.Character:FindFirstChild("Head") then return end
	if player.Character:FindFirstChild("CipherESP_GUI") then return end

	local billboard = Instance.new("BillboardGui")
	billboard.Name = "CipherESP_GUI"
	billboard.Adornee = player.Character.Head
	billboard.Size = UDim2.new(0, 100, 0, 40)
	billboard.StudsOffset = Vector3.new(0, 2, 0)
	billboard.AlwaysOnTop = true
	billboard.Parent = player.Character

	local frame = Instance.new("Frame", billboard)
	frame.Size = UDim2.new(1, 0, 1, 0)
	frame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	frame.BackgroundTransparency = 0.3
	frame.BorderSizePixel = 0

	local label = Instance.new("TextLabel", billboard)
	label.Size = UDim2.new(1, 0, 1, 0)
	label.Text = player.Name
	label.BackgroundTransparency = 1
	label.TextColor3 = Color3.fromRGB(0, 0, 0)
	label.TextStrokeTransparency = 0
	label.TextSize = 14
	label.Font = Enum.Font.GothamBold
end

--// Ativar/Desativar ESP
function enableESP()
	for _, player in ipairs(game.Players:GetPlayers()) do
		createESP(player)
	end
end

function disableESP()
	for _, player in ipairs(game.Players:GetPlayers()) do
		if player.Character and player.Character:FindFirstChild("CipherESP_GUI") then
			player.Character:FindFirstChild("CipherESP_GUI"):Destroy()
		end
	end
end

--// Toggle
toggle.MouseButton1Click:Connect(function()
	espEnabled = not espEnabled
	if espEnabled then
		toggle.Text = "ESP: ON"
		toggle.TextColor3 = Color3.fromRGB(0, 255, 0)
		enableESP()
	else
		toggle.Text = "ESP: OFF"
		toggle.TextColor3 = Color3.fromRGB(255, 0, 0)
		disableESP()
	end
end)

--// Novos jogadores
game.Players.PlayerAdded:Connect(function(player)
	player.CharacterAdded:Connect(function()
		wait(1)
		if espEnabled then
			createESP(player)
		end
	end)
end)
