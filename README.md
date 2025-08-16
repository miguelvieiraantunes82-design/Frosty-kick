Fristy v2 -_-
--[[
    "Frosty Kick" Roblox Lua Script
    - Simple interface to kick a player by nickname
    - UI can be moved around the screen
    - For use in Roblox LocalScript (for testing/educational purposes)
    - Inspired by "Steal a Brainrot" game
    - Note: Only works if you have the power to kick (e.g., in your own game with admin privileges)
]]

-- Check Roblox context
if not game or not game.Players or not game.Players.LocalPlayer then
    print("Frosty Kick: This script must be run in a Roblox client (LocalScript).")
    return
end

local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local StarterGui = game:GetService("StarterGui")

-- UI Creation
local frostyGui = Instance.new("ScreenGui")
frostyGui.Name = "FrostyKickGui"
frostyGui.ResetOnSpawn = false
frostyGui.Parent = Player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 290, 0, 160)
frame.Position = UDim2.new(0.5, -145, 0.5, -80)
frame.BackgroundColor3 = Color3.fromRGB(35, 45, 65)
frame.BorderSizePixel = 0
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.Active = true
frame.Draggable = true -- Roblox deprecated this, see manual drag code below for universal support
frame.Parent = frostyGui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 38)
title.BackgroundTransparency = 1
title.Text = "Frosty Kick"
title.Font = Enum.Font.GothamBold
title.TextSize = 26
title.TextColor3 = Color3.fromRGB(160, 220, 255)
title.Parent = frame

local info = Instance.new("TextLabel")
info.Size = UDim2.new(1, -20, 0, 22)
info.Position = UDim2.new(0, 10, 0, 42)
info.BackgroundTransparency = 1
info.Text = "Enter the player's nickname to expel:"
info.Font = Enum.Font.Gotham
info.TextSize = 15
info.TextColor3 = Color3.fromRGB(200, 220, 240)
info.TextWrapped = true
info.Parent = frame

-- Nickname input
local nicknameBox = Instance.new("TextBox")
nicknameBox.Size = UDim2.new(0.8, 0, 0, 32)
nicknameBox.Position = UDim2.new(0.1, 0, 0, 68)
nicknameBox.BackgroundColor3 = Color3.fromRGB(245, 245, 255)
nicknameBox.TextColor3 = Color3.fromRGB(30, 30, 40)
nicknameBox.PlaceholderText = "PlayerNickname"
nicknameBox.Font = Enum.Font.Gotham
nicknameBox.TextSize = 18
nicknameBox.Parent = frame

-- Kick button
local kickBtn = Instance.new("TextButton")
kickBtn.Size = UDim2.new(0.7, 0, 0, 32)
kickBtn.Position = UDim2.new(0.15, 0, 1, -44)
kickBtn.BackgroundColor3 = Color3.fromRGB(70, 50, 110)
kickBtn.BorderSizePixel = 0
kickBtn.Text = "Kick"
kickBtn.Font = Enum.Font.GothamBold
kickBtn.TextSize = 20
kickBtn.TextColor3 = Color3.fromRGB(255, 240, 255)
kickBtn.Parent = frame

-- Feedback label
local feedback = Instance.new("TextLabel")
feedback.Size = UDim2.new(1, -20, 0, 18)
feedback.Position = UDim2.new(0, 10, 1, -22)
feedback.BackgroundTransparency = 1
feedback.Text = ""
feedback.Font = Enum.Font.Gotham
feedback.TextSize = 14
feedback.TextColor3 = Color3.fromRGB(255, 120, 120)
feedback.TextWrapped = true
feedback.Parent = frame

-- KICK LOGIC
kickBtn.MouseButton1Click:Connect(function()
    local nickname = nicknameBox.Text
    if nickname == "" then
        feedback.Text = "Enter a nickname!"
        return
    end
    local target = nil
    for _, plr in ipairs(Players:GetPlayers()) do
        if string.lower(plr.Name) == string.lower(nickname) or (plr.DisplayName and string.lower(plr.DisplayName) == string.lower(nickname)) then
            target = plr
            break
        end
    end
    if not target then
        feedback.Text = "Player not found!"
        return
    end
    if target == Player then
        feedback.Text = "You cannot kick yourself with this!"
        return
    end

    -- Try to kick (only works if LocalPlayer has permissions, e.g. is server owner)
    local success, err = pcall(function()
        -- For LocalScript, we can't kick others unless we have remote server access (admin powers)
        -- For demonstration: if you have access to a RemoteEvent for admin, fire it here.
        -- Otherwise, show info:
        target:Kick("Frosty Kick: You have been expelled from the game.")
    end)
    if success then
        feedback.Text = "Player kicked!"
    else
        feedback.Text = "Kick failed (no permission?)"
    end
end)

-- Manual Drag Support (for universal compatibility)
local UIS = game:GetService("UserInputService")
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
