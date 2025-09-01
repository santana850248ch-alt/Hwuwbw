-- Script base para Fly e Noclip
-- Coloque em StarterPlayerScripts

-- Configurações
local flySpeed = 50
local flying = false
local noclip = false

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local userInputService = game:GetService("UserInputService")
local runService = game:GetService("RunService")

-- Função de Fly
local function toggleFly()
	flying = not flying
	if flying then
		print("Fly Ativado")
	else
		print("Fly Desativado")
	end
end

-- Função de Noclip
local function toggleNoclip()
	noclip = not noclip
	if noclip then
		print("Noclip Ativado")
	else
		print("Noclip Desativado")
	end
end

-- Detectar teclas
userInputService.InputBegan:Connect(function(input, isTyping)
	if isTyping then return end
	if input.KeyCode == Enum.KeyCode.F then
		toggleFly()
	elseif input.KeyCode == Enum.KeyCode.N then
		toggleNoclip()
	end
end)

-- Atualizar movimentos
runService.RenderStepped:Connect(function()
	if flying then
		local moveDirection = humanoidRootPart.CFrame.LookVector
		if userInputService:IsKeyDown(Enum.KeyCode.W) then
			humanoidRootPart.Velocity = moveDirection * flySpeed
		elseif userInputService:IsKeyDown(Enum.KeyCode.S) then
			humanoidRootPart.Velocity = -moveDirection * flySpeed
		else
			humanoidRootPart.Velocity = Vector3.new(0,0,0)
		end
	end

	if noclip then
		for _, part in pairs(character:GetDescendants()) do
			if part:IsA("BasePart") and part.CanCollide == true then
				part.CanCollide = false
			end
		end
	else
		for _, part in pairs(character:GetDescendants()) do
			if part:IsA("BasePart") and part.CanCollide == false then
				part.CanCollide = true
			end
		end
	end
end)# Hwuwbw
