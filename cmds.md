## BUNCH OF COMMANDS

```lua
local Players = game:GetService("Players")

local PREFIX = ";"
local ADMINS = {
	["YamchaJojo"] = true,
	["Oatmeai_gaming"] = true,
	["Breiwr"] = true,
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
			
			-- SPEED
			if command == "speed" and target then
				local humanoid = target.Character and target.Character:FindFirstChild("Humanoid")
				if humanoid then
					humanoid.WalkSpeed = tonumber(args[3]) or 50
				end
			
			-- JUMP
			elseif command == "jump" and target then
				local humanoid = target.Character and target.Character:FindFirstChild("Humanoid")
				if humanoid then
					humanoid.JumpPower = tonumber(args[3]) or 100
				end
			
			-- HEAL
			elseif command == "heal" and target then
				local humanoid = target.Character and target.Character:FindFirstChild("Humanoid")
				if humanoid then
					humanoid.Health = humanoid.MaxHealth
				end
			
			-- KILL
			elseif command == "kill" and target then
				local humanoid = target.Character and target.Character:FindFirstChild("Humanoid")
				if humanoid then
					humanoid.Health = 0
				end
			
			-- EXPLODE
			elseif command == "explode" and target then
				local char = target.Character
				if char and char:FindFirstChild("HumanoidRootPart") then
					local explosion = Instance.new("Explosion")
					explosion.Position = char.HumanoidRootPart.Position
					explosion.Parent = workspace
				end
			
			-- BRING
			elseif command == "bring" and target then
				if player.Character and target.Character then
					target.Character:PivotTo(player.Character:GetPivot())
				end
			
			-- GOTO
			elseif command == "goto" and target then
				if player.Character and target.Character then
					player.Character:PivotTo(target.Character:GetPivot())
				end
			
			-- SIT
			elseif command == "sit" and target then
				local humanoid = target.Character and target.Character:FindFirstChild("Humanoid")
				if humanoid then
					humanoid.Sit = true
				end
			
			-- UNSIT
			elseif command == "unsit" and target then
				local humanoid = target.Character and target.Character:FindFirstChild("Humanoid")
				if humanoid then
					humanoid.Sit = false
				end
			
			-- INVISIBLE
			elseif command == "invisible" and target then
				for _, part in pairs(target.Character:GetDescendants()) do
					if part:IsA("BasePart") or part:IsA("Decal") then
						part.Transparency = 1
					end
				end
			
			-- VISIBLE
			elseif command == "visible" and target then
				for _, part in pairs(target.Character:GetDescendants()) do
					if part:IsA("BasePart") or part:IsA("Decal") then
						part.Transparency = 0
					end
				end
			
			-- FORCEFIELD
			elseif command == "ff" and target then
				if target.Character then
					local ff = Instance.new("ForceField")
					ff.Parent = target.Character
				end
			
			-- REMOVE FORCEFIELD
			elseif command == "unff" and target then
				if target.Character then
					for _, v in pairs(target.Character:GetChildren()) do
						if v:IsA("ForceField") then
							v:Destroy()
						end
					end
				end
			
			-- FREEZE
			elseif command == "freeze" and target then
				for _, part in pairs(target.Character:GetDescendants()) do
					if part:IsA("BasePart") then
						part.Anchored = true
					end
				end
				frozenPlayers[target] = true
			
			-- UNFREEZE
			elseif command == "unfreeze" and target then
				for _, part in pairs(target.Character:GetDescendants()) do
					if part:IsA("BasePart") then
						part.Anchored = false
					end
				end
				frozenPlayers[target] = nil
			
			-- GRAVITY
			elseif command == "gravity" then
				workspace.Gravity = tonumber(args[2]) or 196.2
			end
		end
	end)
end)
```



