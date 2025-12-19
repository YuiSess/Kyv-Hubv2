local FastAttackModule = {}

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local Player = Players.LocalPlayer

local function SafeWaitForChild(parent, childName)
    local success, result = pcall(function()
        return parent:WaitForChild(childName)
    end)
    if not success or not result then
        warn("SafeWaitForChild falhou: " .. tostring(childName))
    end
    return result
end

local Modules = SafeWaitForChild(ReplicatedStorage, "Modules")
local Net = SafeWaitForChild(Modules, "Net")

local Settings = {
    AutoClick = true,
    ClickDelay = 0
}

local FastAttack = {
    Distance = 100,
    attackMobs = true,
    attackPlayers = true,
    Equipped = nil
}

local RegisterAttack = SafeWaitForChild(Net, "RE/RegisterAttack")
local RegisterHit = SafeWaitForChild(Net, "RE/RegisterHit")

local function IsAlive(character)
    return character and character:FindFirstChild("Humanoid") and character.Humanoid.Health > 0
end

local function ProcessEnemies(OthersEnemies, Folder)
    local BasePart = nil
    for _, Enemy in ipairs(Folder:GetChildren()) do
        local Head = Enemy:FindFirstChild("Head")
        if Head and IsAlive(Enemy) and Player:DistanceFromCharacter(Head.Position) < FastAttack.Distance then
            if Enemy ~= Player.Character then
                table.insert(OthersEnemies, { Enemy, Head })
                BasePart = Head
            end
        end
    end
    return BasePart
end

function FastAttack:Attack(BasePart, OthersEnemies)
    if not BasePart or #OthersEnemies == 0 then return end
    pcall(function()
        RegisterAttack:FireServer(Settings.ClickDelay or 0)
        RegisterHit:FireServer(BasePart, OthersEnemies)
    end)
end

function FastAttack:AttackNearest()
    local OthersEnemies = {}
    local EnemiesFolder = workspace:FindFirstChild("Enemies")
    local CharactersFolder = workspace:FindFirstChild("Characters")
    local Part1 = nil
    local Part2 = nil
    if EnemiesFolder then Part1 = ProcessEnemies(OthersEnemies, EnemiesFolder) end
    if CharactersFolder then Part2 = ProcessEnemies(OthersEnemies, CharactersFolder) end
    local character = Player.Character
    if not character then return end
    local equippedWeapon = character:FindFirstChildOfClass("Tool")
    if equippedWeapon and equippedWeapon:FindFirstChild("LeftClickRemote") then
        for _, enemyData in ipairs(OthersEnemies) do
            local enemy = enemyData[1]
            if enemy and enemy:FindFirstChild("HumanoidRootPart") and character:FindFirstChild("HumanoidRootPart") then
                local direction = (enemy.HumanoidRootPart.Position - character:GetPivot().Position).Unit
                pcall(function()
                    equippedWeapon.LeftClickRemote:FireServer(direction, 1)
                end)
            end
        end
    elseif #OthersEnemies > 0 then
        self:Attack(Part1 or Part2, OthersEnemies)
    else
        task.wait(0)
    end
end

function FastAttack:BladeHits()
    local Equipped = IsAlive(Player.Character) and Player.Character:FindFirstChildOfClass("Tool")
    if Equipped and Equipped.ToolTip ~= "Gun" then
        self:AttackNearest()
    else
        task.wait(0)
    end
end

task.spawn(function()
    while task.wait(Settings.ClickDelay) do
        if Settings.AutoClick then
            FastAttack:BladeHits()
        end
    end
end)

_ENV.rz_FastAttack = FastAttack
FastAttackModule.FastAttack = FastAttack

return FastAttackModule
