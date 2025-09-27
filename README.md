-- ScriptTesteScript1

-- Criar GUI
local ScreenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 220, 0, 120)
Frame.Position = UDim2.new(0.3, 0, 0.3, 0)
Frame.BackgroundColor3 = Color3.fromRGB(30,30,30)
Frame.Active = true
Frame.Draggable = true

-- Barra de título
local TitleBar = Instance.new("Frame", Frame)
TitleBar.Size = UDim2.new(1, 0, 0, 30)
TitleBar.Position = UDim2.new(0, 0, 0, 0)
TitleBar.BackgroundColor3 = Color3.fromRGB(50,50,50)

local Title = Instance.new("TextLabel", TitleBar)
Title.Size = UDim2.new(0.8, 0, 1, 0)
Title.Position = UDim2.new(0, 5, 0, 0)
Title.Text = "Plataforma Dszin Calva"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.BackgroundTransparency = 1
Title.TextScaled = true
Title.TextXAlignment = Enum.TextXAlignment.Left

-- Botão MINIMIZAR
local MinButton = Instance.new("TextButton", TitleBar)
MinButton.Size = UDim2.new(0.2, -5, 1, -5)
MinButton.Position = UDim2.new(0.8, 0, 0, 0)
MinButton.Text = "_"
MinButton.TextColor3 = Color3.fromRGB(255, 255, 0)
MinButton.BackgroundColor3 = Color3.fromRGB(80, 80, 0)
MinButton.TextScaled = true

-- Botão SUBIR
local UpButton = Instance.new("TextButton", Frame)
UpButton.Size = UDim2.new(0, 90, 0, 40)
UpButton.Position = UDim2.new(0.1, 0, 0.5, 0)
UpButton.Text = "Subir"

-- Botão DESCER
local DownButton = Instance.new("TextButton", Frame)
DownButton.Size = UDim2.new(0, 90, 0, 40)
DownButton.Position = UDim2.new(0.55, 0, 0.5, 0)
DownButton.Text = "Descer"

-- Função minimizar/maximizar
local minimized = false
MinButton.MouseButton1Click:Connect(function()
	if minimized then
		Frame.Size = UDim2.new(0, 220, 0, 120)
		UpButton.Visible = true
		DownButton.Visible = true
		minimized = false
	else
		Frame.Size = UDim2.new(0, 220, 0, 30)
		UpButton.Visible = false
		DownButton.Visible = false
		minimized = true
	end
end)

-- Plataforma
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoidRoot = char:WaitForChild("HumanoidRootPart")

local platform = Instance.new("Part")
platform.Size = Vector3.new(6,1,6)
platform.Anchored = true
platform.Color = Color3.fromRGB(128, 0, 128) -- Roxo
platform.Position = humanoidRoot.Position - Vector3.new(0,3,0)
platform.Parent = workspace

-- Altura relativa
local offsetY = -3
local step = 5

UpButton.MouseButton1Click:Connect(function()
	offsetY = offsetY + step
end)
DownButton.MouseButton1Click:Connect(function()
	offsetY = offsetY - step
end)

-- Seguir player
game:GetService("RunService").Heartbeat:Connect(function()
	if char and humanoidRoot then
		platform.Position = humanoidRoot.Position + Vector3.new(0, offsetY, 0)
	end
end)

-- Reset quando renascer
player.CharacterAdded:Connect(function(newChar)
	char = newChar
	humanoidRoot = char:WaitForChild("HumanoidRootPart")
	offsetY = -3
end)
