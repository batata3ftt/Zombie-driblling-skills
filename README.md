local Players = game:GetService("Players")
local player = Players.LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")

local gui = Instance.new("ScreenGui")
gui.Name = "ZombieDribbleGui"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

local cooldownActive = false
local COOLDOWN_TIME = 20

local imageLabel = Instance.new("ImageLabel")
imageLabel.Size = UDim2.fromScale(1, 1)
imageLabel.Position = UDim2.fromScale(0, -0.1)
imageLabel.BackgroundTransparency = 1
imageLabel.Image = "rbxassetid://114625951541411"
imageLabel.ImageTransparency = 1
imageLabel.ZIndex = 5
imageLabel.Visible = false
imageLabel.Parent = gui

local button = Instance.new("ImageButton")
button.Position = UDim2.fromScale(0.405, 0.769)
button.Size = UDim2.fromOffset(35, 35)
button.BackgroundTransparency = 1
button.ImageTransparency = 1
button.Image = ""
button.AutoButtonColor = false
button.ZIndex = 10
button.Parent = gui

local sound = Instance.new("Sound")
sound.SoundId = "rbxassetid://83092494896347"
sound.Volume = 50
sound.RollOffMaxDistance = 0
sound.Parent = workspace

local function fadeIn(img, time)
    img.Visible = true
    img.ImageTransparency = 1
    TweenService:Create(img, TweenInfo.new(time), {ImageTransparency = 0}):Play()
end

local function fadeOut(img, time)
    TweenService:Create(img, TweenInfo.new(time), {ImageTransparency = 1}):Play()
    task.delay(time, function()
        img.Visible = false
    end)
end

local function doDash()
    local character = player.Character
    if not character then return end
    local hrp = character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end

    local forward = hrp.CFrame.LookVector
    local dashForce = Instance.new("BodyVelocity")
    dashForce.Velocity = forward * 270
    dashForce.MaxForce = Vector3.new(1e6, 0, 1e6)
    dashForce.Parent = hrp

    task.wait(0.2)
    dashForce:Destroy()
end

local function startCooldown()
    cooldownActive = true
    task.wait(COOLDOWN_TIME)
    cooldownActive = false
end

button.MouseButton1Click:Connect(function()
    if cooldownActive then return end

    pcall(function()
        local args = {
            "ZOMBIE DRIBBLING",
            "Skill1",
            {
                pitch = -0.7281585312574937,
                lookVector = vector.create(0.7211452722549438, -0.665496289730072, 0.1925206184387207)
            }
        }
        ReplicatedStorage:WaitForChild("SkillActivated"):FireServer(unpack(args))
    end)

    sound:Play()
    doDash()

    fadeIn(imageLabel, 0.8)
    task.delay(0.8, function()
        fadeOut(imageLabel, 0.8)
    end)

    task.spawn(startCooldown)
end)

print("✅ Zombie Dribbling carregado!")local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Remote
local remote = ReplicatedStorage:WaitForChild("SkillActivated")

-- CONFIG
local COOLDOWN_TIME = 20 -- 🔥 botão agora 20 segundos
local IMAGE_ID = "rbxassetid://96468463607962"
local IMAGE_POSITION = UDim2.fromScale(-0.000, -0.028)
local IMAGE_SIZE = UDim2.fromScale(1, 1)
local FADE_TIME = 0.60 -- 🔥 fade agora 0.60
local SHOW_TIME = 0.6 -- tempo antes de começar sumir

-- GUI
local gui = Instance.new("ScreenGui")
gui.Name = "ZombieSkill4Gui"
gui.Parent = playerGui
gui.ResetOnSpawn = false

-- Imagem
local image = Instance.new("ImageLabel")
image.Parent = gui
image.BackgroundTransparency = 1
image.Image = IMAGE_ID
image.Position = IMAGE_POSITION
image.Size = IMAGE_SIZE
image.Visible = false
image.ImageTransparency = 1

-- 🔊 SOM
local sound = Instance.new("Sound")
sound.SoundId = "rbxassetid://114020174103520"
sound.Volume = 50
sound.RollOffMaxDistance = 0
sound.Parent = workspace

-- 🔲 BOTÃO (mesma posição da Skill4 Emperor)
local button = Instance.new("ImageButton")
button.Parent = gui
button.Position = UDim2.fromScale(0.553, 0.769)
button.Size = UDim2.fromOffset(35, 35)
button.BackgroundTransparency = 1
button.ImageTransparency = 1
button.AutoButtonColor = false

-- Cooldown
local onCooldown = false

-- Fade function
local function fade(img, transparency, time)
	TweenService:Create(
		img,
		TweenInfo.new(time, Enum.EasingStyle.Linear),
		{ImageTransparency = transparency}
	):Play()
end

local function showImage()
	image.Visible = true
	
	-- Fade IN
	fade(image, 0, FADE_TIME)

	task.delay(SHOW_TIME, function()
		-- Fade OUT
		fade(image, 1, FADE_TIME)
		
		task.delay(FADE_TIME, function()
			image.Visible = false
		end)
	end)
