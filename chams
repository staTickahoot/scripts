local function createNameTag(character, player)
	local head = character:FindFirstChild("Head")
	if not head then return end

	if head:FindFirstChild("NameTag") then return end

	local billboardGui = Instance.new("BillboardGui")
	billboardGui.Name = "NameTag"
	billboardGui.Adornee = head
	billboardGui.AlwaysOnTop = true
	billboardGui.Size = UDim2.new(0, 100, 0, 20)
	billboardGui.StudsOffset = Vector3.new(0, 2, 0)
	billboardGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

	local textLabel = Instance.new("TextLabel")
	textLabel.Size = UDim2.new(1, 0, 1, 0)
	textLabel.BackgroundTransparency = 1
	textLabel.Text = player.Name
	textLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
	textLabel.TextStrokeTransparency = 1
	textLabel.Font = Enum.Font.SourceSans
	textLabel.TextSize = 14
	textLabel.Parent = billboardGui

	billboardGui.Parent = head
end

local function removeNameTag(character)
	local head = character:FindFirstChild("Head")
	if head then
		local nameTag = head:FindFirstChild("NameTag")
		if nameTag then
			nameTag:Destroy()
		end
	end
end

local function highlightPlayer(character, isLocalPlayer, player)
	local highlight = character:FindFirstChild("Highlight")

	local humanoid = character:FindFirstChildOfClass("Humanoid")
	if humanoid then
		humanoid.DisplayDistanceType = Enum.HumanoidDisplayDistanceType.None
	end

	if not highlight and not isLocalPlayer then
		highlight = Instance.new("Highlight")
		highlight.Parent = character
		highlight.OutlineColor = Color3.fromRGB(255, 0, 0)
		highlight.FillTransparency = 0.75
		highlight.ZIndex = 10
	elseif highlight and isLocalPlayer then
		highlight:Destroy()
	end

	if not isLocalPlayer then
		createNameTag(character, player)
	else
		removeNameTag(character)
	end
end

local function updateHighlights()
	local players = game.Players:GetPlayers()
	for _, player in ipairs(players) do
		local character = player.Character
		if character then
			highlightPlayer(character, player == game.Players.LocalPlayer, player)
		end
	end
end

local function onPlayerAdded(player)
	player.CharacterAdded:Connect(function(character)
		highlightPlayer(character, player == game.Players.LocalPlayer, player)
	end)
end

local function onPlayerRemoving(player)
	local character = player.Character
	if character then
		local highlight = character:FindFirstChild("Highlight")
		if highlight then
			highlight:Destroy()
		end
		removeNameTag(character)
	end
end

game.Players.PlayerAdded:Connect(onPlayerAdded)
game.Players.PlayerRemoving:Connect(onPlayerRemoving)

game:GetService("RunService").Heartbeat:Connect(updateHighlights)
