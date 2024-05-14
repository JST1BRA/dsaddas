local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local localPlayer = Players.LocalPlayer
local currentSpectateTarget = nil

-- Function to start spectating
local function startSpectating(targetPlayer)
    if targetPlayer and targetPlayer.Character then
        currentSpectateTarget = targetPlayer
        RunService:BindToRenderStep("SpectatePlayer", Enum.RenderPriority.Camera.Value, function()
            if currentSpectateTarget and currentSpectateTarget.Character then
                workspace.CurrentCamera.CameraSubject = currentSpectateTarget.Character:FindFirstChildOfClass("Humanoid")
            end
        end)
    end
end

-- Function to stop spectating
local function stopSpectating()
    currentSpectateTarget = nil
    RunService:UnbindFromRenderStep("SpectatePlayer")
    workspace.CurrentCamera.CameraSubject = localPlayer.Character:FindFirstChildOfClass("Humanoid")
end

-- Listen for chat messages
local function onPlayerChatted(message)
    if message:sub(1, 6) == "!view " then
        local playerName = message:sub(7)
        local targetPlayer = Players:FindFirstChild(playerName)
        if targetPlayer then
            startSpectating(targetPlayer)
        else
            print("Player not found.")
        end
    elseif message == "!unview" then
        stopSpectating()
    end
end

localPlayer.Chatted:Connect(onPlayerChatted)
