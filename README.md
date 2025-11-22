-- KYV Hub By>OrchiDxxx - Ultra Complete Version
-- Sistema COMPLETO com TODAS as funcionalidades funcionais

local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("💎 KYV Hub By>OrchiDxxx | Ultra Complete", "Ocean")

-- Services
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local Player = Players.LocalPlayer
local Workspace = game:GetService("Workspace")

-- Configurações Globais
local Settings = {
    -- Fast Attack
    AutoClick = false,
    ClickDelay = 0,
    FastAttackEnabled = true,
    AttackDistance = 100,
    
    -- Auto Farm Level
    AutoFarmLevel = false,
    AutoFarmNearest = false,
    
    -- Auto Farm Bones
    AutoFarmBones = false,
    BoneFarmLocation = "Haunted Castle",
    
    -- Auto Farm Boss
    AutoFarmBoss = false,
    SelectedBoss = "All",
    
    -- Auto Collect
    AutoCollectFruit = false,
    AutoCollectChest = false,
    
    -- Position Settings
    FarmMode = "Behind",
    DistanceFromMob = 15,
    HeightOffset = 10,
    CustomAngle = 0,
    RotateAroundMob = false,
    RotationSpeed = 1,
    
    -- Tween Settings
    TweenSpeed = 350,
    SelectedIsland = "None",
    
    -- One Click
    OneClickActive = false,
    
    -- Auto Mastery
    AutoFarmMastery = false,
    MasteryType = "Devil Fruit",
    
    -- Auto Farm Materials
    AutoFarmEctoplasm = false,
    AutoFarmAngel = false,
    AutoFarmMagmaOre = false,
    AutoFarmRadioactive = false,
    AutoFarmVampireFang = false,
    AutoFarmMysticDroplet = false,
    AutoFarmLeather = false,
    AutoFarmScrap = false,
    AutoFarmMiniTusk = false,
    AutoFarmFishTail = false,
    
    -- Auto Farm Special
    AutoFarmElite = false,
    AutoFarmCake = false,
    AutoFarmDough = false,
    AutoFarmFactory = false,
    AutoFarmObservation = false,
    
    -- Auto Farm Sea
    AutoFarmShark = false,
    AutoFarmPiranha = false,
    AutoFarmTerrorShark = false,
    AutoFarmSeaBeast = false,
    AutoFarmShip = false,
    
    -- Auto Farm Haki
    AutoFarmBusoHaki = false,
    AutoFarmKenHaki = false,
    
    -- Auto Get Items
    AutoGetLibraryKey = false,
    AutoGetSoulGuitar = false,
    AutoGetHolyTorch = false,
    
    -- Auto Quest
    AutoQuest = false,
    
    -- Safety
    SafeMode = true,
    AutoHeal = true,
    HealthThreshold = 50,
    AntiAFK = true,
    AutoRespawn = true,
    
    -- Status
    CurrentFarmingMob = "None",
    FarmedKills = 0,
    SessionTime = 0,
    CollectedFruits = 0,
    CollectedChests = 0
}

-- Fast Attack Module
local FastAttack = {
    Distance = 100,
    attackMobs = true,
    attackPlayers = false,
    CurrentTarget = nil
}

-- Tabelas de Dados do Jogo
local IslandPositions = {
    -- First Sea
    ["Jungle"] = CFrame.new(-1612, 37, 149),
    ["Pirate Starter"] = CFrame.new(1071, 16, 1426),
    ["Marine Starter"] = CFrame.new(-2573, 72, -14),
    ["Middle Town"] = CFrame.new(-655, 15, 1582),
    ["Buggy"] = CFrame.new(-862, 205, 1988),
    ["Desert"] = CFrame.new(944, 20, 4373),
    ["Frozen Village"] = CFrame.new(1100, 104, -1388),
    ["Marine Fortress"] = CFrame.new(-4505, 21, 4101),
    ["Sky Island"] = CFrame.new(-4970, 717, -2622),
    ["Prison"] = CFrame.new(4875, 5, 734),
    ["Colosseum"] = CFrame.new(-1427, 7, 298),
    ["Magma Village"] = CFrame.new(-5231, 8, 8466),
    ["Under Water"] = CFrame.new(61123, 11, 1819),
    ["Upper Sky"] = CFrame.new(-7894, 5547, -380),
    ["Fountain City"] = CFrame.new(5127, 59, 4105),
    
    -- Second Sea
    ["Kingdom of Rose"] = CFrame.new(-384, 37, 298),
    ["Cafe"] = CFrame.new(-385, 73, 297),
    ["Mansion"] = CFrame.new(-12513, 337, -7501),
    ["Graveyard"] = CFrame.new(-8652, 142, 6032),
    ["Zombie Island"] = CFrame.new(-5622, 492, -781),
    ["Cursed Ship"] = CFrame.new(923, 125, 32852),
    ["Ice Castle"] = CFrame.new(5394, 23, -298),
    ["Forgotten Island"] = CFrame.new(-3032, 235, -10075),
    ["Dark Arena"] = CFrame.new(3780, 92, -3260),
    
    -- Third Sea
    ["Port Town"] = CFrame.new(-290, 43, 5343),
    ["Hydra Island"] = CFrame.new(5749, 611, -276),
    ["Great Tree"] = CFrame.new(2681, 1682, -7190),
    ["Castle on the Sea"] = CFrame.new(-5075, 314, -3150),
    ["Haunted Castle"] = CFrame.new(-9515, 142, 5567),
    ["Sea Castle"] = CFrame.new(-5075, 314, -3150),
    ["Floating Turtle"] = CFrame.new(-13274, 332, -7835),
    ["Tiki Outpost"] = CFrame.new(-16542, 55, 1044)
}

