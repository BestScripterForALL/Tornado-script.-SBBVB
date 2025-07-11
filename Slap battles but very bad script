local plr = game:GetService("Players").LocalPlayer
local UIS = game:GetService("UserInputService")
local RS = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

local gui = Instance.new("ScreenGui", plr:WaitForChild("PlayerGui"))
gui.ResetOnSpawn = false

local toggleBtn = Instance.new("TextButton", gui)
toggleBtn.Size = UDim2.fromOffset(50, 50)
toggleBtn.Position = UDim2.new(0, 10, .5, -25)
toggleBtn.Text = "Т"
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextScaled = true
toggleBtn.BackgroundColor3 = Color3.fromRGB(180, 255, 180)
Instance.new("UICorner", toggleBtn).CornerRadius = UDim.new(1, 0)

local menu = Instance.new("Frame", gui)
menu.Size = UDim2.fromOffset(200, 180)
menu.Position = UDim2.new(.5, -100, .5, -90)
menu.BackgroundColor3 = Color3.fromRGB(255, 220, 220)
menu.Visible = false
Instance.new("UICorner", menu).CornerRadius = UDim.new(0, 24)

local function makeRow(name, index)
	local row = Instance.new("TextLabel", menu)
	row.Size = UDim2.new(.7, 0, 0, 40)
	row.Position = UDim2.new(0, 10, 0, (index - 1) * 60)
	row.BackgroundTransparency = 1
	row.Font = Enum.Font.GothamBold
	row.TextScaled = true
	row.Text = name
	row.TextColor3 = Color3.new()

	local btn = Instance.new("TextButton", menu)
	btn.Size = UDim2.fromOffset(40, 40)
	btn.Position = UDim2.new(1, -60, 0, (index - 1) * 60)
	btn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
	btn.BorderSizePixel = 0
	Instance.new("UICorner", btn).CornerRadius = UDim.new(1, 0)

	return btn
end

local autoBtn = makeRow("AUTOFARM", 1)
local killBtn = makeRow("KILLAURA", 2)
local espBtn = makeRow("ESP", 3)

local autoOn, killOn, espOn = false, false, false

toggleBtn.MouseButton1Click:Connect(function()
	menu.Visible = not menu.Visible
end)

autoBtn.MouseButton1Click:Connect(function()
	autoOn = not autoOn
	autoBtn.BackgroundColor3 = autoOn and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
end)

killBtn.MouseButton1Click:Connect(function()
	killOn = not killOn
	killBtn.BackgroundColor3 = killOn and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
end)

espBtn.MouseButton1Click:Connect(function()
	espOn = not espOn
	espBtn.BackgroundColor3 = espOn and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
	for _,v in ipairs(workspace:GetDescendants()) do
		if v.Name == "ESP_HIGHLIGHT" then v:Destroy() end
	end
end)

local function getSlapples()
	local root = workspace:FindFirstChild("Arena") and workspace.Arena:FindFirstChild("island4") and workspace.Arena.island4:FindFirstChild("Slapples")
	if not root then return {} end
	local t = {}
	for _,m in ipairs(root:GetChildren()) do
		if m:IsA("Model") and (m.Name=="Slapple" or m.Name=="GoldenSlapple" or m.Name=="MelonSlapple") then
			local g = m:FindFirstChild("Glove")
			if g and g:IsA("BasePart") and g.Transparency < 1 then table.insert(t, g) end
		end
	end
	return t
end

task.spawn(function()
	while true do
		task.wait(1)
		if autoOn and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
			local list = getSlapples()
			if #list > 0 then
				local g = list[math.random(#list)]
				plr.Character.HumanoidRootPart.CFrame = CFrame.new(g.Position + Vector3.new(0, 3, 0))
			end
		end
	end
end)

task.spawn(function()
	while true do
		task.wait(3)
		if killOn and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
			local root = plr.Character.HumanoidRootPart
			for _, p in ipairs(game:GetService("Players"):GetPlayers()) do
				if p ~= plr and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
					local trg = p.Character.HumanoidRootPart
					if (trg.Position - root.Position).Magnitude <= 100 then
						local behind = trg.Position - trg.CFrame.LookVector * 2 + Vector3.new(0, 1.5, 0)
						root.CFrame = CFrame.new(behind, trg.Position)
						local tool = plr.Character:FindFirstChildOfClass("Tool")
						if tool then tool:Activate() end
						break
					end
				end
			end
		end
	end
end)

local function highlightChar(char)
	if char:FindFirstChild("ESP_HIGHLIGHT") then return end

	local h = Instance.new("Highlight", char)
	h.Name = "ESP_HIGHLIGHT"
	h.FillTransparency = 0.5
	h.OutlineTransparency = 1
	h.FillColor = Color3.fromRGB(255, 0, 0)

	local tween = TweenService:Create(h, TweenInfo.new(1, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut, -1, true), {
		FillColor = Color3.fromRGB(255, 100, 150)
	})
	tween:Play()
end

RS.RenderStepped:Connect(function()
	if espOn then
		for _, p in ipairs(game:GetService("Players"):GetPlayers()) do
			if p ~= plr and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
				highlightChar(p.Character)
			end
		end
	end
end)
