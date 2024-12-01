-- LocalScript
local UserInputService = game:GetService("UserInputService")
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local flying = false

-- 비행 설정
local flightSpeed = 50 -- 비행 속도
local verticalSpeed = 50 -- 상승/하강 속도
local bodyVelocity = Instance.new("BodyVelocity")
bodyVelocity.MaxForce = Vector3.new(100000, 100000, 100000)
bodyVelocity.Velocity = Vector3.zero

-- 상승/하강 상태
local ascend = false
local descend = false

-- H 키를 눌렀을 때 비행 토글
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.H then
        flying = not flying
        if flying then
            -- 비행 시작
            local rootPart = character:WaitForChild("HumanoidRootPart")
            bodyVelocity.Parent = rootPart
            humanoid.PlatformStand = true
        else
            -- 비행 중지
            bodyVelocity.Parent = nil
            humanoid.PlatformStand = false
        end
    elseif flying and input.KeyCode == Enum.KeyCode.Space then
        -- 상승
        ascend = true
    elseif flying and input.KeyCode == Enum.KeyCode.LeftShift then
        -- 하강
        descend = true
    end
end)

UserInputService.InputEnded:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.Space then
        ascend = false
    elseif input.KeyCode == Enum.KeyCode.LeftShift then
        descend = false
    end
end)

-- 비행 중일 때 이동
game:GetService("RunService").RenderStepped:Connect(function()
    if flying then
        local camera = workspace.CurrentCamera
        local moveDirection = humanoid.MoveDirection
        local verticalVelocity = 0

        if ascend then
            verticalVelocity = verticalSpeed
        elseif descend then
            verticalVelocity = -verticalSpeed
        end

        bodyVelocity.Velocity = camera.CFrame.LookVector * moveDirection.Magnitude * flightSpeed + Vector3.new(0, verticalVelocity, 0)
    end
end)
