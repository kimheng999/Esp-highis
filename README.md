local players = game:GetService("Players")
local localPlayer = players.LocalPlayer
local camera = game:GetService("Workspace").CurrentCamera
local runService = game:GetService("RunService")
 
-- Function to create the ESP box with team color
local function createESPBox(player)
    local highlight = Instance.new("Highlight")
    highlight.Name = player.Name .. "_ESP"
    highlight.Adornee = player.Character
    highlight.FillTransparency = 1 -- Makes the fill of the box invisible
    
    -- Check if the player is on a team and apply the team color
    if player.Team then
        highlight.OutlineColor = player.Team.TeamColor.Color -- Set the box color to the team's color
    else
        highlight.OutlineColor = Color3.new(1, 1, 1) -- Default color (white) if not in a team
    end
    
    highlight.OutlineTransparency = 0 -- Fully visible outline
    highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop -- Can be seen through walls
    highlight.Parent = player.Character
end
 
-- Add ESP to all players
local function addESP()
    for _, player in pairs(players:GetPlayers()) do
        if player ~= localPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            -- Check if ESP already exists to avoid duplication
            if not player.Character:FindFirstChild(player.Name .. "_ESP") then
                createESPBox(player)
            end
        end
    end
end
 
-- Run the ESP adding function continuously
runService.RenderStepped:Connect(function()
    addESP()
end)
 
-- Remove ESP if a player leaves
players.PlayerRemoving:Connect(function(player)
    if player.Character and player.Character:FindFirstChild(player.Name .. "_ESP") then
        player.Character[player.Name .. "_ESP"]:Destroy()
    end
end)
