-- HouseCatHub Final - Feito por ChatGPT

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RS = game:GetService("RunService")
local player = Players.LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "HouseCatHubUI"
gui.ResetOnSpawn = false

local mainButton = Instance.new("TextButton", gui)
mainButton.Text = "😸"
mainButton.Size = UDim2.new(0, 50, 0, 50)
mainButton.Position = UDim2.new(0, 10, 1, -60)
mainButton.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
mainButton.TextScaled = true
mainButton.Draggable = true
mainButton.Active = true

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 400, 0, 300)
frame.Position = UDim2.new(0.5, -200, 0.5, -150)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.Visible = false

local tabs = Instance.new("Frame", frame)
tabs.Size = UDim2.new(1, 0, 0, 40)
tabs.BackgroundColor3 = Color3.fromRGB(60, 60, 60)

local mainTab = Instance.new("TextButton", tabs)
mainTab.Size = UDim2.new(0.5, 0, 1, 0)
mainTab.Text = "Main"
mainTab.BackgroundColor3 = Color3.fromRGB(80, 80, 80)

local aurasTab = Instance.new("TextButton", tabs)
aurasTab.Size = UDim2.new(0.5, 0, 1, 0)
aurasTab.Position = UDim2.new(0.5, 0, 0, 0)
aurasTab.Text = "Auras"
aurasTab.BackgroundColor3 = Color3.fromRGB(80, 80, 80)

local function createScrollArea(parent)
	local scroll = Instance.new("ScrollingFrame", parent)
	scroll.Size = UDim2.new(1, 0, 1, -40)
	scroll.Position = UDim2.new(0, 0, 0, 40)
	scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
	scroll.ScrollBarThickness = 8
	scroll.BackgroundTransparency = 1
	scroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
	scroll.AutomaticSize = Enum.AutomaticSize.None

	local layout = Instance.new("UIListLayout", scroll)
	layout.Padding = UDim.new(0, 5)
	layout.SortOrder = Enum.SortOrder.LayoutOrder

	return scroll
end

local mainContent = createScrollArea(frame)
local aurasContent = createScrollArea(frame)
aurasContent.Visible = false

local ActiveScripts = {}

local function createToggle(parent, name, url, scriptId)
	local container = Instance.new("Frame", parent)
	container.Size = UDim2.new(1, -10, 0, 50)
	container.BackgroundTransparency = 1

	local label = Instance.new("TextLabel", container)
	label.Size = UDim2.new(1, -160, 1, 0)
	label.Text = name
	label.TextColor3 = Color3.new(1, 1, 1)
	label.BackgroundTransparency = 1
	label.TextXAlignment = Enum.TextXAlignment.Left

	local onBtn = Instance.new("TextButton", container)
	onBtn.Size = UDim2.new(0, 60, 0, 30)
	onBtn.Position = UDim2.new(1, -130, 0.5, -15)
	onBtn.Text = "Ligar"
	onBtn.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
	onBtn.TextColor3 = Color3.new(1,1,1)

	local offBtn = Instance.new("TextButton", container)
	offBtn.Size = UDim2.new(0, 60, 0, 30)
	offBtn.Position = UDim2.new(1, -65, 0.5, -15)
	offBtn.Text = "Desligar"
	offBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
	offBtn.TextColor3 = Color3.new(1,1,1)

	onBtn.MouseButton1Click:Connect(function()
		if scriptId == "infjump" then
			ActiveScripts["infjump"] = UIS.JumpRequest:Connect(function()
				local h = player.Character and player.Character:FindFirstChildWhichIsA("Humanoid")
				if h then h:ChangeState(Enum.HumanoidStateType.Jumping) end
			end)
		elseif scriptId == "noclip" then
			ActiveScripts["noclip"] = RS.Stepped:Connect(function()
				local char = player.Character
				if char then
					for _, part in pairs(char:GetDescendants()) do
						if part:IsA("BasePart") then
							part.CanCollide = false
						end
					end
				end
			end)
		elseif scriptId == "esp" then
			loadstring(game:HttpGet(url))()
		else
			pcall(function() loadstring(game:HttpGet(url))() end)
		end
	end)

	offBtn.MouseButton1Click:Connect(function()
		if scriptId == "infjump" and ActiveScripts["infjump"] then
			ActiveScripts["infjump"]:Disconnect()
			ActiveScripts["infjump"] = nil
		elseif scriptId == "noclip" and ActiveScripts["noclip"] then
			ActiveScripts["noclip"]:Disconnect()
			ActiveScripts["noclip"] = nil
			local char = player.Character
			if char then
				for _, part in pairs(char:GetDescendants()) do
					if part:IsA("BasePart") then
						part.CanCollide = true
					end
				end
			end
		elseif scriptId == "esp" then
			for _, v in pairs(workspace:GetDescendants()) do
				if v:IsA("BoxHandleAdornment") and v.Name == "ESP_BOX" then
					v:Destroy()
				end
			end
		end
	end)
end

local function createExecButton(parent, name, url)
	local btn = Instance.new("TextButton", parent)
	btn.Size = UDim2.new(1, -20, 0, 40)
	btn.Text = name
	btn.BackgroundColor3 = Color3.fromRGB(100, 100, 255)
	btn.TextColor3 = Color3.new(1,1,1)
	btn.Position = UDim2.new(0, 10, 0, 0)
	btn.AutoButtonColor = true
	btn.MouseButton1Click:Connect(function()
		pcall(function()
			loadstring(game:HttpGet(url))()
		end)
	end)
end

-- Main
createToggle(mainContent, "ESP Players", "https://raw.githubusercontent.com/Sansofc/Esp-players/refs/heads/main/README.md", "esp")
createExecButton(mainContent, "Fly Test", "https://github.com/Sinister-Scripts/Roblox-Exploits/raw/refs/heads/main/FE-Animated-Mobile-Fly")
createToggle(mainContent, "Inf Jump", "https://raw.githubusercontent.com/Sansofc/Inf-jump/refs/heads/main/README.md", "infjump")
createToggle(mainContent, "Noclip", "https://raw.githubusercontent.com/Sansofc/Noclip/refs/heads/main/README.md", "noclip")

-- Auras
createExecButton(aurasContent, "Loki", "https://raw.githubusercontent.com/Sansofc/Loki/refs/heads/main/README.md")
createExecButton(aurasContent, "Ender", "https://raw.githubusercontent.com/Sansofc/Ender-TP/refs/heads/main/README.md")

mainTab.MouseButton1Click:Connect(function()
	mainContent.Visible = true
	aurasContent.Visible = false
end)

aurasTab.MouseButton1Click:Connect(function()
	mainContent.Visible = false
	aurasContent.Visible = true
end)

mainButton.MouseButton1Click:Connect(function()
	frame.Visible = not frame.Visible
end)
