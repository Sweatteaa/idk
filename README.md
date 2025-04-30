local Players = game:GetService("Players")
local localPlayer = Players.LocalPlayer
 
-- Team color definitions
local teamColors = {
    ["Guards"] = Color3.fromRGB(0, 170, 255),     -- Blue
    ["Criminals"] = Color3.fromRGB(255, 0, 0),    -- Red
    ["Inmates"] = Color3.fromRGB(255, 140, 0),    -- Vivid Orange
}
 
-- Function to highlight a character based on team
local function highlightCharacter(character, teamName)
    if not character or not character:IsDescendantOf(game) then return end
    if character:FindFirstChild("PlayerHighlight") then return end
 
    local fillColor = teamColors[teamName]
    if not fillColor then return end
 
    local highlight = Instance.new("Highlight")
    highlight.Name = "PlayerHighlight"
    highlight.FillColor = fillColor
    highlight.OutlineTransparency = 1 -- Hide stroke
    highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
    highlight.Adornee = character
    highlight.Parent = character
end
 
-- Setup a player for highlighting
local function setupPlayer(player)
    if player == localPlayer then return end
 
    local function applyHighlight(character)
        local teamName = player.Team and player.Team.Name or nil
        highlightCharacter(character, teamName)
    end
 
    player.CharacterAdded:Connect(applyHighlight)
 
    if player.Character then
        applyHighlight(player.Character)
    end
end
 
-- Run setup for all existing and future players
for _, player in ipairs(Players:GetPlayers()) do
    setupPlayer(player)
end
 
Players.PlayerAdded:Connect(setupPlayer)
