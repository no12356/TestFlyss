-- LocalScript to enable flying in Roblox

-- Variables
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- Constants
local FLY_SPEED = 50 -- Adjust the flying speed as needed

-- Function to toggle flying
local function toggleFly()
    if humanoid:GetState() == Enum.HumanoidStateType.Physics then
        -- Already flying, so we stop flying
        humanoid.PlatformStand = false
        character:WaitForChild("HumanoidRootPart").Anchored = false
    else
        -- Start flying
        humanoid.PlatformStand = true
        character:WaitForChild("HumanoidRootPart").Anchored = true
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Velocity = Vector3.new(0, FLY_SPEED, 0)
        bodyVelocity.MaxForce = Vector3.new(0, math.huge, 0)
        bodyVelocity.Parent = character:WaitForChild("HumanoidRootPart")
        
        -- Fly control
        while humanoid.PlatformStand do
            local moveDirection = Vector3.new()
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                moveDirection = moveDirection + character:WaitForChild("HumanoidRootPart").CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                moveDirection = moveDirection - character:WaitForChild("HumanoidRootPart").CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                moveDirection = moveDirection - character:WaitForChild("HumanoidRootPart").CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                moveDirection = moveDirection + character:WaitForChild("HumanoidRootPart").CFrame.RightVector
            end
            
            bodyVelocity.Velocity = moveDirection.Unit * FLY_SPEED
            wait()
        end
        
        -- Clean up
        bodyVelocity:Destroy()
    end
end

-- Bind toggle function to a key
local UserInputService = game:GetService("UserInputService")
UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.Space then
        toggleFly()
    end
end)
