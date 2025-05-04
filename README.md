-- Script simples para testar ataque e movimentação
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local humanoidRoot = player.Character:WaitForChild("HumanoidRootPart")

local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "TestFarmUI"

local button = Instance.new("TextButton", gui)
button.Size = UDim2.new(0, 180, 0, 40)
button.Position = UDim2.new(0, 0.8, 0, 0)
button.Text = "Ativar AutoFarm"
button.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
button.TextColor3 = Color3.new(1, 1, 1)
button.Font = Enum.Font.SourceSansBold
button.TextSize = 16
Instance.new("UICorner", button)

local ativo = false

local function autoFarm()
	while ativo do
		for _, enemy in pairs(workspace.Enemies:GetChildren()) do
			if enemy:FindFirstChild("HumanoidRootPart") and enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 then
				humanoidRoot.CFrame = enemy.HumanoidRootPart.CFrame + Vector3.new(0, 5, 0)
				while enemy.Humanoid.Health > 0 and ativo do
					local tool = player.Character:FindFirstChildOfClass("Tool")
					if tool then tool:Activate() end
					task.wait(0.2)
				end
			end
		end
		wait(1)
	end
end

button.MouseButton1Click:Connect(function()
	ativo = not ativo
	button.Text = ativo and "Parar Farm" or "Ativar AutoFarm"
	if ativo then
		task.spawn(autoFarm)
	end
end)