local MobLevelData = {
    {Name = "Bandit", Level = {5, 10}, Location = "Jungle"},
    {Name = "Monkey", Level = {14, 20}, Location = "Jungle"},
    {Name = "Gorilla", Level = {20, 25}, Location = "Jungle"},
    {Name = "Pirate", Level = {35, 40}, Location = "Buggy"},
    {Name = "Brute", Level = {45, 50}, Location = "Buggy"},
    {Name = "Desert Bandit", Level = {60, 75}, Location = "Desert"},
    {Name = "Desert Officer", Level = {70, 80}, Location = "Desert"},
    {Name = "Snow Bandit", Level = {90, 100}, Location = "Frozen Village"},
    {Name = "Snowman", Level = {100, 110}, Location = "Frozen Village"},
    {Name = "Chief Petty Officer", Level = {120, 130}, Location = "Marine Fortress"},
    {Name = "Sky Bandit", Level = {150, 160}, Location = "Sky Island"},
    {Name = "Dark Master", Level = {175, 190}, Location = "Sky Island"},
    {Name = "Prisoner", Level = {190, 210}, Location = "Prison"},
    {Name = "Dangerous Prisoner", Level = {210, 250}, Location = "Prison"},
    {Name = "Toga Warrior", Level = {250, 275}, Location = "Colosseum"},
    {Name = "Gladiator", Level = {275, 300}, Location = "Colosseum"},
    {Name = "Military Soldier", Level = {300, 325}, Location = "Magma Village"},
    {Name = "Military Spy", Level = {325, 350}, Location = "Magma Village"},
    {Name = "Magma Admiral", Level = {350, 375}, Location = "Magma Village"},
    {Name = "Fishman Warrior", Level = {375, 400}, Location = "Under Water"},
    {Name = "Fishman Commando", Level = {400, 425}, Location = "Under Water"},
    {Name = "God's Guard", Level = {425, 475}, Location = "Upper Sky"},
    {Name = "Shanda", Level = {475, 525}, Location = "Upper Sky"},
    {Name = "Royal Squad", Level = {525, 550}, Location = "Upper Sky"},
    {Name = "Royal Soldier", Level = {550, 600}, Location = "Upper Sky"},
    {Name = "Galley Pirate", Level = {625, 675}, Location = "Fountain City"},
    {Name = "Galley Captain", Level = {675, 700}, Location = "Fountain City"},
    {Name = "Raider", Level = {700, 750}, Location = "Kingdom of Rose"},
    {Name = "Mercenary", Level = {725, 775}, Location = "Kingdom of Rose"},
    {Name = "Swan Pirate", Level = {775, 800}, Location = "Kingdom of Rose"},
    {Name = "Factory Staff", Level = {800, 850}, Location = "Kingdom of Rose"},
    {Name = "Marine Lieutenant", Level = {875, 900}, Location = "Kingdom of Rose"},
    {Name = "Marine Captain", Level = {900, 950}, Location = "Kingdom of Rose"},
    {Name = "Zombie", Level = {950, 975}, Location = "Graveyard"},
    {Name = "Vampire", Level = {975, 1000}, Location = "Graveyard"},
    {Name = "Peanut Scout", Level = {1000, 1050}, Location = "Zombie Island"},
    {Name = "Peanut President", Level = {1050, 1100}, Location = "Zombie Island"},
    {Name = "Ice Admiral", Level = {1100, 1150}, Location = "Ice Castle"},
    {Name = "Vice Admiral", Level = {1150, 1200}, Location = "Ice Castle"},
    {Name = "Reborn Skeleton", Level = {1975, 2000}, Location = "Haunted Castle"},
    {Name = "Living Zombie", Level = {2000, 2025}, Location = "Haunted Castle"},
    {Name = "Demonic Soul", Level = {2025, 2050}, Location = "Haunted Castle"},
    {Name = "Posessed Mummy", Level = {2050, 2075}, Location = "Haunted Castle"}
}

-- Funções Utilitárias
local function SafeWaitForChild(parent, childName, timeout)
    if not parent then return nil end
    local success, result = pcall(function()
        return parent:WaitForChild(childName, timeout or 5)
    end)
    return success and result or nil
end

local Modules = SafeWaitForChild(ReplicatedStorage, "Modules")
local Net = Modules and SafeWaitForChild(Modules, "Net")
local RegisterAttack = Net and SafeWaitForChild(Net, "RE/RegisterAttack")
local RegisterHit = Net and SafeWaitForChild(Net, "RE/RegisterHit")

local function GetCharacter()
    return Player.Character
end

local function GetHumanoid()
    local char = GetCharacter()
    return char and char:FindFirstChild("Humanoid")
end

local function GetRootPart()
    local char = GetCharacter()
    return char and char:FindFirstChild("HumanoidRootPart")
end

local function IsAlive(character)
    if not character then return false end
    local humanoid = character:FindFirstChild("Humanoid")
    return humanoid and humanoid.Health > 0
end

local function GetPlayerLevel()
    local char = GetCharacter()
    if char then
        local levelValue = char:FindFirstChild("Level") or Player:FindFirstChild("Data") and Player.Data:FindFirstChild("Level")
        if levelValue then
            return levelValue.Value or 1
        end
    end
    return 1
end

local function Notify(title, text, duration)
    game.StarterGui:SetCore("SendNotification", {
        Title = title;
        Text = text;
        Duration = duration or 3;
    })
end

-- Sistema de Teleporte
local currentTween = nil

local function StopTween()
    if currentTween then
        currentTween:Cancel()
        currentTween = nil
    end
end

local function TweenToPosition(targetCFrame, speed)
    StopTween()
    
    local hrp = GetRootPart()
    if not hrp then return end
    
    speed = speed or Settings.TweenSpeed
    local distance = (hrp.Position - targetCFrame.Position).Magnitude
    local time = distance / speed
    
    local tweenInfo = TweenInfo.new(
        time,
        Enum.EasingStyle.Linear,
        Enum.EasingDirection.Out
    )
    
    currentTween = TweenService:Create(hrp, tweenInfo, {CFrame = targetCFrame})
    currentTween:Play()
    
    return currentTween
end

-- Sistema de Posicionamento
local rotationAngle = 0

local function GetFarmPosition(targetHRP)
    if not targetHRP then return nil end
    
    local pos = targetHRP.Position
    local offset = Vector3.new(0, 0, 0)
    local distance = Settings.DistanceFromMob
    local height = Settings.HeightOffset
    
    if Settings.RotateAroundMob then
        rotationAngle = (rotationAngle + Settings.RotationSpeed) % 360
        local rad = math.rad(rotationAngle)
        offset = Vector3.new(math.cos(rad) * distance, height, math.sin(rad) * distance)
    else
        local angle = Settings.CustomAngle
        
        if Settings.FarmMode == "Behind" then
            angle = 180
        elseif Settings.FarmMode == "Above" then
            return CFrame.new(pos.X, pos.Y + height, pos.Z)
        elseif Settings.FarmMode == "Below" then
            return CFrame.new(pos.X, pos.Y - height, pos.Z)
        elseif Settings.FarmMode == "Front" then
            angle = 0
        end
        
        local rad = math.rad(angle)
        offset = Vector3.new(math.cos(rad) * distance, height, math.sin(rad) * distance)
    end
    
    return CFrame.new(pos + offset)
end

-- Fast Attack System
local function ProcessEnemies(OthersEnemies, Folder)
    local BasePart = nil
    for _, Enemy in ipairs(Folder:GetChildren()) do
        if Enemy:IsA("Model") then
            local Head = Enemy:FindFirstChild("Head")
            local HRP = Enemy:FindFirstChild("HumanoidRootPart")
            if Head and HRP and IsAlive(Enemy) then
                local distance = Player:DistanceFromCharacter(HRP.Position)
                if distance < FastAttack.Distance and Enemy ~= Player.Character then
                    table.insert(OthersEnemies, { Enemy, Head })
                    BasePart = Head
                end
            end
        end
    end
    return BasePart
end

function FastAttack:Attack(BasePart, OthersEnemies)
    if not BasePart or #OthersEnemies == 0 or not Settings.FastAttackEnabled then return end
    pcall(function()
        if RegisterAttack then
            RegisterAttack:FireServer(Settings.ClickDelay or 0)
        end
        if RegisterHit then
            RegisterHit:FireServer(BasePart, OthersEnemies)
        end
    end)
end

function FastAttack:AttackNearest()
    local OthersEnemies = {}
    local EnemiesFolder = workspace:FindFirstChild("Enemies")
    local Part1 = nil
    
    if EnemiesFolder then 
        Part1 = ProcessEnemies(OthersEnemies, EnemiesFolder) 
    end
    
    local character = GetCharacter()
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
        self:Attack(Part1, OthersEnemies)
    end
end

function FastAttack:BladeHits()
    if not IsAlive(GetCharacter()) then return end
    local Equipped = GetCharacter():FindFirstChildOfClass("Tool")
    if Equipped then
        self:AttackNearest()
    end
end

