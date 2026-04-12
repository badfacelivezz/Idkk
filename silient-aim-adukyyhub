game:GetService("StarterGui"):SetCore("SendNotification", {
    Title = "Silient aim gun & knife",
    Text = "Script Activated",
    Duration = 5
})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local Camera = workspace.CurrentCamera

local function isEnemyVisible(targetPart)
    if not targetPart then return false end
    
    local origin = Camera.CFrame.Position
    local destination = targetPart.Position
    local direction = destination - origin
    
    local raycastParams = RaycastParams.new()
    raycastParams.FilterDescendantsInstances = {LocalPlayer.Character, Camera}
    raycastParams.FilterType = Enum.RaycastFilterType.Exclude
    
    local raycastResult = workspace:Raycast(origin, direction, raycastParams)
    
    if raycastResult then
        if raycastResult.Instance:IsDescendantOf(targetPart.Parent) then
            return true
        end
    end
    
    return false
end

local function getBestTarget()
    local target = nil
    local shortestDistance = math.huge

    local myTeam = LocalPlayer:GetAttribute("Team")

    for _, p in pairs(Players:GetPlayers()) do
        local pTeam = p:GetAttribute("Team")

        if p ~= LocalPlayer
        and p.Character
        and p.Character:FindFirstChild("Head")
        and p.Character:FindFirstChild("Humanoid")
        and p.Character.Humanoid.Health > 0
        and (pTeam == nil or myTeam == nil or pTeam ~= myTeam) then
            
            local pos, onScreen = Camera:WorldToViewportPoint(p.Character.Head.Position)

            if onScreen and isEnemyVisible(p.Character.Head) then
                local distance = (Vector2.new(pos.X,pos.Y) - Vector2.new(Mouse.X,Mouse.Y)).Magnitude

                if distance < shortestDistance then
                    target = p.Character.Head
                    shortestDistance = distance
                end
            end
        end
    end

    return target
end

local mt = getrawmetatable(game)
local oldIndex = mt.__index
setreadonly(mt,false)

mt.__index = newcclosure(function(self,index)
    if self == Mouse and (index == "Hit" or index == "Target") then
        local target = getBestTarget()
        if target then
            return (index == "Hit" and target.CFrame or target)
        end
    end
    return oldIndex(self,index)
end)

setreadonly(mt,true)
