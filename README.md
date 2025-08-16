--[[
    frosty_kick.lua
    Simple Lua script: "Frosty Kick" - Kicks a player from the game (inspired by "Steal a Brainrot" games)
    Not-so-advanced interface, for educational purposes.
]]

-- Check if running in Roblox
if not game or not game.Players or not game.Players.LocalPlayer then
    print("Frosty Kick: This script must be run in a Roblox client (LocalScript).")
    return
end

-- Services
local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local StarterGui = game:GetService("StarterGui")

-- Create ScreenGui
local frostyGui = Instance.new("ScreenGui")
frostyGui.Name = "FrostyKickGui"
frostyGui.ResetOnSpawn = false
frostyGui.Parent = Player:WaitForChild("PlayerGui")

-- Create main frame
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 250, 0, 120)
frame.Position = UDim2.new(0.5, -125, 0.5, -60)
frame.BackgroundColor3 = Color3.fromRGB(35, 45, 65)
frame.BorderSizePixel = 0
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.Parent = frostyGui

-- Title label
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundTransparency = 1
title.Text = "Frosty Kick"
title.Font = Enum.Font.GothamBold
title.TextSize = 28
title.TextColor3 = Color3.fromRGB(160, 220, 255)
title.Parent = frame

-- Info label
local info = Instance.new("TextLabel")
info.Size = UDim2.new(1, -20, 0, 30)
info.Position = UDim2.new(0, 10, 0, 45)
info.BackgroundTransparency = 1
info.Text = "Press the button to expel yourself from the game."
info.Font = Enum.Font.Gotham
info.TextSize = 15
info.TextColor3 = Color3.fromRGB(200, 220, 240)
info.TextWrapped = true
info.Parent = frame

-- Kick button
local kickBtn = Instance.new("TextButton")
kickBtn.Size = UDim2.new(0.7, 0, 0, 32)
kickBtn.Position = UDim2.new(0.15, 0, 1, -42)
kickBtn.BackgroundColor3 = Color3.fromRGB(70, 50, 110)
kickBtn.BorderSizePixel = 0
kickBtn.Text = "Kick (Expel)"
kickBtn.Font = Enum.Font.GothamBold
kickBtn.TextSize = 20
kickBtn.TextColor3 = Color3.fromRGB(255, 240, 255)
kickBtn.Parent = frame

-- Button behavior
kickBtn.MouseButton1Click:Connect(function()
    -- Optionally, show a notification before kicking
    StarterGui:SetCore("SendNotification", {
        Title = "Frosty Kick",
        Text = "You have been expelled from the game.",
        Duration = 2
    })
    wait(0.7)
    Player:Kick("Frosty Kick: You have been expelled from the game.")
end)

-- Optional: Drag functionality
local dragging, dragInput, dragStart, startPos
frame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = frame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)
frame.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)