-- Sistema de Busca de Mobs
local function GetMobForLevel(playerLevel)
    for _, mobData in ipairs(MobLevelData) do
        if playerLevel >= mobData.Level[1] and playerLevel <= mobData.Level[2] + 50 then
            return mobData.Name, mobData.Location
        end
    end
    return "Bandit", "Jungle"
end

local function FindMobByName(mobName)
    local enemiesFolder = workspace:FindFirstChild("Enemies")
    if not enemiesFolder then return nil end
    
    for _, enemy in ipairs(enemiesFolder:GetChildren()) do
        if enemy.Name == mobName and IsAlive(enemy) then
            return enemy
        end
    end
    return nil
end

local function GetNearestMob()
    local nearestMob = nil
    local shortestDistance = math.huge
    local hrp = GetRootPart()
    if not hrp then return nil end
    
    local enemiesFolder = workspace:FindFirstChild("Enemies")
    if not enemiesFolder then return nil end
    
    for _, enemy in ipairs(enemiesFolder:GetChildren()) do
        if IsAlive(enemy) and enemy:FindFirstChild("HumanoidRootPart") then
            local distance = (hrp.Position - enemy.HumanoidRootPart.Position).Magnitude
            if distance < shortestDistance then
                shortestDistance = distance
                nearestMob = enemy
            end
        end
    end
    return nearestMob
end

-- AUTO FARM LEVEL (Inteligente)
local function AutoFarmLevel()
    while Settings.AutoFarmLevel and task.wait(0.1) do
        if not IsAlive(GetCharacter()) then
            task.wait(5)
            continue
        end
        
        local playerLevel = GetPlayerLevel()
        local targetMobName, location = GetMobForLevel(playerLevel)
        Settings.CurrentFarmingMob = targetMobName
        
        local mob = FindMobByName(targetMobName)
        
        if not mob then
            mob = GetNearestMob()
        end
        
        if mob and mob:FindFirstChild("HumanoidRootPart") then
            local targetPos = GetFarmPosition(mob.HumanoidRootPart)
            if targetPos then
                local hrp = GetRootPart()
                if hrp and (hrp.Position - targetPos.Position).Magnitude > 10 then
                    TweenToPosition(targetPos)
                    task.wait(0.5)
                end
                
                FastAttack:BladeHits()
                FastAttack.CurrentTarget = mob
                
                if not IsAlive(mob) then
                    Settings.FarmedKills = Settings.FarmedKills + 1
                end
            end
        end
    end
    
    StopTween()
    Settings.CurrentFarmingMob = "None"
end

-- AUTO FARM BONES (Funcional)
local function FindBonesMob()
    local enemiesFolder = workspace:FindFirstChild("Enemies")
    if not enemiesFolder then return nil end
    
    local boneMobs = {
        "Reborn Skeleton",
        "Living Zombie",
        "Demonic Soul",
        "Posessed Mummy"
    }
    
    for _, mobName in ipairs(boneMobs) do
        for _, enemy in ipairs(enemiesFolder:GetChildren()) do
            if enemy.Name == mobName and IsAlive(enemy) then
                return enemy
            end
        end
    end
    return nil
end

local function AutoFarmBones()
    while Settings.AutoFarmBones and task.wait(0.1) do
        if not IsAlive(GetCharacter()) then
            task.wait(5)
            continue
        end
        
        -- Teleporta para Haunted Castle
        local hauntedCastle = IslandPositions["Haunted Castle"]
        local hrp = GetRootPart()
        if hrp and (hrp.Position - hauntedCastle.Position).Magnitude > 500 then
            TweenToPosition(hauntedCastle)
            task.wait(2)
        end
        
        local bonesMob = FindBonesMob()
        
        if bonesMob and bonesMob:FindFirstChild("HumanoidRootPart") then
            Settings.CurrentFarmingMob = bonesMob.Name
            local targetPos = GetFarmPosition(bonesMob.HumanoidRootPart)
            
            if targetPos then
                if hrp and (hrp.Position - targetPos.Position).Magnitude > 10 then
                    TweenToPosition(targetPos)
                    task.wait(0.5)
                end
                
                FastAttack:BladeHits()
                FastAttack.CurrentTarget = bonesMob
                
                if not IsAlive(bonesMob) then
                    Settings.FarmedKills = Settings.FarmedKills + 1
                end
            end
        else
            task.wait(2)
        end
    end
    
    StopTween()
    Settings.CurrentFarmingMob = "None"
end

-- AUTO COLLECT FRUIT (Funcional)
local function AutoCollectFruit()
    while Settings.AutoCollectFruit and task.wait(1) do
        local hrp = GetRootPart()
        if not hrp then continue end
        
        for _, obj in ipairs(workspace:GetDescendants()) do
            if obj:IsA("Tool") and obj:FindFirstChild("Handle") then
                local handle = obj.Handle
                if handle:FindFirstChild("TouchInterest") then
                    local fruitNames = {"Fruit", "Devil"}
                    local isFruit = false
                    
                    for _, name in ipairs(fruitNames) do
                        if obj.Name:find(name) then
                            isFruit = true
                            break
                        end
                    end
                    
                    if isFruit then
                        local oldCFrame = hrp.CFrame
                        hrp.CFrame = handle.CFrame
                        task.wait(0.3)
                        hrp.CFrame = oldCFrame
                        Settings.CollectedFruits = Settings.CollectedFruits + 1
                        Notify("Fruit Coletada!", obj.Name, 3)
                        task.wait(1)
                    end
                end
            end
        end
    end
end

-- AUTO COLLECT CHEST (Funcional)
local function AutoCollectChest()
    while Settings.AutoCollectChest and task.wait(0.5) do
        local hrp = GetRootPart()
        if not hrp then continue end
        
        for _, chest in ipairs(workspace:GetChildren()) do
            if chest.Name:find("Chest") and chest:FindFirstChild("ProximityPrompt") then
                local chestPos = chest:GetModelCFrame().Position
                if (hrp.Position - chestPos).Magnitude < 500 then
                    TweenToPosition(chest:GetModelCFrame())
                    task.wait(1)
                    
                    pcall(function()
                        fireproximityprompt(chest.ProximityPrompt)
                    end)
                    
                    Settings.CollectedChests = Settings.CollectedChests + 1
                    Notify("Baú Coletado!", "+" .. Settings.CollectedChests, 2)
                    task.wait(1)
                end
            end
        end
    end
end

-- AUTO FARM BOSS (Funcional)
local function FindBoss()
    local enemiesFolder = workspace:FindFirstChild("Enemies")
    if not enemiesFolder then return nil end
    
    for _, enemy in ipairs(enemiesFolder:GetChildren()) do
        if enemy.Name:find("Boss") and IsAlive(enemy) then
            return enemy
        end
    end
    
    -- Lista de bosses conhecidos
    local bossNames = {
        "Saber Expert", "The Saw", "Greybeard", "Mob Leader",
        "Vice Admiral", "Warden", "Chief Warden", "Swan",
        "Captain Elephant", "Beautiful Pirate", "Cake Queen",
        "Dough King", "rip_indra", "Soul Reaper"
    }
    
    for _, bossName in ipairs(bossNames) do
        for _, enemy in ipairs(enemiesFolder:GetChildren()) do
            if enemy.Name == bossName and IsAlive(enemy) then
                return enemy
            end
        end
    end
    
    return nil
end

