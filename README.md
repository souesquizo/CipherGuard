--// CONFIG
local teamCheck = true
local espEnabled = false
local superJumpEnabled = false
local jumpPowerAmount = 150

--// UI
local gui = Instance.new("ScreenGui", game:GetService("CoreGui"))
gui.Name = "CipherGuardESP_UI"

--// Fundo quadriculado (ImageLabel)
local background = Instance.new("ImageLabel", gui)
background.Size = UDim2.new(0, 240, 0, 160)
background.Position = UDim2.new(0, 20, 0, 20)
background.Image = "rbxassetid://2151741365" -- textura quadriculada leve
background.ImageTransparency = 0.3
background.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
background.BackgroundTransparency = 0
background.BorderSizePixel = 0

-- Arredondamento
local bgCorner = Instance.new("UICorner", background)
bgCorner.CornerRadius = UDim.new(0, 12)

--// Título estilizado
local title = Instance.new("TextLabel", background)
title.Size = UDim2.new(1, 0, 0, 30)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundTransparency = 1
title.Text = "☠ CipherGuard ☠"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextSize = 20
title.TextStrokeTransparency = 0.6

--// Descrição
local desc = Instance.new("TextLabel", background)
desc.Size = UDim2.new(1, -10, 0, 20)
desc.Position = UDim2.new(0, 5, 0, 28)
desc.BackgroundTransparency = 1
desc.Text = "testando essa bst de linguagem lua"
desc.TextColor3 = Color3.fromRGB(200, 200, 200)
desc.Font = Enum.Font.Gotham
desc.TextSize = 13
desc.TextXAlignment = Enum.TextXAlignment.Left
desc.TextStrokeTransparency = 0.8

--// Botão de ESP
local toggleESP = Instance.new("TextButton", background)
toggleESP.Size = UDim2.new(1, -20, 0, 35)
toggleESP.Position = UDim2.new(0, 10, 0, 55)
toggleESP.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
toggleESP.TextColor3 = Color3.fromRGB(255, 0, 0)
toggleESP.Font = Enum.Font.GothamBold
toggleESP.TextSize = 18
toggleESP.Text = "ESP: OFF"
toggleESP.BorderSizePixel = 0
Instance.new("UICorner", toggleESP)

--// Botão de Super Pulo
local toggleJump = Instance.new("TextButton", background)
toggleJump.Size = UDim2.new(1, -20, 0, 35)
toggleJump.Position = UDim2.new(0, 10, 0, 95)
toggleJump.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
toggleJump.TextColor3 = Color3.fromRGB(255, 0, 0)
toggleJump.Font = Enum.Font.GothamBold
toggleJump.TextSize = 18
toggleJump.Text = "Super Pulo: OFF"
toggleJump.BorderSizePixel = 0
Instance.new("UICorner", toggleJump)

--// Função ESP
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

--// Toggle ESP
toggleESP.MouseButton1Click:Connect(function()
	espEnabled = not espEnabled
	if espEnabled then
		toggleESP.Text = "ESP: ON"
		toggleESP.TextColor3 = Color3.fromRGB(0, 255, 0)
		enableESP()
	else
		toggleESP.Text = "ESP: OFF"
		toggleESP.TextColor3 = Color3.fromRGB(255, 0, 0)
		disableESP()
	end
end)

--// Toggle Super Jump
toggleJump.MouseButton1Click:Connect(function()
	superJumpEnabled = not superJumpEnabled
	local human = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
	if superJumpEnabled then
		if human then human.JumpPower = jumpPowerAmount end
		toggleJump.Text = "Super Pulo: ON"
		toggleJump.TextColor3 = Color3.fromRGB(0, 255, 0)
	else
		if human then human.JumpPower = 50 end
		toggleJump.Text = "Super Pulo: OFF"
		toggleJump.TextColor3 = Color3.fromRGB(255, 0, 0)
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
