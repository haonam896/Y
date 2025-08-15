--// Knife client script (equip-on-key + swing)
local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local player = Players.LocalPlayer
local Tool = script.Parent
local SwingRE = Tool:WaitForChild("Swing")

local COOLDOWN = 0.5
local isCooling = false
local humanoid
local track

-- Thay AnimationId
local swingAnim = Instance.new("Animation")
swingAnim.AnimationId = "rbxassetid://507777826" -- placeholder

local function onEquipped()
	local char = player.Character
	if not char then return end
	humanoid = char:FindFirstChildOfClass("Humanoid")
	if humanoid then
		track = track or humanoid:LoadAnimation(swingAnim)
	end
end

local function swing()
	if isCooling then return end
	isCooling = true
	if track then track:Play(0.05, 1, 1) end
	SwingRE:FireServer()
	task.delay(COOLDOWN, function()
		isCooling = false
	end)
end

Tool.Equipped:Connect(onEquipped)
Tool.Activated:Connect(swing)

-- Ấn phím F để trang bị dao
UIS.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.F then
		local char = player.Character
		if not char then return end
		local hum = char:FindFirstChildOfClass("Humanoid")
		if hum and Tool.Parent == player.Backpack then
			hum:EquipTool(Tool)
		end
	end
end)