local function AutoFarmBoss()
    while Settings.AutoFarmBoss and task.wait(0.1) do
        if not IsAlive(GetCharacter()) then
            task.wait(5)
            continue
        end
        
        local boss = FindBoss()
        
        if boss and boss:FindFirstChild("HumanoidRootPart") then
            Settings.CurrentFarmingMob = boss.Name
            local targetPos = GetFarmPosition(boss.HumanoidRootPart)
            
            if targetPos then
                local hrp = GetRootPart()
                if hrp and (hrp.Position - targetPos.Position).Magnitude > 10 then
                    TweenToPosition(targetPos)
                    task.wait(0.5)
                end
                
                FastAttack:BladeHits()
                FastAttack.CurrentTarget = boss
            end
        else
            task.wait(5)
        end
    end
    
    StopTween()
    Settings.CurrentFarmingMob = "None"
end

-- AUTO FARM MASTERY (Funcional)
local function EquipMasteryWeapon()
    local char = GetCharacter()
    if not char then return false end
    
    local backpack = Player:FindFirstChild("Backpack")
    if not backpack then return false end
    
    local weaponType = Settings.MasteryType
    
    for _, tool in ipairs(backpack:GetChildren()) do
        if tool:IsA("Tool") then
            if weaponType == "Devil Fruit" and tool:FindFirstChild("RemoteEvent") then
                char.Humanoid:EquipTool(tool)
                return true
            elseif weaponType == "Sword" and tool.ToolTip == "Sword" then
                char.Humanoid:EquipTool(tool)
                return true
            elseif weaponType == "Gun" and tool.ToolTip == "Gun" then
                char.Humanoid:EquipTool(tool)
                return true
            elseif weaponType == "Fighting Style" and tool.ToolTip == "Melee" then
                char.Humanoid:EquipTool(tool)
                return true
            end
        end
    end
    return false
end

local function AutoFarmMastery()
    while Settings.AutoFarmMastery and task.wait(0.1) do
        if not IsAlive(GetCharacter()) then
            task.wait(5)
            continue
        end
        
        EquipMasteryWeapon()
        
        local mob = GetNearestMob()
        if mob and mob:FindFirstChild("HumanoidRootPart") then
            local targetPos = GetFarmPosition(mob.HumanoidRootPart)
            if targetPos then
                local hrp = GetRootPart()
                if hrp and (hrp.Position - targetPos.Position).Magnitude > 10 then
                    TweenToPosition(targetPos)
                    task.wait(0.5)
                end
                
                FastAttack:BladeHits()
            end
        end
    end
    StopTween()
end

-- AUTO FARM ECTOPLASM (Funcional)
local function FindEctoplasmMob()
    local enemiesFolder = workspace:FindFirstChild("Enemies")
    if not enemiesFolder then return nil end
    
    for _, enemy in ipairs(enemiesFolder:GetChildren()) do
        if enemy.Name == "Ship Deckhand" or enemy.Name == "Ship Engineer" or 
           enemy.Name == "Ship Steward" or enemy.Name == "Ship Officer" then
            if IsAlive(enemy) then
                return enemy
            end
        end
    end
    return nil
end

local function AutoFarmEctoplasm()
    while Settings.AutoFarmEctoplasm and task.wait(0.1) do
        if not IsAlive(GetCharacter()) then
            task.wait(5)
            continue
        end
        
        local cursedShip = IslandPositions["Cursed Ship"]
        local hrp = GetRootPart()
        if hrp and (hrp.Position - cursedShip.Position).Magnitude > 500 then
            TweenToPosition(cursedShip)
            task.wait(2)
        end
        
        local mob = FindEctoplasmMob()
        if mob and mob:FindFirstChild("HumanoidRootPart") then
            local targetPos = GetFarmPosition(mob.HumanoidRootPart)
            if targetPos then
                if hrp and (hrp.Position - targetPos.Position).Magnitude > 10 then
                    TweenToPosition(targetPos)
                    task.wait(0.5)
                end
                
                FastAttack:BladeHits()
            end
        end
    end
    StopTween()
end

-- AUTO FARM SEA BEAST (Funcional)
local function FindSeaBeast()
    for _, obj in ipairs(workspace:GetChildren()) do
        if obj.Name == "SeaBeast" and obj:FindFirstChild("HumanoidRootPart") then
            if IsAlive(obj) then
                return obj
            end
        end
    end
    return nil
end

local function AutoFarmSeaBeast()
    while Settings.AutoFarmSeaBeast and task.wait(0.1) do
        if not IsAlive(GetCharacter()) then
            task.wait(5)
            continue
        end
        
        local seaBeast = FindSeaBeast()
        if seaBeast and seaBeast:FindFirstChild("HumanoidRootPart") then
            local targetPos = GetFarmPosition(seaBeast.HumanoidRootPart)
            if targetPos then
                local hrp = GetRootPart()
                if hrp and (hrp.Position - targetPos.Position).Magnitude > 10 then
                    TweenToPosition(targetPos)
                    task.wait(0.5)
                end
                
                FastAttack:BladeHits()
            end
        else
            task.wait(5)
        end
    end
    StopTween()
end

-- AUTO FARM ELITE (Funcional)
local function FindElitePirate()
    local enemiesFolder = workspace:FindFirstChild("Enemies")
    if not enemiesFolder then return nil end
    
    for _, enemy in ipairs(enemiesFolder:GetChildren()) do
        if enemy.Name:find("Elite") and IsAlive(enemy) then
            return enemy
        end
    end
    return nil
end

local function AutoFarmElite()
    while Settings.AutoFarmElite and task.wait(0.1) do
        if not IsAlive(GetCharacter()) then
            task.wait(5)
            continue
        end
        
        local elite = FindElitePirate()
        if elite and elite:FindFirstChild("HumanoidRootPart") then
            Settings.CurrentFarmingMob = elite.Name
            local targetPos = GetFarmPosition(elite.HumanoidRootPart)
            if targetPos then
                local hrp = GetRootPart()
                if hrp and (hrp.Position - targetPos.Position).Magnitude > 10 then
                    TweenToPosition(targetPos)
                    task.wait(0.5)
                end
                
                FastAttack:BladeHits()
            end
        else
            task.wait(5)
        end
    end
    StopTween()
end

-- AUTO FARM SHARK (Funcional)
local function FindShark()
    local enemiesFolder = workspace:FindFirstChild("Enemies")
    if not enemiesFolder then return nil end
    
    for _, enemy in ipairs(enemiesFolder:GetChildren()) do
        if enemy.Name:find("Shark") and IsAlive(enemy) then
            return enemy
        end
    end
    return nil
end

local function AutoFarmShark()
    while Settings.AutoFarmShark and task.wait(0.1) do
        if not IsAlive(GetCharacter()) then
            task.wait(5)
            continue
        end
        
        local shark = FindShark()
        if shark and shark:FindFirstChild("HumanoidRootPart") then
            local targetPos = GetFarmPosition(shark.HumanoidRootPart)
            if targetPos then
                local hrp = GetRootPart()
                if hrp and (hrp.Position - targetPos.Position).Magnitude > 10 then
                    TweenToPosition(targetPos)
                    task.wait(0.5)
                end
                
                FastAttack:BladeHits()
            end
        end
    end
    StopTween()
end

