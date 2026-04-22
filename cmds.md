## BUNCH OF COMMANDS

```lua
local Players = game:GetService("Players")

local PREFIX = ";"
local ADMINS = {
	["YamchaJojo"] = true
}

local frozenPlayers = {}

local function isAdmin(player)
	return ADMINS[player.Name] == true
end

local function getPlayer(name)
	name = name:lower()
	for _, player in pairs(Players:GetPlayers()) do
		if player.Name:lower():sub(1, #name) == name then
			return player
		end
	end
end

Players.PlayerAdded:Connect(function(player)
	player.Chatted:Connect(function(msg)
		if not isAdmin(player) then return end
		
		if msg:sub(1,1) == PREFIX then
			local args = msg:sub(2):split(" ")
			local command = args[1]:lower()
			local target = getPlayer(args[2] or "")
			
			if command == "speed" and target then
				local humanoid = target.Character and target.Character:FindFirstChild("Humanoid")
				if humanoid then
					humanoid.WalkSpeed = tonumber(args[3]) or 50
				end
			
			elseif command == "jump" and target then
				local humanoid = target.Character and target.Character:FindFirstChild("Humanoid")
				if humanoid then
					humanoid.JumpPower = tonumber(args[3]) or 100
				end
			
			elseif command == "heal" and target then
				local humanoid = target.Character and target.Character:FindFirstChild("Humanoid")
				if humanoid then
					humanoid.Health = humanoid.MaxHealth
				end
			
			elseif command == "explode" and target then
				local char = target.Character
				if char and char:FindFirstChild("HumanoidRootPart") then
					local explosion = Instance.new("Explosion")
					explosion.Position = char.HumanoidRootPart.Position
					explosion.Parent = workspace
				end
			
			elseif command == "freeze" and target then
				local char = target.Character
				if char then
					for _, part in pairs(char:GetDescendants()) do
						if part:IsA("BasePart") then
							part.Anchored = true
						end
					end
					frozenPlayers[target] = true
				end
			
			elseif command == "unfreeze" and target then
				local char = target.Character
				if char then
					for _, part in pairs(char:GetDescendants()) do
						if part:IsA("BasePart") then
							part.Anchored = false
						end
					end
					frozenPlayers[target] = nil
				end
			end
		end
	end)
end)
```



