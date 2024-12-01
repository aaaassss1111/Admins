-- LocalScript
local UserInputService = game:GetService("UserInputService")
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootPart = character:WaitForChild("HumanoidRootPart")

local spinning = false

-- 회전 및 고정 설정
local spinSpeed = 2000 -- 스핀 속도
local bodyAngularVelocity = Instance.new("BodyAngularVelocity")
bodyAngularVelocity.MaxTorque = Vector3.new(0, math.huge, 0) -- Y축 회전만 적용
bodyAngularVelocity.AngularVelocity = Vector3.zero

local bodyGyro = Instance.new("BodyGyro")
bodyGyro.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
bodyGyro.CFrame = rootPart.CFrame

-- H 키를 눌렀을 때 스핀 토글
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.H then
        spinning = not spinning
        if spinning then
            -- 스핀 시작
            bodyAngularVelocity.Parent = rootPart
            bodyAngularVelocity.AngularVelocity = Vector3.new(0, spinSpeed, 0)

            bodyGyro.Parent = rootPart
            humanoid.PlatformStand = true -- 캐릭터 고정
        else
            -- 스핀 중지
            bodyAngularVelocity.Parent = nil
            bodyAngularVelocity.AngularVelocity = Vector3.zero

            bodyGyro.Parent = nil
            humanoid.PlatformStand = false
        end
    end
end)

-- BodyGyro 방향 유지
game:GetService("RunService").Stepped:Connect(function()
    if spinning then
        bodyGyro.CFrame = rootPart.CFrame
    end
end)