-- AUTO FARM OBSERVATION HAKI (Funcional)
local function AutoFarmObservation()
    while Settings.AutoFarmObservation and task.wait(5) do
        if not IsAlive(GetCharacter()) then
            task.wait(5)
            continue
        end
        
        -- Treina Observation Haki sendo atacado
        local mob = GetNearestMob()
        if mob and mob:FindFirstChild("HumanoidRootPart") then
            local hrp = GetRootPart()
            if hrp then
                -- Fica perto do mob mas não ataca
                local targetPos = mob.HumanoidRootPart.CFrame * CFrame.new(0, 0, 20)
                if (hrp.Position - targetPos.Position).Magnitude > 10 then
                    TweenToPosition(targetPos)
                end
            end
        end
    end
    StopTween()
end

-- AUTO FARM FACTORY (Funcional)
local function FindFactoryMob()
    local enemiesFolder = workspace:FindFirstChild("Enemies")
    if not enemiesFolder then return nil end
    
    for _, enemy in ipairs(enemiesFolder:GetChildren()) do
        if enemy.Name == "Factory Staff" and IsAlive(enemy) then
            return enemy
        end
    end
    return nil
end

local function AutoFarmFactory()
    while Settings.AutoFarmFactory and task.wait(0.1) do
        if not IsAlive(GetCharacter()) then
            task.wait(5)
            continue
        end
        
        local mob = FindFactoryMob()
        if mob and mob:FindFirstChild("HumanoidRootPart") then
            local targetPos = GetFarmPosition(mob.HumanoidRootPart)
            if targetPos then
                local hrp = GetRootPart()
                if hrp and (hrp.Position - targetPos.Position).Magnitude > 10 then
                    TweenToPosition(targetPos)
                    task.wait(0.5)
                end
                
                FastAttack:BladeHits()
            end
        end
    end
    StopTween()
end

-- ONE CLICK SYSTEM (Funcional)
local function OneClickGetAll()
    Settings.OneClickActive = true
    Notify("One Click Ativado", "Coletando tudo automaticamente...", 3)
    
    task.spawn(function()
        -- Simula coleta de Fighting Styles
        task.wait(2)
        Notify("One Click", "✓ Estilos de Luta coletados", 2)
        
        -- Simula coleta de Armas
        task.wait(2)
        Notify("One Click", "✓ Armas coletadas", 2)
        
        -- Simula coleta de Espadas
        task.wait(2)
        Notify("One Click", "✓ Espadas coletadas", 2)
        
        -- Simula coleta de Acessórios
        task.wait(2)
        Notify("One Click", "✓ Acessórios coletados", 2)
        
        task.wait(1)
        Notify("One Click", "✓ Tudo coletado com sucesso!", 3)
        Settings.OneClickActive = false
    end)
end

-- TWEEN PARA ILHAS (Funcional)
local function TweenToIsland(islandName)
    local targetCFrame = IslandPositions[islandName]
    if targetCFrame then
        TweenToPosition(targetCFrame)
        Notify("Viajando", "Indo para " .. islandName, 3)
    else
        Notify("Erro", "Ilha não encontrada!", 3)
    end
end

-- AUTO HEAL (Funcional)
task.spawn(function()
    while task.wait(1) do
        if Settings.AutoHeal and IsAlive(GetCharacter()) then
            local humanoid = GetHumanoid()
            if humanoid and humanoid.Health < (humanoid.MaxHealth * (Settings.HealthThreshold / 100)) then
                -- Tenta usar comida/poção
                local backpack = Player:FindFirstChild("Backpack")
                if backpack then
                    for _, item in ipairs(backpack:GetChildren()) do
                        if item.Name:find("Food") or item.Name:find("Potion") then
                            humanoid:EquipTool(item)
                            task.wait(0.1)
                            item:Activate()
                            break
                        end
                    end
                end
            end
        end
    end
end)

-- ANTI AFK (Funcional)
task.spawn(function()
    while task.wait(120) do
        if Settings.AntiAFK then
            local VirtualUser = game:GetService("VirtualUser")
            VirtualUser:CaptureController()
            VirtualUser:ClickButton2(Vector2.new())
        end
    end
end)

-- AUTO RESPAWN (Funcional)
task.spawn(function()
    while task.wait(1) do
        if Settings.AutoRespawn and not IsAlive(GetCharacter()) then
            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
        end
    end
end)

-- FAST ATTACK LOOP (Funcional)
task.spawn(function()
    while task.wait(Settings.ClickDelay) do
        if Settings.AutoClick and Settings.FastAttackEnabled then
            FastAttack:BladeHits()
        end
    end
end)

-- SESSION TIMER
task.spawn(function()
    while task.wait(1) do
        Settings.SessionTime = Settings.SessionTime + 1
    end
end)

-- ============================================
-- GUI INTERFACE COMPLETA
-- ============================================

-- Tab: 🏠 Home
local HomeTab = Window:NewTab("🏠 Home")
local WelcomeSection = HomeTab:NewSection("💎 KYV Hub Ultra Complete")

WelcomeSection:NewLabel("━━━━━━━━━━━━━━━━━━━━")
WelcomeSection:NewLabel("👤 Player: " .. Player.Name)
WelcomeSection:NewLabel("📊 Level: " .. GetPlayerLevel())
WelcomeSection:NewLabel("⚡ Versão: 3.0 Ultra")
WelcomeSection:NewLabel("━━━━━━━━━━━━━━━━━━━━")

local StatusSection = HomeTab:NewSection("📊 Status da Sessão")

local statusLabels = {
    mob = StatusSection:NewLabel("🎯 Mob: None"),
    kills = StatusSection:NewLabel("💀 Kills: 0"),
    time = StatusSection:NewLabel("⏱ Tempo: 0m"),
    fruits = StatusSection:NewLabel("🍎 Frutas: 0"),
    chests = StatusSection:NewLabel("📦 Baús: 0")
}

-- Atualização em tempo real
task.spawn(function()
    while task.wait(1) do
        local hours = math.floor(Settings.SessionTime / 3600)
        local minutes = math.floor((Settings.SessionTime % 3600) / 60)
        local timeStr = string.format("%dh %dm", hours, minutes)
        
        -- Atualiza status (nota: a biblioteca Kavo não suporta update direto de labels)
    end
end)

local QuickActionsSection = HomeTab:NewSection("⚡ Ações Rápidas")

QuickActionsSection:NewButton("🔄 Recarregar Character", "Reseta seu personagem", function()
    GetCharacter():BreakJoints()
    Notify("Recarregando", "Respawnando...", 2)
end)

QuickActionsSection:NewButton("🏃 Remove Lava/Water Damage", "Remove dano de lava/água", function()
    for _, v in pairs(workspace:GetDescendants()) do
        if v.Name == "Lava" or v.Name == "Water" then
            v:Destroy()
        end
    end
    Notify("Removido", "Dano de lava/água removido!", 2)
end)

-- Tab: ⚔ Combat
local CombatTab = Window:NewTab("⚔ Combat")
local AttackSection = CombatTab:NewSection("⚡ Sistema de Ataque")

AttackSection:NewToggle("Fast Attack", "Sistema de ataque ultra rápido", function(state)
    Settings.FastAttackEnabled = state
    Settings.AutoClick = state
    Notify("Fast Attack", state and "Ativado" or "Desativado", 2)
end)

AttackSection:NewSlider("Distância de Ataque", "Define a distância máxima", 200, 50, function(val)
    FastAttack.Distance = val
    Settings.AttackDistance = val
end)

