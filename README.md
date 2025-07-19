local speedEnabled = false
local jumpEnabled = false

local walkSpeedAmount = 100
local jumpPowerAmount = 150

local gui = Instance.new("ScreenGui", game:GetService("CoreGui"))
gui.Name = "CipherGuard_UI"

local background = Instance.new("ImageLabel", gui)
background.Size = UDim2.new(0, 240, 0, 160)
background.Position = UDim2.new(0, 20, 0, 20)
background.Image = "rbxassetid://2151741365" 
background.ImageTransparency = 0.3
background.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
background.BackgroundTransparency = 0
background.BorderSizePixel = 0
Instance.new("UICorner", background).CornerRadius = UDim.new(0, 12)

local title = Instance.new("TextLabel", background)
title.Size = UDim2.new(1, 0, 0, 30)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundTransparency = 1
title.Text = "☠ CipherGuard ☠"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextSize = 20
title.TextStrokeTransparency = 0.6

local desc = Instance.new("TextLabel", background)
desc.Size = UDim2.new(1, -10, 0, 20)
desc.Position = UDim2.new(0, 5, 0, 28)
desc.BackgroundTransparency = 1
desc.Text = "testando essa bst de linguagem lua"
desc.TextColor3 = Color3.fromRGB(200, 200, 200)
desc.Font = Enum.Font.Gotham
desc.TextSize = 13
desc.TextXAlignment = Enum.TextXAlignment.Center
desc.TextStrokeTransparency = 0.8

local jumpBtn = Instance.new("TextButton", background)
jumpBtn.Size = UDim2.new(1, -20, 0, 35)
jumpBtn.Position = UDim2.new(0, 10, 0, 55)
jumpBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
jumpBtn.TextColor3 = Color3.fromRGB(255, 0, 0)
jumpBtn.Font = Enum.Font.GothamBold
jumpBtn.TextSize = 18
jumpBtn.Text = "Super Pulo: OFF"
jumpBtn.BorderSizePixel = 0
Instance.new("UICorner", jumpBtn)

local speedBtn = Instance.new("TextButton", background)
speedBtn.Size = UDim2.new(1, -20, 0, 35)
speedBtn.Position = UDim2.new(0, 10, 0, 95)
speedBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
speedBtn.TextColor3 = Color3.fromRGB(255, 0, 0)
speedBtn.Font = Enum.Font.GothamBold
speedBtn.TextSize = 18
speedBtn.Text = "Velocidade: OFF"
speedBtn.BorderSizePixel = 0
Instance.new("UICorner", speedBtn)

-- Toggle button para abrir/fechar o painel
local toggleBtn = Instance.new("TextButton", gui)
toggleBtn.Size = UDim2.new(0, 30, 0, 30)
toggleBtn.Position = UDim2.new(0, 20 + 240 -35, 0, 20, + 5)
toggleBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextSize = 20
toggleBtn.Text = "▲"
toggleBtn.BorderSizePixel = 0
Instance.new("UICorner", toggleBtn)

local isOpen = true

toggleBtn.MouseButton1Click:Connect(function()
    isOpen = not isOpen
    for _, child in pairs(background:GetChildren()) do
        if child ~= toggleBtn then
            child.Visible = isOpen
        end
    end
    toggleBtn.Text = isOpen and "▲" or "▼"
end)

function updateStats()
	local char = game.Players.LocalPlayer.Character or game.Players.LocalPlayer.CharacterAdded:Wait()
	local humanoid = char:FindFirstChildOfClass("Humanoid")
	if not humanoid then return end

	humanoid.JumpPower = jumpEnabled and jumpPowerAmount or 50
	humanoid.WalkSpeed = speedEnabled and walkSpeedAmount or 16
end

jumpBtn.MouseButton1Click:Connect(function()
	jumpEnabled = not jumpEnabled
	updateStats()
	jumpBtn.Text = jumpEnabled and "Super Pulo: ON" or "Super Pulo: OFF"
	jumpBtn.TextColor3 = jumpEnabled and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
end)

speedBtn.MouseButton1Click:Connect(function()
	speedEnabled = not speedEnabled
	updateStats()
	speedBtn.Text = speedEnabled and "Velocidade: ON" or "Velocidade: OFF"
	speedBtn.TextColor3 = speedEnabled and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
end)

game.Players.LocalPlayer.CharacterAdded:Connect(function()
	wait(1)
	updateStats()
end)
