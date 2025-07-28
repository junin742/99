-- Script de voo simples para Roblox (somente para uso em jogos próprios)
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local flying = false
local speed = 50

local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local bodyGyro = Instance.new("BodyGyro")
bodyGyro.MaxTorque = Vector3.new(400000, 400000, 400000)
bodyGyro.P = 100000

local bodyVelocity = Instance.new("BodyVelocity")
bodyVelocity.MaxForce = Vector3.new(400000, 400000, 400000)
bodyVelocity.P = 10000

-- Função para ativar ou desativar o voo
function toggleFly()
	if not flying then
		flying = true
		bodyGyro.Parent = humanoidRootPart
		bodyVelocity.Parent = humanoidRootPart
	else
		flying = false
		bodyGyro.Parent = nil
		bodyVelocity.Parent = nil
	end
end

UIS.InputBegan:Connect(function(input, processed)
	if processed then return end
	if input.KeyCode == Enum.KeyCode.F then -- Aperte "F" para voar
		toggleFly()
	end
end)

RunService.RenderStepped:Connect(function()
	if flying then
		bodyGyro.CFrame = workspace.CurrentCamera.CFrame
		bodyVelocity.Velocity = workspace.CurrentCamera.CFrame.LookVector * speed
	end
end)