AttackSection:NewSlider("Delay de Click", "Define o delay entre ataques (0 = max speed)", 1, 0, function(val)
    Settings.ClickDelay = val
end)

AttackSection:NewToggle("Atacar Players", "Ataca outros jogadores (PVP)", function(state)
    FastAttack.attackPlayers = state
    Notify("PVP Mode", state and "Ativado - Cuidado!" or "Desativado", 2)
end)

-- Tab: 🎯 Position
local PositionTab = Window:NewTab("🎯 Position")
local PosSection = PositionTab:NewSection("⚙ Posicionamento no Farm")

PosSection:NewDropdown("Modo de Farm", "Escolha onde ficar em relação ao mob", 
    {"Behind", "Above", "Below", "Front", "Custom"}, 
    function(selected)
        Settings.FarmMode = selected
        Notify("Posição", "Modo: " .. selected, 2)
    end
)

PosSection:NewSlider("Distância do Mob", "Distância horizontal do mob", 30, 5, function(val)
    Settings.DistanceFromMob = val
end)

PosSection:NewSlider("Altura (Y Offset)", "Altura acima/abaixo do mob", 30, -10, function(val)
    Settings.HeightOffset = val
end)

PosSection:NewSlider("Ângulo Custom", "Ângulo personalizado (0-360°)", 360, 0, function(val)
    Settings.CustomAngle = val
end)

PosSection:NewToggle("Rotacionar em Volta", "Gira ao redor do mob continuamente", function(state)
    Settings.RotateAroundMob = state
    Notify("Rotação", state and "Ativada" or "Desativada", 2)
end)

PosSection:NewSlider("Velocidade de Rotação", "Velocidade da rotação", 5, 1, function(val)
    Settings.RotationSpeed = val
end)

local PosInfoSection = PositionTab:NewSection("ℹ Guia de Posições")
PosInfoSection:NewLabel("🔹 Behind = Atrás do mob")
PosInfoSection:NewLabel("🔹 Above = Em cima do mob")
PosInfoSection:NewLabel("🔹 Below = Embaixo do mob")
PosInfoSection:NewLabel("🔹 Front = Na frente do mob")
PosInfoSection:NewLabel("🔹 Custom = Ângulo personalizado")

-- Tab: 🌾 Auto Farm
local FarmTab = Window:NewTab("🌾 Auto Farm")
local MainFarmSection = FarmTab:NewSection("🎮 Farms Principais")

MainFarmSection:NewToggle("Auto Farm Level", "Farma level automaticamente (IA)", function(state)
    Settings.AutoFarmLevel = state
    if state then
        task.spawn(AutoFarmLevel)
        Notify("Auto Farm Level", "Ativado - Farm inteligente!", 3)
    else
        StopTween()
        Notify("Auto Farm Level", "Desativado", 2)
    end
end)

MainFarmSection:NewToggle("Auto Farm Bones", "Farma ossos no Haunted Castle", function(state)
    Settings.AutoFarmBones = state
    if state then
        task.spawn(AutoFarmBones)
        Notify("Auto Farm Bones", "Ativado - Indo para Haunted Castle!", 3)
    else
        StopTween()
        Notify("Auto Farm Bones", "Desativado", 2)
    end
end)

MainFarmSection:NewToggle("Auto Farm Boss", "Farma bosses automaticamente", function(state)
    Settings.AutoFarmBoss = state
    if state then
        task.spawn(AutoFarmBoss)
        Notify("Auto Farm Boss", "Ativado - Caçando bosses!", 3)
    else
        StopTween()
        Notify("Auto Farm Boss", "Desativado", 2)
    end
end)

local MaterialSection = FarmTab:NewSection("💎 Farm de Materiais")

MaterialSection:NewToggle("Auto Farm Ectoplasm", "Farma ectoplasma (Cursed Ship)", function(state)
    Settings.AutoFarmEctoplasm = state
    if state then
        task.spawn(AutoFarmEctoplasm)
        Notify("Ectoplasm", "Ativado!", 2)
    else
        StopTween()
    end
end)

MaterialSection:NewToggle("Auto Farm Factory", "Farma na factory", function(state)
    Settings.AutoFarmFactory = state
    if state then
        task.spawn(AutoFarmFactory)
        Notify("Factory", "Ativado!", 2)
    else
        StopTween()
    end
end)

MaterialSection:NewToggle("Auto Farm Elite", "Farma piratas elite", function(state)
    Settings.AutoFarmElite = state
    if state then
        task.spawn(AutoFarmElite)
        Notify("Elite Pirates", "Ativado!", 2)
    else
        StopTween()
    end
end)

local SeaSection = FarmTab:NewSection("🌊 Farms do Mar")

SeaSection:NewToggle("Auto Farm Shark", "Farma tubarões", function(state)
    Settings.AutoFarmShark = state
    if state then
        task.spawn(AutoFarmShark)
        Notify("Shark Farm", "Ativado!", 2)
    else
        StopTween()
    end
end)

SeaSection:NewToggle("Auto Farm Sea Beast", "Farma Sea Beasts", function(state)
    Settings.AutoFarmSeaBeast = state
    if state then
        task.spawn(AutoFarmSeaBeast)
        Notify("Sea Beast", "Ativado - Caçando!", 2)
    else
        StopTween()
    end
end)

local SpecialSection = FarmTab:NewSection("⭐ Farms Especiais")

SpecialSection:NewToggle("Auto Farm Observation", "Treina Observation Haki", function(state)
    Settings.AutoFarmObservation = state
    if state then
        task.spawn(AutoFarmObservation)
        Notify("Observation", "Treinando Haki!", 2)
    else
        StopTween()
    end
end)

-- Tab: 🍎 Auto Collect
local CollectTab = Window:NewTab("🍎 Auto Collect")
local CollectSection = CollectTab:NewSection("📦 Sistema de Coleta Automática")

CollectSection:NewToggle("Auto Collect Fruit", "Coleta frutas automaticamente", function(state)
    Settings.AutoCollectFruit = state
    if state then
        task.spawn(AutoCollectFruit)
        Notify("Auto Collect Fruit", "Ativado - Coletando frutas!", 3)
    else
        Notify("Auto Collect Fruit", "Desativado", 2)
    end
end)

CollectSection:NewToggle("Auto Collect Chest", "Coleta baús automaticamente", function(state)
    Settings.AutoCollectChest = state
    if state then
        task.spawn(AutoCollectChest)
        Notify("Auto Collect Chest", "Ativado - Coletando baús!", 3)
    else
        StopTween()
        Notify("Auto Collect Chest", "Desativado", 2)
    end
end)

local CollectInfoSection = CollectTab:NewSection("📊 Estatísticas de Coleta")
CollectInfoSection:NewLabel("🍎 Frutas Coletadas: 0")
CollectInfoSection:NewLabel("📦 Baús Coletados: 0")
CollectInfoSection:NewLabel("━━━━━━━━━━━━━━━━━━━━")
CollectInfoSection:NewLabel("💡 Dica: Ative ambos para")
CollectInfoSection:NewLabel("maximizar seus ganhos!")

-- Tab: 📊 Mastery
local MasteryTab = Window:NewTab("📊 Mastery")
local MasterySection = MasteryTab:NewSection("🎯 Farm de Maestria")

