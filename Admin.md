-- LocalScript
local UserInputService = game:GetService("UserInputService")
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local flying = false

-- 회전 설정
local spinSpeed = 2000 -- 스핀 속도
local bodyAngularVelocity = Instance.new("BodyAngularVelocity")
bodyAngularVelocity.MaxTorque = Vector3.new(0, math.huge, 0) -- Y축 회전
bodyAngularVelocity.AngularVelocity = Vector3.zero

-- H 키를 눌렀을 때 스핀 토글
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.H then
        flying = not flying
        if flying then
            -- 스핀 시작
            local rootPart = character:WaitForChild("HumanoidRootPart")
            bodyAngularVelocity.Parent = rootPart
            bodyAngularVelocity.AngularVelocity = Vector3.new(0, spinSpeed, 0)
            humanoid.PlatformStand = true -- 몸 고정
        else
            -- 스핀 중지
            bodyAngularVelocity.Parent = nil
            bodyAngularVelocity.AngularVelocity = Vector3.zero
            humanoid.PlatformStand = false
        end
    end
end)