end

-- Clique
button.MouseButton1Click:Connect(function()
	if onCooldown then return end
	onCooldown = true

	-- 🔥 Remote Event
	local args = {
		"ZOMBIE DRIBBLING",
		"Skill4",
		{
			pitch = -0.7631947223756257,
			lookVector = Vector3.new(
				-0.6950508952140808,
				-0.6912335753440857,
				-0.1977384090423584
			)
		}
	}

	remote:FireServer(unpack(args))

	-- 🔊 Toca o áudio
	sound:Play()

	-- Visual
	showImage()

	-- Cooldown do botão (20s)
	task.delay(COOLDOWN_TIME, function()
		onCooldown = false
	end)
end)

print("✅ Zombie Skill4 carregada | 20s cooldown | som integrado")local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local skillEvent = ReplicatedStorage:WaitForChild("SkillActivated")

-- GUI
local gui = Instance.new("ScreenGui")
gui.Name = "Skill3InvisibleButton"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

-- 🔲 BOTÃO INVISÍVEL (permanece invisível)
local button = Instance.new("ImageButton")
button.Parent = gui
button.Position = UDim2.fromScale(0.503, 0.766)
button.Size = UDim2.fromOffset(36, 36)
button.BackgroundTransparency = 1
button.ImageTransparency = 1
button.AutoButtonColor = false

-- 🖼 IMAGEM QUE APARECE NA SKILL
local skillImage = Instance.new("ImageLabel")
skillImage.Parent = gui
skillImage.Position = UDim2.fromScale(0.005, -0.092)
skillImage.Size = UDim2.fromScale(1, 1)
skillImage.BackgroundTransparency = 1
skillImage.Image = "rbxassetid://139200942081122"
skillImage.ImageTransparency = 1 -- começa invisível
skillImage.Visible = true
skillImage.ZIndex = 10

-- 🔊 SOM
local sound = Instance.new("Sound")
sound.SoundId = "rbxassetid://137822897777384"
sound.Volume = 1
sound.Parent = gui

-- COOLDOWN
local COOLDOWN_TIME = 30
local onCooldown = false

-- CONFIG
local delay1 = 0.20
local delay2 = 1.10
local delay3 = 1.00

local dashDistance = 35
local lastDashExtra = 8
local dashSteps = 12

-- DASH
local function forcedDash(direction, customDistance)
	local char = player.Character
	if not char then return end
	
	local hrp = char:FindFirstChild("HumanoidRootPart")
	if not hrp then return end
	
	direction = direction.Unit
	local distance = customDistance or dashDistance
	
	local startPos = hrp.Position
	local targetPos = startPos + (direction * distance)
	
	for i = 1, dashSteps do
		local newPos = startPos:Lerp(targetPos, i / dashSteps)
		hrp.CFrame = CFrame.new(newPos, newPos + direction)
		task.wait(0.01)
	end
end

-- 🎬 FUNÇÃO FADE
local function playImageEffect()
	local fadeIn = TweenService:Create(
		skillImage,
		TweenInfo.new(0.67, Enum.EasingStyle.Quad, Enum.EasingDirection.Out),
		{ImageTransparency = 0}
	)

	local fadeOut = TweenService:Create(
		skillImage,
		TweenInfo.new(0.67, Enum.EasingStyle.Quad, Enum.EasingDirection.In),
		{ImageTransparency = 1}
	)

	fadeIn:Play()
	fadeIn.Completed:Wait()

	task.wait(0.3) -- tempo visível antes de sumir

	fadeOut:Play()
end

-- ATIVAR SKILL
local function activateSkill()
	local char = player.Character
	if not char then return end
	
	local hrp = char:WaitForChild("HumanoidRootPart")

	local args = {
		"ZOMBIE DRIBBLING",
		"Skill3",
		{
			pitch = -0.10069907809458593,
			lookVector = hrp.CFrame.LookVector
		}
	}

	-- 🔥 Dispara Remote
	skillEvent:FireServer(unpack(args))

	-- 🔊 Som
	sound:Play()

	-- 🖼 Efeito visual
	task.spawn(playImageEffect)

	task.wait(1)

	task.wait(delay1)
	forcedDash(hrp.CFrame.LookVector)

	task.wait(delay2)
	forcedDash(hrp.CFrame.LookVector - hrp.CFrame.RightVector)

	task.wait(delay3)
	local finalDirection = (hrp.CFrame.RightVector * 0.6 + hrp.CFrame.LookVector * 0.4)
	forcedDash(finalDirection, dashDistance + lastDashExtra)
end

-- CLIQUE
button.MouseButton1Click:Connect(function()
	if onCooldown then return end
	onCooldown = true

	task.spawn(activateSkill)

	task.delay(COOLDOWN_TIME, function()
		onCooldown = false
	end)
end)

print("✅ Skill3 pronta | Fade 0.67s ativado")