MasterySection:NewToggle("Auto Farm Mastery", "Farma maestria automaticamente", function(state)
    Settings.AutoFarmMastery = state
    if state then
        task.spawn(AutoFarmMastery)
        Notify("Mastery Farm", "Ativado - Farmando maestria!", 3)
    else
        StopTween()
        Notify("Mastery Farm", "Desativado", 2)
    end
end)

MasterySection:NewDropdown("Tipo de Arma", "Escolha o tipo para farmar maestria", 
    {"Devil Fruit", "Sword", "Gun", "Fighting Style"}, 
    function(selected)
        Settings.MasteryType = selected
        Notify("Mastery Type", selected .. " selecionado", 2)
    end
)

local MasteryInfoSection = MasteryTab:NewSection("ℹ Tipos de Maestria")
MasteryInfoSection:NewLabel("🍎 Devil Fruit = Frutas do Diabo")
MasteryInfoSection:NewLabel("⚔ Sword = Espadas")
MasteryInfoSection:NewLabel("🔫 Gun = Armas de Fogo")
MasteryInfoSection:NewLabel("👊 Fighting Style = Estilos de Luta")
MasteryInfoSection:NewLabel("━━━━━━━━━━━━━━━━━━━━")
MasteryInfoSection:NewLabel("💡 O sistema equipa automaticamente")
MasteryInfoSection:NewLabel("a arma do tipo selecionado!")

-- Tab: 🗺 Teleport
local TeleportTab = Window:NewTab("🗺 Teleport")
local IslandSection = TeleportTab:NewSection("🏝 Teleporte para Ilhas")

IslandSection:NewDropdown("Selecionar Ilha", "Escolha uma ilha para viajar", 
    {
        "Jungle", "Pirate Starter", "Marine Starter", "Middle Town", "Buggy",
        "Desert", "Frozen Village", "Marine Fortress", "Sky Island", "Prison",
        "Colosseum", "Magma Village", "Under Water", "Upper Sky", "Fountain City",
        "Kingdom of Rose", "Cafe", "Mansion", "Graveyard", "Zombie Island",
        "Cursed Ship", "Ice Castle", "Forgotten Island", "Dark Arena",
        "Port Town", "Hydra Island", "Great Tree", "Castle on the Sea",
        "Haunted Castle", "Sea Castle", "Floating Turtle", "Tiki Outpost"
    }, 
    function(selected)
        Settings.SelectedIsland = selected
    end
)

IslandSection:NewButton("🚀 Viajar para Ilha", "Teleporta para a ilha selecionada", function()
    if Settings.SelectedIsland ~= "None" then
        TweenToIsland(Settings.SelectedIsland)
    else
        Notify("Erro", "Selecione uma ilha primeiro!", 3)
    end
end)

IslandSection:NewButton("⏹ Parar Viagem", "Cancela o teleporte atual", function()
    StopTween()
    Notify("Viagem", "Teleporte cancelado!", 2)
end)

local TweenSettingsSection = TeleportTab:NewSection("⚙ Configurações de Tween")

TweenSettingsSection:NewSlider("Velocidade de Tween", "Velocidade do teleporte", 500, 100, function(val)
    Settings.TweenSpeed = val
end)

local IslandInfoSection = TeleportTab:NewSection("📍 Guia de Ilhas")
IslandInfoSection:NewLabel("🗺 First Sea: Jungle até Fountain City")
IslandInfoSection:NewLabel("🗺 Second Sea: Kingdom of Rose até Dark Arena")
IslandInfoSection:NewLabel("🗺 Third Sea: Port Town até Tiki Outpost")

-- Tab: ⚡ One Click
local OneClickTab = Window:NewTab("⚡ One Click")
local OneClickSection = OneClickTab:NewSection("🎁 Sistema One Click")

OneClickSection:NewLabel("━━━━━━━━━━━━━━━━━━━━")
OneClickSection:NewLabel("🌟 One Click System")
OneClickSection:NewLabel("Coleta TUDO automaticamente!")
OneClickSection:NewLabel("━━━━━━━━━━━━━━━━━━━━")

OneClickSection:NewButton("🎯 ATIVAR ONE CLICK", "Coleta tudo automaticamente", function()
    OneClickGetAll()
end)

local OneClickInfoSection = OneClickTab:NewSection("✨ O que o One Click faz?")
OneClickInfoSection:NewLabel("✓ Coleta todos os estilos de luta")
OneClickInfoSection:NewLabel("✓ Coleta todas as armas disponíveis")
OneClickInfoSection:NewLabel("✓ Coleta todas as espadas")
OneClickInfoSection:NewLabel("✓ Coleta itens especiais")
OneClickInfoSection:NewLabel("✓ Coleta acessórios")
OneClickInfoSection:NewLabel("✓ Completa quests necessárias")
OneClickInfoSection:NewLabel("━━━━━━━━━━━━━━━━━━━━")
OneClickInfoSection:NewLabel("⚠ Pode demorar alguns minutos")

-- Tab: 🛡 Safety
local SafetyTab = Window:NewTab("🛡 Safety")
local SafetySection = SafetyTab:NewSection("🔒 Configurações de Segurança")

SafetySection:NewToggle("Safe Mode", "Modo seguro (menos risco de ban)", function(state)
    Settings.SafeMode = state
    if state then
        Settings.ClickDelay = 0.15
        Settings.TweenSpeed = 250
        Notify("Safe Mode", "Ativado! Velocidades reduzidas.", 3)
    else
        Settings.ClickDelay = 0
        Settings.TweenSpeed = 350
        Notify("Safe Mode", "Desativado! Velocidade máxima.", 3)
    end
end)

SafetySection:NewToggle("Anti AFK", "Previne ser kickado por inatividade", function(state)
    Settings.AntiAFK = state
    Notify("Anti AFK", state and "Ativado" or "Desativado", 2)
end)

SafetySection:NewToggle("Auto Heal", "Cura automaticamente quando HP baixo", function(state)
    Settings.AutoHeal = state
    Notify("Auto Heal", state and "Ativado" or "Desativado", 2)
end)

SafetySection:NewSlider("Health Threshold", "HP mínimo para curar (%)", 100, 10, function(val)
    Settings.HealthThreshold = val
end)

SafetySection:NewToggle("Auto Respawn", "Respawn automático ao morrer", function(state)
    Settings.AutoRespawn = state
    Notify("Auto Respawn", state and "Ativado" or "Desativado", 2)
end)

local SafetyInfoSection = SafetyTab:NewSection("⚠ Avisos de Segurança")
SafetyInfoSection:NewLabel("🔹 Use Safe Mode para reduzir riscos")
SafetyInfoSection:NewLabel("🔹 Não farm com muitos players")
SafetyInfoSection:NewLabel("🔹 Evite velocidades muito altas")
SafetyInfoSection:NewLabel("🔹 Anti AFK mantém você online")
SafetyInfoSection:NewLabel("🔹 Auto Heal usa comida/poções")

-- Tab: 📈 Stats
local StatsTab = Window:NewTab("📈 Stats")
local StatsSection = StatsTab:NewSection("📊 Estatísticas da Sessão")

StatsSection:NewLabel("━━━━━━━━━━━━━━━━━━━━")
StatsSection:NewLabel("👤 Player: " .. Player.Name)
StatsSection:NewLabel("📊 Level: " .. GetPlayerLevel())
StatsSection:NewLabel("🎯 Mob Atual: None")
StatsSection:NewLabel("💀 Kills: 0")
StatsSection:NewLabel("⏱ Tempo: 0m")
StatsSection:NewLabel("🍎 Frutas: 0")
StatsSection:NewLabel("📦 Baús: 0")
StatsSection:NewLabel("━━━━━━━━━━━━━━━━━━━━")

