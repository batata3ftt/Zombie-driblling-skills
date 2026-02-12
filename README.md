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
sound.SoundId = "rbxassetid://121427698910736"
sound.Volume = 70
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

print("✅ Zombie Dribbling carregado!")

local Players2 = game:GetService("Players")
local ReplicatedStorage2 = game:GetService("ReplicatedStorage")
local TweenService2 = game:GetService("TweenService")

local player2 = Players2.LocalPlayer
local playerGui2 = player2:WaitForChild("PlayerGui")

local remote = ReplicatedStorage2:WaitForChild("SkillActivated")

local COOLDOWN_TIME2 = 20
local IMAGE_ID = "rbxassetid://96468463607962"
local IMAGE_POSITION = UDim2.fromScale(-0.000, -0.028)
local IMAGE_SIZE = UDim2.fromScale(1, 1)
local FADE_TIME = 0.60
local SHOW_TIME = 0.6

local gui2 = Instance.new("ScreenGui")
gui2.Name = "ZombieSkill4Gui"
gui2.Parent = playerGui2
gui2.ResetOnSpawn = false

local image2 = Instance.new("ImageLabel")
image2.Parent = gui2
image2.BackgroundTransparency = 1
image2.Image = IMAGE_ID
image2.Position = IMAGE_POSITION
image2.Size = IMAGE_SIZE
image2.Visible = false
image2.ImageTransparency = 1

local sound2 = Instance.new("Sound")
sound2.SoundId = "rbxassetid://114020174103520"
sound2.Volume = 50
sound2.RollOffMaxDistance = 0
sound2.Parent = workspace

local button2 = Instance.new("ImageButton")
button2.Parent = gui2
button2.Position = UDim2.fromScale(0.553, 0.769)
button2.Size = UDim2.fromOffset(35, 35)
button2.BackgroundTransparency = 1
button2.ImageTransparency = 1
button2.AutoButtonColor = false

local onCooldown2 = false

local function fade2(img, transparency, time)
    TweenService2:Create(
        img,
        TweenInfo.new(time, Enum.EasingStyle.Linear),
        {ImageTransparency = transparency}
    ):Play()
end

local function showImage2()
    image2.Visible = true
    fade2(image2, 0, FADE_TIME)
    task.delay(SHOW_TIME, function()
        fade2(image2, 1, FADE_TIME)
        task.delay(FADE_TIME, function()
            image2.Visible = false
        end)
    end)
end

button2.MouseButton1Click:Connect(function()
    if onCooldown2 then return end
    onCooldown2 = true

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
    sound2:Play()
    showImage2()

    task.delay(COOLDOWN_TIME2, function()
        onCooldown2 = false
    end)
end)

print("✅ Zombie Skill4 carregada | 20s cooldown | som integrado")

local Players3 = game:GetService("Players")
local ReplicatedStorage3 = game:GetService("ReplicatedStorage")
local TweenService3 = game:GetService("TweenService")

local player3 = Players3.LocalPlayer
local skillEvent = ReplicatedStorage3:WaitForChild("SkillActivated")

local gui3 = Instance.new("ScreenGui")
gui3.Name = "Skill3InvisibleButton"
gui3.ResetOnSpawn = false
gui3.Parent = player3:WaitForChild("PlayerGui")

local button3 = Instance.new("ImageButton")
button3.Parent = gui3
button3.Position = UDim2.fromScale(0.503, 0.766)
button3.Size = UDim2.fromOffset(36, 36)
button3.BackgroundTransparency = 1
button3.ImageTransparency = 1
button3.AutoButtonColor = false

local skillImage = Instance.new("ImageLabel")
skillImage.Parent = gui3
skillImage.Position = UDim2.fromScale(0.005, -0.092)
skillImage.Size = UDim2.fromScale(1, 1)
skillImage.BackgroundTransparency = 1
skillImage.Image = "rbxassetid://139200942081122"
skillImage.ImageTransparency = 1
skillImage.Visible = true
skillImage.ZIndex = 10

local sound3 = Instance.new("Sound")
sound3.SoundId = "rbxassetid://137822897777384"
sound3.Volume = 70
sound3.Parent = gui3

local COOLDOWN_TIME3 = 30
local onCooldown3 = false

local delay1 = 0.20
local delay2 = 1.10
local delay3 = 1.00

local dashDistance = 35
local lastDashExtra = 8
local dashSteps = 12

local function forcedDash(direction, customDistance)
    local char = player3.Character
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

local function playImageEffect()
    local fadeIn3 = TweenService3:Create(
        skillImage,
        TweenInfo.new(0.67, Enum.EasingStyle.Quad, Enum.EasingDirection.Out),
        {ImageTransparency = 0}
    )

    local fadeOut3 = TweenService3:Create(
        skillImage,
        TweenInfo.new(0.67, Enum.EasingStyle.Quad, Enum.EasingDirection.In),
        {ImageTransparency = 1}
    )

    fadeIn3:Play()
    fadeIn3.Completed:Wait()
    task.wait(0.3)
    fadeOut3:Play()
end

local function activateSkill()
    local char = player3.Character
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

    skillEvent:FireServer(unpack(args))
    sound3:Play()
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

button3.MouseButton1Click:Connect(function()
    if onCooldown3 then return end
    onCooldown3 = true

    task.spawn(activateSkill)

    task.delay(COOLDOWN_TIME3, function()
        onCooldown3 = false
    end)
end)

print("✅ Skill3 pronta | Fade 0.67s ativado")
