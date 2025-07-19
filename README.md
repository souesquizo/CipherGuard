local teamCheck = true
local espEnabled = false

local gui = Instance.new("ScreenGui", game:GetService("CoreGui"))
gui.Name = "CipherGuard_UI"


local title = Instance.new("TextLabel", gui)
title.Size = UDim2.new(0, 250, 0, 35)
title.Position = UDim2.new(0, 20, 0, 20)
title.BackgroundTransparency = 1
title.Text = "☠ CipherGuard ☠"
title.Font = Enum.Font.GothamBold
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextStrokeTransparency = 0.4
title.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
title.TextSize = 22

local desc = Instance.new("TextLabel", gui)
desc.Size = UDim2.new(0, 300, 0, 20)
desc.Position = UDim2.new(0, 20, 0, 55)
desc.BackgroundTransparency = 1
desc.Text = "testando essa bst de linguagem lua"
desc.Font = Enum.Font.Gotham
desc.TextColor3 = Color3.fromRGB(200, 200, 200)
desc.TextSize = 14
desc.TextXAlignment = Enum.TextXAlignment.Left

local button = Instance.new("TextButton", gui)
button.Size = UDim2.new(0, 200, 0, 40)
button.Position = UDim2.new(0, 20, 0, 80)
button.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
button.BackgroundTransparency = 0.1
button.TextColor3 = Color3.fromRGB(255, 0, 0)
button.Font = Enum.Font.GothamBold
button.TextSize = 20
button.Text = "ESP: OFF"

local corner = Instance.new("UICorner", button)
corner.CornerRadius = UDim.new(0, 6)

function createESP(player)
	if not player.Character then return end
	if teamCheck and player.Team == game.Players.LocalPlayer.Team then return end
	if player == game.Players.LocalPlayer then return end
	if player.Character:FindFirstChild("CipherESP") then return end

	local head = player.Character:FindFirstChild("Head")
	if not head then return end

	local billboard = Instance.new("BillboardGui")
	billboard.Name = "CipherESP"
	billboard.Adornee = head
	billboard.Size = UDim2.new(0, 100, 0, 40)
	billboard.StudsOffset = Vector3.new(0, 2.5, 0)
	billboard.AlwaysOnTop = true
	billboard.Parent = player.Character

	local frame = Instance.new("Frame", billboard)
	frame.Size = UDim2.new(1, 0, 1, 0)
	frame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	frame.BackgroundTransparency = 0.2
	frame.BorderSizePixel = 0

	local name = Instance.new("TextLabel", billboard)
	name.Size = UDim2.new(1, 0, 1, 0)
	name.Text = player.Name
	name.BackgroundTransparency = 1
	name.TextColor3 = Color3.fromRGB(0, 0, 0)
	name.TextStrokeTransparency = 0
	name.Font = Enum.Font.GothamBold
	name.TextSize = 14
end


function enableESP()
	for _, player in pairs(game.Players:GetPlayers()) do
		createESP(player)
	end
end

function disableESP()
	for _, player in pairs(game.Players:GetPlayers()) do
		if player.Character and player.Character:FindFirstChild("CipherESP") then
			player.Character:FindFirstChild("CipherESP"):Destroy()
		end
	end
end

function toggleESP()
	espEnabled = not espEnabled
	if espEnabled then
		button.Text = "ESP: ON"
		button.TextColor3 = Color3.fromRGB(0, 255, 0)
		enableESP()
	else
		button.Text = "ESP: OFF"
		button.TextColor3 = Color3.fromRGB(255, 0, 0)
		disableESP()
	end
end

button.MouseButton1Click:Connect(toggleESP)

game.Players.PlayerAdded:Connect(function(player)
	player.CharacterAdded:Connect(function()
		wait(1)
		if espEnabled then
			createESP(player)
		end
	end)
end)