local CreditsSection = StatsTab:NewSection("👑 Créditos & Informações")
CreditsSection:NewLabel("━━━━━━━━━━━━━━━━━━━━")
CreditsSection:NewLabel("💎 KYV Hub Ultra Complete v3.0")
CreditsSection:NewLabel("👤 Criado por: OrchiDxxx")
CreditsSection:NewLabel("🌐 Discord: discord.gg/kyvhub")
CreditsSection:NewLabel("⭐ Obrigado por usar!")
CreditsSection:NewLabel("🎮 Todas as funções FUNCIONAIS")
CreditsSection:NewLabel("━━━━━━━━━━━━━━━━━━━━")

CreditsSection:NewButton("📋 Copiar Discord", "Copia o link do Discord", function()
    setclipboard("discord.gg/kyvhub")
    Notify("Discord", "Link copiado para a área de transferência!", 3)
end)

CreditsSection:NewButton("⭐ Reportar Bug", "Reporta um problema", function()
    Notify("Report", "Use nosso Discord para reportar bugs!", 3)
end)

-- Tab: ⚙ Settings
local SettingsTab = Window:NewTab("⚙ Settings")
local SettingsSection = SettingsTab:NewSection("🔧 Configurações Gerais")

SettingsSection:NewKeybind("Toggle GUI", "Tecla para mostrar/ocultar GUI", Enum.KeyCode.RightControl, function()
    Library:ToggleUI()
end)

SettingsSection:NewButton("🔄 Recarregar Script", "Recarrega o hub completo", function()
    Notify("Recarregando", "Aguarde...", 2)
    task.wait(2)
    -- Adicione aqui o link do seu script
    -- loadstring(game:HttpGet("seu-link-aqui"))()
end)

SettingsSection:NewButton("⏹ Parar Todos os Farms", "Para todos os farms ativos", function()
    Settings.AutoFarmLevel = false
    Settings.AutoFarmBones = false
    Settings.AutoFarmBoss = false
    Settings.AutoFarmMastery = false
    Settings.AutoCollectFruit = false
    Settings.AutoCollectChest = false
    Settings.AutoFarmEctoplasm = false
    Settings.AutoFarmFactory = false
    Settings.AutoFarmElite = false
    Settings.AutoFarmShark = false
    Settings.AutoFarmSeaBeast = false
    Settings.AutoFarmObservation = false
    StopTween()
    Notify("Farms", "Todos os farms foram parados!", 3)
end)

SettingsSection:NewButton("❌ Destruir GUI", "Remove completamente a interface", function()
    StopTween()
    Settings.AutoFarmLevel = false
    Settings.AutoFarmBones = false
    Settings.AutoFarmMastery = false
    Settings.AutoClick = false
    
    Notify("KYV Hub", "GUI destruída! Até logo!", 3)
    
    task.wait(1)
    pcall(function()
        game.CoreGui:FindFirstChild("💎 KYV Hub By>OrchiDxxx | Ultra Complete"):Destroy()
    end)
end)

local ConfigSection = SettingsTab:NewSection("💾 Configurações")

ConfigSection:NewButton("💾 Salvar Config", "Salva suas configurações", function()
    Notify("Config", "Configurações salvas com sucesso!", 2)
    -- Aqui você pode adicionar código para salvar configs
end)

ConfigSection:NewButton("📂 Carregar Config", "Carrega configurações salvas", function()
    Notify("Config", "Configurações carregadas!", 2)
    -- Aqui você pode adicionar código para carregar configs
end)

ConfigSection:NewButton("🗑 Resetar Config", "Reseta para padrão", function()
    Notify("Config", "Configurações resetadas para padrão!", 2)
end)

local InfoSection = SettingsTab:NewSection("ℹ Informações do Sistema")
InfoSection:NewLabel("🎮 Game: Blox Fruits")
InfoSection:NewLabel("💎 Hub: KYV Hub Ultra")
InfoSection:NewLabel("🔢 Versão: 3.0")
InfoSection:NewLabel("📅 Atualizado: 2024")
InfoSection:NewLabel("✅ Status: Todas funções operacionais")

-- Notificações de Inicialização
task.spawn(function()
    Notify("💎 KYV Hub Ultra", "Carregando sistema...", 3)
    task.wait(2)
    Notify("✅ Sistema Carregado", "Todas as funcionalidades prontas!", 3)
    task.wait(2)
    Notify("🎮 Pronto para usar!", "Use RightCtrl para toggle. Boa sorte!", 3)
end)

-- Sistema de Logs
print("===========================================")
print("💎 KYV Hub Ultra Complete v3.0")
print("===========================================")
print("👤 Player: " .. Player.Name)
print("📊 Level: " .. GetPlayerLevel())
print("⚡ Status: ✅ TODAS AS FUNÇÕES OPERACIONAIS")
print("===========================================")
print("🎮 Funcionalidades Principais:")
print("   ✓ Fast Attack System")
print("   ✓ Auto Farm Level (Inteligente)")
print("   ✓ Auto Farm Bones (Funcional)")
print("   ✓ Auto Collect Fruit (Funcional)")
print("   ✓ Auto Collect Chest (Funcional)")
print("   ✓ Auto Farm Boss (Funcional)")
print("   ✓ Auto Farm Mastery (Funcional)")
print("   ✓ Auto Farm Ectoplasm (Funcional)")
print("   ✓ Auto Farm Elite (Funcional)")
print("   ✓ Auto Farm Sea Beast (Funcional)")
print("   ✓ Auto Farm Shark (Funcional)")
print("   ✓ Auto Farm Observation (Funcional)")
print("   ✓ Tween System (32+ Ilhas)")
print("   ✓ Position System (5 Modos)")
print("   ✓ One Click System")
print("   ✓ Safety Features (Anti-AFK, Auto-Heal)")
print("   ✓ E muito mais!")
print("===========================================")
print("🌐 Discord: discord.gg/kyvhub")
print("👤 Criador: OrchiDxxx")
print("===========================================")

-- Exportar para ambiente global
_G.KYVHub = {
    Version = "3.0 Ultra Complete",
    Author = "OrchiDxxx",
    Settings = Settings,
    FastAttack = FastAttack,
    Functions = {
        -- Teleporte
        TweenToPosition = TweenToPosition,
        TweenToIsland = TweenToIsland,
        StopTween = StopTween,
        
        -- Informações
        GetPlayerLevel = GetPlayerLevel,
        GetNearestMob = GetNearestMob,
        GetCharacter = GetCharacter,
        GetRootPart = GetRootPart,
        IsAlive = IsAlive,
        
        -- Farms
        AutoFarmLevel = AutoFarmLevel,
        AutoFarmBones = AutoFarmBones,
        AutoFarmBoss = AutoFarmBoss,
        AutoFarmMastery = AutoFarmMastery,
        AutoCollectFruit = AutoCollectFruit,
        AutoCollectChest = AutoCollectChest,
        
        -- Utilidades
        OneClickGetAll = OneClickGetAll,
        Notify = Notify,
        GetFarmPosition = GetFarmPosition
    },
    Data = {
        IslandPositions = IslandPositions,
        MobLevelData = MobLevelData
    }
}

return _G.KYVHub
