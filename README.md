lua
local function createSplashScreen(showLine, fadeTime)

end
function TeleportTo(cframe)
    local player = game.Players.LocalPlayer
    local humanoidRootPart = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    
    local function attemptTeleport()
        local cframebypass = CFrame.new(1151, 20, -46)
        local cframebypass2 = CFrame.new(1152, 20, -23)

        local splashDone = false
        local operationComplete = false
        local success = true
        
        task.spawn(function()
            createSplashScreen(false, math.huge)
            splashDone = true
        end)

        task.wait(0.3)
        
        task.spawn(function()
            repeat
                pcall(function()
                    humanoidRootPart.CFrame = cframebypass
                end)
                task.wait()
                local distance = (humanoidRootPart.Position - cframebypass2.Position).Magnitude
            until distance <= 10
            
            task.wait(0.3)
                        
            pcall(function()
                humanoidRootPart.CFrame = cframe
            end)
            
            task.wait(0.5)
                        
            local finalDistance = (humanoidRootPart.Position - cframe.Position).Magnitude
            if finalDistance > 8 then
                success = false
            end
            
            operationComplete = true
        end)

        repeat task.wait() until operationComplete
        repeat task.wait() until splashDone
        
        return success
    end
    
    local attempts = 0
    local maxAttempts = 10
    
    local casinoDoor = workspace:WaitForChild("Map"):WaitForChild("Locations"):WaitForChild("Casino"):WaitForChild("Robbery"):WaitForChild("Door"):WaitForChild("Part")
    if casinoDoor and casinoDoor.CFrame == CFrame.new(1149.60217, 20.4918041, -31.2715397, -0.258864403, 0, 0.965913713, 0, 1, 0, -0.965913713, 0, -0.258864403) then
        warn("Teleport failed - Try again later!")
        return false
    end

    while attempts < maxAttempts do
        attempts = attempts + 1
        local success = attemptTeleport()
        if success then
            break
        end
        if attempts == maxAttempts then
            print("Teleport failed after " .. maxAttempts .. " attempts - Stopping")
            return
        end
        print("Teleport failed - Attempt " .. attempts .. "/" .. maxAttempts .. " - Retrying...")
        task.wait(1)
    end
end

TeleportTo(CFrame.new(-459.257, 0.875, 340.997))
