--==================================================================
-- HUB UNIFICADO: bananaXpvp
-- Base Original Funcional + V4 Farm (Limite de NPCs Puxados)
-- Auto Click TURBO (Apenas na função de Click)
--==================================================================

local ok, A = pcall(function()
    return loadstring(game:HttpGet("https://raw.githubusercontent.com/obiiyeuem/vthangsitink/refs/heads/main/zzzz.lua"))()
end)

if not ok or not A or type(A.CreateMain) ~= "function" then
    warn("[bananaXpvp] Erro ao carregar a UI.")
    return
end

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local HttpService = game:GetService("HttpService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local Workspace = game:GetService("Workspace")
local TeleportService = game:GetService("TeleportService")
local UserInputService = game:GetService("UserInputService")
local localPlayer = Players.LocalPlayer
local currentCamera = Workspace.CurrentCamera
local str = "BF_AutoBounty_Config.json"

-- VARIÁVEIS DE CONFIGURAÇÃO
local tbl = {
	"Hunt_Auto", "Hunt_AutoRun", "Hunt_AutoSelect", "Hunt_StartWait", "Hunt_UseLevel", "Hunt_MinLevel",
	"Hunt_AllowUnknown", "Hunt_NoSafe", "Hunt_NoArena", "Hunt_NoMarineAlly", "Hunt_MaxDist", "Hunt_FlySpeed",
	"Hunt_StopDist", "Hunt_AutoSoru", "Hunt_SoruInterval", "Hunt_AutoSkill", "Hunt_SkillInterval", "Hunt_AutoHop",
	"Hunt_HopInterval", "Hunt_HopIfEmpty", "Hunt_EmptyWait", "Hunt_Team", "Hunt_HopWaitCombat", "Hunt_HopCombatWait",
	"G_ServerRegion", "G_HopPlayerRange", "G_HopBountyRange", "Hunt_AutoAttack", "Hunt_AutoEquip", "Hunt_Weapons",
	"Hunt_SkillAim", "Hunt_SoruAim", "Hunt_RotateWeapon", "Hunt_AutoFlee", "Hunt_FleeHP", "Hunt_FleeHeight",
	"Hunt_FleeRecover", "Hunt_AutoStand", "Hunt_HopSkyUp", "Hunt_HopSkyHeight", "Hunt_KillHop", "Hunt_KillHopCount",
	"Hunt_NoKilledEnough", "G_FastAttack", "G_FastAttackMode", "G_AttackMobs", "G_AttackPlayers", "Hunt_AutoSave",
	"G_Language", "Hunt_TargetSwitchDelay", "Hunt_AttackSpeed", "Hunt_PositionMode", "G_Noclip",
	"Macro_Enabled", "Macro_Delay", "Macro_Weapons", "G_InactivityHop", "G_InactivityTime",
    "V4_Enabled", "V4_TweenSpeed", "V4_AutoActivate", "V4_PullCount"
}

local tbl2 = { "Fruta", "Soco", "Espada", "Arma" }
local tbl3 = {
	["Soco"] = { "Z", "X", "C" },
	["Espada"] = { "Z", "X" },
	["Arma"] = { "Z", "X" },
	["Fruta"] = { "Z", "X", "C", "V", "F" },
}

local flag = writefile ~= nil and readfile ~= nil and isfile ~= nil

local function fn()
	local tbl4 = {}
	for _, v in ipairs(tbl) do tbl4[v] = _G[v] end
	tbl4.Hunt_TypeSkills = {}
	for _, v in ipairs(tbl2) do
		tbl4.Hunt_TypeSkills[v] = {}
		for _, v2 in ipairs(tbl3[v]) do tbl4.Hunt_TypeSkills[v][v2] = _G.Hunt_TypeSkills[v][v2] ~= false end
	end
	tbl4.Hunt_Weapons = {}
	for _, v in ipairs(tbl2) do tbl4.Hunt_Weapons[v] = _G.Hunt_Weapons[v] == true end
	return tbl4
end

local function fn2()
	local tbl4 = {}
	for _, v in ipairs(tbl) do table.insert(tbl4, v .. "=" .. tostring(_G[v])) end
	for _, v in ipairs(tbl2) do
		for _, v2 in ipairs(tbl3[v]) do table.insert(tbl4, "Sk" .. v .. v2 .. "=" .. tostring(_G.Hunt_TypeSkills[v][v2])) end
	end
	for _, v in ipairs(tbl2) do table.insert(tbl4, "Weapon" .. v .. "=" .. tostring(_G.Hunt_Weapons[v])) end
	return table.concat(tbl4, "|")
end

local function fn3()
	if not flag then return false end
	return (pcall(function() writefile("BF_AutoBounty_Config.json", HttpService:JSONEncode(fn())) end))
end

local function fn4()
	if not flag then return false end
	local json = nil
	if not pcall(function() if isfile("BF_AutoBounty_Config.json") then json = readfile("BF_AutoBounty_Config.json") end end) or not json or json == "" then return false end
	local data = nil
	if not pcall(function() data = HttpService:JSONDecode(json) end) or type(data) ~= "table" then return false end
	for _, v in ipairs(tbl) do if data[v] ~= nil then _G[v] = data[v] end end
	if type(data.Hunt_TypeSkills) == "table" then
		for _, v in ipairs(tbl2) do
			local v2 = data.Hunt_TypeSkills[v]
			if type(v2) == "table" then for _, v3 in ipairs(tbl3[v]) do _G.Hunt_TypeSkills[v][v3] = v2[v3] ~= false end end
		end
	end
	if type(data.Hunt_Weapons) == "table" then
		for _, v in ipairs(tbl2) do _G.Hunt_Weapons[v] = data.Hunt_Weapons[v] == true end
	end
	return true
end

_G.Hunt_Auto = true
_G.Hunt_StartWait = 8
_G.Hunt_AutoSelect = true
_G.Hunt_Target = nil
_G.Hunt_UseLevel = true
_G.Hunt_MinLevel = 2100
_G.Hunt_AllowUnknown = true
_G.Hunt_NoSafe = true
_G.Hunt_NoArena = true
_G.Hunt_NoPvp = true
_G.Hunt_NoMarineAlly = true
_G.Hunt_MaxDist = 10000
_G.Hunt_FlySpeed = 160
_G.Hunt_StopDist = 6
_G.Hunt_TargetSwitchDelay = 3
_G.Hunt_AttackSpeed = 0.12
_G.Hunt_PositionMode = "Frente"
_G.G_Noclip = false
_G.Hunt_AutoSoru = true
_G.Hunt_SoruInterval = 13
_G.Hunt_AutoSkill = true
_G.Hunt_SkillInterval = 1
_G.G_InactivityHop = true
_G.G_InactivityTime = 30

-- PADRÕES V4
_G.V4_Enabled = false
_G.V4_TweenSpeed = 350
_G.V4_AutoActivate = true
_G.V4_PullCount = 5

_G.Macro_Enabled = true
_G.Macro_Delay = 2.5
_G.Macro_Weapons = { "Fruta", "Espada" }

_G.Hunt_TypeSkills = {
	["Soco"] = { Z = true, X = true, C = true },
	["Espada"] = { Z = true, X = true },
	["Arma"] = { Z = true, X = true },
	["Fruta"] = { Z = true, X = true, C = true, V = true, F = true },
}

_G.Hunt_AutoHop = true
_G.Hunt_HopInterval = 300
_G.Hunt_HopIfEmpty = true
_G.Hunt_EmptyWait = 15
_G.Hunt_HopWaitCombat = true
_G.Hunt_HopCombatWait = 60
_G.Hunt_Team = "Não trocar"
_G.Hunt_AutoAttack = true
_G.Hunt_AutoEquip = true
_G.Hunt_Weapons = { ["Fruta"] = true, ["Soco"] = true, ["Espada"] = true, ["Arma"] = true }
_G.Hunt_SkillAim = true
_G.Hunt_SoruAim = true
_G.Hunt_RotateWeapon = true
_G.Hunt_AutoFlee = true
_G.Hunt_FleeHP = 30
_G.Hunt_FleeHeight = 3500
_G.Hunt_FleeRecover = 100
_G.Hunt_FleeMaxTime = 30
_G.Hunt_AutoStand = true
_G.Hunt_HopSkyUp = true
_G.Hunt_HopSkyHeight = 3500
_G.Hunt_KillHop = true
_G.Hunt_KillHopCount = 3
_G.Hunt_NoKilledEnough = true
_G.Hunt_Fleeing = false
_G.Hunt_HopSkyTimer = false
_G.Hunt_HopSkyEmpty = false
_G.Hunt_ForceHop = false
_G.Hunt_HopTimer = nil
_G.Hunt_RotateIndex = 1

pcall(function() getgenv().IsServerHopping = false end)

_G.Hunt_AutoHaki = true
_G.Hunt_AutoKen = true
_G.Hunt_AutoV3 = true
_G.Hunt_AutoV4 = true
_G.Hunt_AutoRun = true
_G.Hunt_AutoSave = true
_G.G_FastAttack = false
_G.G_FastAttackMode = "Modo 2 (Usar se o 1 falhar)"
_G.G_AttackMobs = true
_G.G_AttackPlayers = true
_G.G_Language = "Português"
local v = fn4()
_G.Hunt_AutoAttack = true
local v2 = fn2()

-- ==========================================
-- NOVO SERVER HOP (DIRETO DA API - GARANTIDO)
-- ==========================================
local divinePlaceId = 135658482799046

local function ForceHopServer()
    if getgenv().IsServerHopping then return end
    getgenv().IsServerHopping = true
    
    local url = "https://games.roblox.com/v1/games/" .. divinePlaceId .. "/servers/Public?sortOrder=Asc&limit=100"
    local success, result = pcall(function() return game:HttpGet(url) end)
    
    if success and result then
        local data = HttpService:JSONDecode(result)
        if data and data.data then
            local servers = {}
            for _, srv in ipairs(data.data) do
                if srv.id ~= game.JobId and srv.playing < srv.maxPlayers then
                    table.insert(servers, srv)
                end
            end
            
            if #servers > 0 then
                table.sort(servers, function(a, b) return a.playing < b.playing end)
                local target = servers[1]
                
                print("[bananaXpvp] Teleportando para servidor: " .. target.id .. " (" .. target.playing .. " players)")
                pcall(function()
                    TeleportService:TeleportToPlaceInstance(divinePlaceId, target.id, localPlayer)
                end)
                return
            end
        end
    end
    
    print("[bananaXpvp] API falhou ou servidores cheios. Tentando Rejoin...")
    pcall(function()
        TeleportService:Teleport(divinePlaceId, localPlayer)
    end)
    
    task.wait(10)
    getgenv().IsServerHopping = false
end

-- ==========================================
-- LOOP DO NOCLIP E ATIVIDADE
-- ==========================================
_G.LastActivityTime = tick()

UserInputService.InputBegan:Connect(function(input, processed) if not processed then _G.LastActivityTime = tick() end end)
UserInputService.InputChanged:Connect(function(input, processed) if not processed then _G.LastActivityTime = tick() end end)

task.spawn(function()
    while task.wait(0.1) do
        if _G.G_Noclip and localPlayer.Character then
            for _, part in pairs(localPlayer.Character:GetDescendants()) do
                if part:IsA("BasePart") and part.CanCollide then
                    part.CanCollide = false
                end
            end
        end
    end
end)

task.spawn(function()
    while task.wait(1) do
        if _G.Hunt_AutoSave and flag then
            local v3 = fn2()
            if v3 ~= v2 then if fn3() then v2 = v3 end end
        end
        
        if _G.G_InactivityHop and not getgenv().IsServerHopping then
            local timeIdle = tick() - _G.LastActivityTime
            if timeIdle >= _G.G_InactivityTime then
                print("[bananaXpvp] Inatividade detectada por " .. math.floor(timeIdle) .. "s. Pulando servidor...")
                ForceHopServer()
                _G.LastActivityTime = tick()
            end
        end
    end
end)

local function UpdateActivity()
    _G.LastActivityTime = tick()
end

-- ==========================================
-- LÓGICA AUTO BOUNTY (RESTANTE)
-- ==========================================
_G.G_ServerRegion = _G.G_ServerRegion or "Singapore"
_G.G_HopPlayerRange = _G.G_HopPlayerRange or "1-12"
_G.G_HopBountyRange = _G.G_HopBountyRange or "Maior que 3m"
_G.G_HopMinPlayers = _G.G_HopMinPlayers or 1
_G.G_HopMaxPlayers = _G.G_HopMaxPlayers or 12
_G.G_HopMinBounty = _G.G_HopMinBounty or 3000001
_G.G_HopMaxBounty = _G.G_HopMaxBounty or 999999999

local function fn5(arg)
	local match, v3, v4 = tostring(math.floor(tonumber(arg) or 0)):match("^([^%d]*%d)(%d*)(.-)$")
	return match .. v3:reverse():gsub("(%d%d%d)", "%1,"):reverse() .. v4
end

local function fn6(gHopPlayerRange)
	_G.G_HopPlayerRange = gHopPlayerRange
	local match, v3 = gHopPlayerRange:match("(%d+)%-(%d+)")
	if match and v3 then
		_G.G_HopMinPlayers = tonumber(match)
		_G.G_HopMaxPlayers = tonumber(v3)
		return
	end
	_G.G_HopMinPlayers = 1
	_G.G_HopMaxPlayers = 12
end

local function fn7(gHopBountyRange)
	_G.G_HopBountyRange = gHopBountyRange
	local match = gHopBountyRange:match("Maior que ([%d%.]+[Mm]?)") or gHopBountyRange:match("大于([%d%.]+[Mm]?)")
	if match then
		local match2, v3 = match:match("([%d%.]+)([Mm]?)")
		local n = tonumber(match2) or 0
		if v3 and v3:lower() == "m" then n *= 1000000 end
		_G.G_HopMinBounty = n + 1
		_G.G_HopMaxBounty = 999999999
		return
	end
	_G.G_HopMinBounty = 0
	_G.G_HopMaxBounty = 999999999
end

fn6(_G.G_HopPlayerRange)
fn7(_G.G_HopBountyRange)

local n = 5000

local function fn19(arg)
	if not arg then return false end
	local humanoid = arg:FindFirstChildOfClass("Humanoid")
	return humanoid and humanoid.Health and humanoid.Health > 0
end

local function fn20(arg)
	if not arg then return nil end
	local hrp = arg:FindFirstChild("HumanoidRootPart")
	if not hrp then return nil end
	local tbl5 = { arg:FindFirstChild("Head"), arg:FindFirstChild("UpperTorso"), arg:FindFirstChild("LowerTorso"), arg:FindFirstChild("Torso"), hrp }
	local tbl6 = {}
	for _, v3 in ipairs(tbl5) do if v3 and v3:IsA("BasePart") then table.insert(tbl6, v3) end end
	if #tbl6 > 0 then return tbl6[math.random(1, #tbl6)] end
	return hrp
end

local tbl5 = { consecutiveFailures = 0, maxConsecutiveFailures = 5 }

local function fn21()
	if tbl5.Remotes and tbl5.Net and tbl5.RegisterAttack and tbl5.RegisterHit and tbl5.Enemies then
		return tbl5.Remotes, tbl5.Net, tbl5.RegisterAttack, tbl5.RegisterHit, tbl5.Enemies
	end
	local remotes = ReplicatedStorage:FindFirstChild("Remotes")
	local modules = ReplicatedStorage:FindFirstChild("Modules")
	local net = modules and modules:FindFirstChild("Net")
	local reAttack = net and (net:FindFirstChild("RE/RegisterAttack") or net:FindFirstChild("RegisterAttack"))
	local reHit = net and (net:FindFirstChild("RE/RegisterHit") or net:FindFirstChild("RegisterHit"))
	local enemies = Workspace:FindFirstChild("Enemies") or Workspace:FindFirstChild("NPCs")
	if remotes and modules and net and reAttack and reHit and enemies then
		tbl5.Remotes, tbl5.Net, tbl5.RegisterAttack, tbl5.RegisterHit, tbl5.Enemies = remotes, net, reAttack, reHit, enemies
		return remotes, net, reAttack, reHit, enemies
	end
	return nil, nil, nil, nil, nil
end

local function fn22(arg, arg2)
	if not arg2 or not _G.G_AttackMobs then return nil end
	local pos = localPlayer.Character and localPlayer.Character.PrimaryPart and localPlayer.Character.PrimaryPart.Position
	if not pos then return nil end
	local v3 = nil
	for _, child in ipairs(arg2:GetChildren()) do
		if not (child == localPlayer.Character or not fn19(child)) then
			local hrp = child:FindFirstChild("HumanoidRootPart")
			if hrp and (hrp.Position - pos).Magnitude < 500 then
				local v4 = fn20(child)
				if v4 then table.insert(arg, { child, v4 }); v3 = v4 end
			end
		end
	end
	return v3
end

local function fn23(arg)
	if not _G.G_AttackPlayers then return nil end
	if not (localPlayer.Character and localPlayer.Character.PrimaryPart) then return nil end
	local v3 = nil
	for _, player in ipairs(Players:GetPlayers()) do
		if player ~= localPlayer then
			local char = player.Character
			if fn19(char) then
				local v4 = fn20(char)
				if v4 and localPlayer:DistanceFromCharacter(v4.Position) < 500 then table.insert(arg, { char, v4 }); v3 = v4 end
			end
		end
	end
	return v3
end

local function fn24(arg, arg2)
	local v3, v4, v5, v6 = fn21()
	if not (arg and arg2 and #arg2 > 0 and v5 and v6) then return end
	pcall(function() v5:FireServer(0.3); v6:FireServer(arg, arg2) end)
end

local function fn25()
	local v3, v4, v5, v6, v7 = fn21()
	if not v7 then return end
	local tbl6 = {}
	local v8 = fn22(tbl6, v7)
	local v9 = fn23(tbl6)
	if #tbl6 > 0 then fn24(v8 or v9, tbl6) end
end

local tbl6 = { Found = nil, Id = nil }
task.spawn(function()
	local tbl7 = { ReplicatedStorage:FindFirstChild("Util"), ReplicatedStorage:FindFirstChild("Common"), ReplicatedStorage:FindFirstChild("Remotes"), ReplicatedStorage:FindFirstChild("Assets"), ReplicatedStorage:FindFirstChild("FX") }
	for _, v3 in ipairs(tbl7) do
		if v3 then
			for _, child in ipairs(v3:GetChildren()) do
				if child:IsA("RemoteEvent") and child:GetAttribute("Id") then tbl6.Id = child:GetAttribute("Id"); tbl6.Found = child end
			end
		end
	end
end)

local function fn27()
	local char = localPlayer.Character
	local hrp = char and char:FindFirstChild("HumanoidRootPart")
	if not hrp then return {} end
	local tbl7 = {}
	for _, v3 in ipairs({ Workspace:FindFirstChild("Enemies"), Workspace:FindFirstChild("Characters") }) do
		if v3 then
			for _, child in ipairs(v3:GetChildren()) do
				if child ~= char then
					local h = child:FindFirstChild("HumanoidRootPart")
					local hum = child:FindFirstChild("Humanoid")
					if h and hum and hum.Health > 0 and (h.Position - hrp.Position).Magnitude <= n then
						table.insert(tbl7, { Model = child, Root = h, Head = child:FindFirstChild("Head") or h })
					end
				end
			end
		end
	end
	return tbl7
end

local function fn28()
	local char = localPlayer.Character
	if not char or not (char:FindFirstChildOfClass("Tool") or char:FindFirstChild("EquippedWeapon")) then return end
	local v3 = fn27()
	if #v3 == 0 then return end
	local modules = ReplicatedStorage:FindFirstChild("Modules")
	local net = modules and modules:FindFirstChild("Net")
	local reAttack = net and (net:FindFirstChild("RE/RegisterAttack") or net:FindFirstChild("RegisterAttack"))
	local reHit = net and (net:FindFirstChild("RE/RegisterHit") or net:FindFirstChild("RegisterHit"))
	if not reAttack or not reHit then return end
	local tbl7 = {}
	for _, v5 in ipairs(v3) do table.insert(tbl7, { v5.Model, v5.Root }) end
	pcall(function() reAttack:FireServer(); reHit:FireServer(v3[1].Head, tbl7, {}, tostring(localPlayer.UserId):sub(2, 4) .. tostring(math.random(10000, 99999))) end)
end

task.spawn(function()
	while true do
		if not _G.G_FastAttack then task.wait(0.3) else pcall(fn28); task.wait(0.05) end
	end
end)

local fn30, fn31, fn32, fn33, fn34
local tbl7 = {}
local function fn35(arg, arg2)
	local v3 = tbl7[arg]
	if v3 then pcall(function() v3:Set(arg2) end) end
end

local function fn36(huntAuto)
	_G.Hunt_Auto, _G.Hunt_AutoSoru, _G.Hunt_AutoSkill, _G.Hunt_AutoAttack, _G.Hunt_AutoEquip, _G.Hunt_SkillAim, _G.Hunt_SoruAim, _G.Hunt_AutoHop = huntAuto, huntAuto, huntAuto, huntAuto, huntAuto, huntAuto, huntAuto, huntAuto
	fn35("Hunt_Auto", huntAuto); fn35("Hunt_AutoSoru", huntAuto); fn35("Hunt_AutoSkill", huntAuto); fn35("Hunt_AutoAttack", huntAuto); fn35("Hunt_AutoEquip", huntAuto); fn35("Hunt_SkillAim", huntAuto); fn35("Hunt_SoruAim", huntAuto); fn35("Hunt_AutoHop", huntAuto)
end

local function fn39(arg)
	local attribute = nil
	if pcall(function() attribute = arg:GetAttribute("Level") end) and attribute then return tonumber(attribute) end
	if pcall(function() attribute = arg.Data.Level.Value end) and attribute then return tonumber(attribute) end
	if pcall(function() attribute = arg.leaderstats.Level.Value end) and attribute then return tonumber(attribute) end
	return nil
end

local function fn40(arg)
	local char = arg.Character
	if not char then return false end
	local hum = char:FindFirstChildOfClass("Humanoid")
	return hum and hum.Health > 0
end

local v4, name, tbl9, n2, tbl10, tbl11, tbl12 = nil, nil, {}, 30, {}, {}, {}

local function fn41(arg)
	local char = arg.Character
	local tbl13 = { arg, char, char and char:FindFirstChild("HumanoidRootPart") }
	for _, v5 in ipairs(tbl13) do
		if v5 then
			local attr = v5:GetAttribute("SafeZone") or v5:GetAttribute("InSafeZone")
			if attr ~= nil then return attr == true end
		end
	end
	return nil
end

local function fn42(arg)
	if fn41(arg) == true then return true end
	local v5 = tbl12[arg.Name]
	if v5 and tick() < v5 then return true end
	return false
end

local tbl13 = {}
local function fn43(arg)
	if type(arg) ~= "string" or arg == "" then return false end
	local v5 = string.lower(arg)
	return string.find(v5, "arena", 1, true) or string.find(v5, "colosseum", 1, true)
end

local function fn44(arg)
	local char = arg.Character
	local tbl15 = { arg, char, char and char:FindFirstChild("HumanoidRootPart") }
	for _, v5 in ipairs(tbl15) do
		if v5 then
			local ok, result = pcall(function() return v5:GetAttributes() end)
			if ok and type(result) == "table" then
				for k, v6 in pairs(result) do
					if fn43(k) and v6 == true then return true end
					if type(v6) == "string" and fn43(v6) then return true end
				end
			end
		end
	end
	return nil
end

local tbl15 = {}
local function fn45(arg)
	local char = arg.Character
	local hrp = char and char:FindFirstChild("HumanoidRootPart")
	if not hrp then return nil end
	local v5 = tbl15[arg.Name]
	if v5 and tick() < v5.t then return v5.v end
	local ok, result = pcall(function() return Workspace:GetPartBoundsInRadius(hrp.Position, 80) end)
	local flag5 = nil
	if ok and result then
		for _, v7 in ipairs(result) do
			if fn43(v7.Name) then flag5 = true; break end
		end
	end
	tbl15[arg.Name] = { v = flag5, t = tick() + 3 }
	return flag5
end

local function fn46(arg)
	if fn44(arg) == true then return true end
	local v5 = tbl13[arg.Name]
	if v5 and tick() < v5 then return true end
	return fn45(arg) == true
end

local function fn47(arg)
	for _, v5 in ipairs({ arg, arg.Character }) do
		if v5 then
			local ok, result = pcall(function() return v5:GetAttribute("PvpDisabled") end)
			if ok and result ~= nil then return result end
		end
	end
	return nil
end

local function fn48(arg)
	local v5 = fn47(arg)
	return v5 == nil or v5 == false
end

local function fn49(arg)
	if _G.Hunt_NoPvp and not fn48(arg) then return false end
	if _G.Hunt_NoSafe and fn42(arg) then return false end
	if _G.Hunt_NoArena and fn46(arg) then return false end
	if _G.Hunt_NoKilledEnough and (tbl11[arg.Name] or 0) >= (_G.Hunt_KillHopCount or 3) then return false end
	if _G.Hunt_NoMarineAlly and localPlayer.Team and arg.Team and localPlayer.Team.Name == "Marines" and arg.Team.Name == "Marines" then return false end
	if not _G.Hunt_UseLevel then return true end
	local v5 = fn39(arg)
	return v5 and v5 >= _G.Hunt_MinLevel or (not v5 and _G.Hunt_AllowUnknown)
end

local function fn50(arg)
	local char = arg.Character
	return char and char:FindFirstChild("HumanoidRootPart"), char
end

local function fn51(arg)
	local hrp = localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart")
	local v5 = fn50(arg)
	return (hrp and v5) and (v5.Position - hrp.Position).Magnitude or nil
end

local function fn52(arg)
	local v5 = fn51(arg)
	return v5 and v5 > (_G.Hunt_MaxDist or 10000)
end

local now = nil
local function fn54()
	if not now then now = tick() end
	return tick() - now < (_G.Hunt_TargetSwitchDelay or 3)
end

local function fn53(arg)
	if not arg or not arg.Parent then return false, "gone" end
	local char = arg.Character
	char = char and char:FindFirstChildOfClass("Humanoid")
	if not char or char.Health <= 0 then return false, "dead" end
	if fn52(arg) then return false, "far" end
	if not fn49(arg) then return false, "filter" end
	return true
end

local function fn55()
	if _G.Hunt_Target and _G.Hunt_Target ~= "" then
		local v5 = Players:FindFirstChild(_G.Hunt_Target)
		if v5 and v5 ~= localPlayer and fn53(v5) then return v5, 1 end
	end
	if _G.Hunt_AutoSelect and v4 and v4 ~= localPlayer then
		local v5 = fn53(v4)
		if v5 then return v4, 1 end
		if fn54() then return v4, 1 end
		now = nil
	end
	if not _G.Hunt_AutoSelect then return nil, 0 end
	local hrp = localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart")
	if not hrp then return nil, 0 end
	local n5, huge, v5 = 0, math.huge, nil
	for _, player in ipairs(Players:GetPlayers()) do
		if player ~= localPlayer and fn40(player) and fn49(player) then
			local v6 = fn50(player)
			if v6 then
				n5 += 1
				local mag = (v6.Position - hrp.Position).Magnitude
				if mag <= _G.Hunt_MaxDist and mag < huge then huge = mag; v5 = player end
			end
		end
	end
	return v5, n5
end

task.spawn(function()
	if (_G.Hunt_StartWait or 0) > 0 then task.wait(_G.Hunt_StartWait) end
	while task.wait(1) do
		if _G.Hunt_Auto then
			local ok, result, result2 = pcall(fn55)
			v4 = result
			name = result and result.Name or nil
		end
	end
end)

task.spawn(function()
	while task.wait(5) do _G.Hunt_NoPvp = true end
end)

fn30 = function()
	local hrp = localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart")
	if not hrp then return end
	local pos = _G.Hunt_SoruAim and v4 and fn50(v4) and (v4.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)).Position or hrp.Position + hrp.CFrame.LookVector * 40
	local cFrame = hrp.CFrame
	local n8 = cFrame - cFrame.Position + pos + Vector3.new(0, hrp.Size.Y * 1.5, 0)
	pcall(function()
		local commE = ReplicatedStorage:FindFirstChild("Remotes") and ReplicatedStorage.Remotes:FindFirstChild("CommE")
		if commE then commE:FireServer("Soru", cFrame, n8, workspace:GetServerTimeNow(), math.random(1, 999999999)) end
	end)
end

local tbl16 = { ["Fruta"] = { "Blox Fruit" }, ["Soco"] = { "Melee", "Fighting" }, ["Espada"] = { "Sword" }, ["Arma"] = { "Gun" } }

fn33 = function()
	local bp = localPlayer:FindFirstChild("Backpack")
	if not bp then return {} end
	local t = {}
	for _, c in ipairs(bp:GetChildren()) do if c:IsA("Tool") then table.insert(t, c) end end
	return t
end

fn32 = function()
	if not _G.Hunt_AutoEquip then return end
	local char = localPlayer.Character
	local hum = char and char:FindFirstChildOfClass("Humanoid")
	if not hum then return end
	local allowed = {}
	for _, v6 in ipairs(tbl2) do if _G.Hunt_Weapons[v6] then for _, v7 in ipairs(tbl16[v6]) do table.insert(allowed, v7) end end end
	if #allowed == 0 then return end
	local current = char:FindFirstChildOfClass("Tool")
	if current then for _, v in ipairs(allowed) do if current.ToolTip == v then return end end end
	local bp = localPlayer:FindFirstChild("Backpack")
	if bp then for _, c in ipairs(bp:GetChildren()) do
		if c:IsA("Tool") then for _, v in ipairs(allowed) do if c.ToolTip == v then pcall(function() hum:EquipTool(c) end); return end end end
	end end
end

task.spawn(function() while task.wait(0.5) do fn32() end end)

local tbl17 = { ["Pirata"] = "Pirates", ["Marinha"] = "Marines" }
fn34 = function()
	local v6 = tbl17[_G.Hunt_Team]
	if not v6 then return end
	pcall(function()
		local rem = ReplicatedStorage:FindFirstChild("Remotes") and ReplicatedStorage.Remotes:FindFirstChild("CommF_")
		if rem then rem:InvokeServer("SetTeam", v6) end
	end)
end

task.spawn(function() while task.wait(2) do if _G.Hunt_AutoHaki and not localPlayer.Character:FindFirstChild("HasBuso") then pcall(function() ReplicatedStorage.Remotes.CommF_:InvokeServer("Buso") end) end end end)
task.spawn(function() while task.wait(0.5) do if _G.Hunt_AutoKen then pcall(function() ReplicatedStorage.Remotes.CommE:FireServer("Ken", true) end) end end end)
task.spawn(function() while task.wait(1) do if _G.Hunt_AutoV3 then pcall(function() ReplicatedStorage.Remotes.CommE:FireServer("ActivateAbility") end) end end end)
task.spawn(function() while task.wait(1) do if _G.Hunt_AutoV4 then pcall(function() local aw = localPlayer.Backpack:FindFirstChild("Awakening"); if aw then aw.RemoteFunction:InvokeServer(true) end end) end end end)

fn31 = function(arg)
	if not _G.Hunt_AutoSkill or _G.Hunt_Fleeing or _G.Hunt_HopSkyTimer or _G.Hunt_HopSkyEmpty then return end
	local char = localPlayer.Character
	local tool = char and char:FindFirstChildOfClass("Tool")
	if not tool then return end
	local typeMap = { ["Blox Fruit"] = "Fruta", Melee = "Soco", Fighting = "Soco", Sword = "Espada", Gun = "Arma" }
	local tType = typeMap[tool.ToolTip]
	if not tType or not tbl3[tType] then return end
	task.spawn(function()
		for _, key in ipairs(tbl3[tType]) do
			if _G.Hunt_TypeSkills[tType][key] then
				pcall(function() VirtualInputManager:SendKeyEvent(true, Enum.KeyCode[key], false, game); task.wait(0.03); VirtualInputManager:SendKeyEvent(false, Enum.KeyCode[key], false, game) end)
				task.wait(0.05)
			end
		end
	end)
end

task.spawn(function() while task.wait(1) do if _G.Hunt_Auto and _G.Hunt_AutoSoru and not _G.Hunt_Fleeing then pcall(fn30) end end end)

local n11 = 0
local function fn66(arg)
	if not _G.Hunt_AutoAttack then return end
	if tick() - n11 < (_G.Hunt_AttackSpeed or 0.12) then return end
	n11 = tick()
	UpdateActivity()
	
	local char = localPlayer.Character
	local hrp = char and char:FindFirstChild("HumanoidRootPart")
	local targetHrp = arg.Character and arg.Character:FindFirstChild("HumanoidRootPart")
	if not hrp or not targetHrp then return end
	local net = ReplicatedStorage:FindFirstChild("Modules") and ReplicatedStorage.Modules:FindFirstChild("Net")
	if not net then return end
	local hit = net:FindFirstChild("RE/RegisterHit")
	if not hit then return end
	local tbl = {}
	for _, c in ipairs(arg.Character:GetChildren()) do if c:IsA("BasePart") then table.insert(tbl, {arg.Character, c}) end end
	pcall(function() hit:FireServer(targetHrp, tbl) end)
end

local flag6, n12, n13, y = false, 0, 0, nil

local function ClickM1V4(mob)
    local char = localPlayer.Character
    if not char or not mob or not mob:FindFirstChild("HumanoidRootPart") then return end
    if (char.HumanoidRootPart.Position - mob.HumanoidRootPart.Position).Magnitude > 70 then return end
    
    local fruitName = NameWeapon("Blox Fruit")
    if fruitName and char:FindFirstChild(fruitName) then
        local tool = char:FindFirstChild(fruitName)
        local leftClick = tool:FindFirstChild("LeftClickRemote")
        if leftClick then
            leftClick:FireServer((mob.HumanoidRootPart.Position - char.HumanoidRootPart.Position).Unit, 1)
            return
        end
    end
    local hits = AttackAOE(80, true)
    if hits then
        local v3, v4, v5, v6 = fn21()
        if v5 and v6 then pcall(function() v5:FireServer(0); v6:FireServer(table.remove(hits, 1)[2], hits) end) end
    end
end

local function fn67(arg)
	local char = localPlayer.Character
	local hrp = char and char:FindFirstChild("HumanoidRootPart")
	local hum = char and char:FindFirstChildOfClass("Humanoid")
	if not hrp or not hum then return end
	
    -- SE O V4 ESTIVER ATIVO, ELE USA O SISTEMA DE PUXAR COM LIMITE
    if _G.V4_Enabled then
        if _G.V4_AutoActivate then
            local aw = char:FindFirstChild("Awakening") or localPlayer.Backpack:FindFirstChild("Awakening")
            local energy = char:FindFirstChild("RaceEnergy")
            if aw and energy and energy.Value >= 1 and not char:FindFirstChild("RaceTransformed") then
                pcall(function() aw.RemoteFunction:InvokeServer(true) end)
            end
        end

        if tick() - (getgenv().LastV4Attack or 0) > 0.1 then
            getgenv().LastV4Attack = tick()
            
            local validMobs = {}
            local enemies = workspace:FindFirstChild("Enemies")
            
            -- Varre todos os mobs num raio de 400 studs e coloca na lista
            if enemies then
                for _, mob in ipairs(enemies:GetChildren()) do
                    local eHrp = mob:FindFirstChild("HumanoidRootPart")
                    local eHum = mob:FindFirstChild("Humanoid")
                    
                    if eHrp and eHum and eHum.Health > 0 and not Players:GetPlayerFromCharacter(mob) then
                        local d = (eHrp.Position - hrp.Position).Magnitude
                        if d <= 400 then
                            table.insert(validMobs, {mob = mob, eHrp = eHrp, dist = d})
                        end
                    end
                end
            end
            
            -- Ordena do mais perto pro mais longe
            table.sort(validMobs, function(a, b) return a.dist < b.dist end)
            
            local limit = _G.V4_PullCount or 5
            local pulledCount = 0
            
            -- Puxa apenas a quantidade definida no Slider
            for i, data in ipairs(validMobs) do
                if pulledCount >= limit then break end
                
                data.eHrp.CFrame = hrp.CFrame * CFrame.new(math.random(-3,3), 5, math.random(-3,3))
                data.eHrp.Velocity = Vector3.zero
                ClickM1V4(data.mob)
                pulledCount = pulledCount + 1
            end

            -- Voa até o mais próximo se estiver longe
            local closest = validMobs[1]
            if closest and closest.dist > 50 then
                local speed = _G.V4_TweenSpeed or 350
                hrp.CFrame = CFrame.new(hrp.Position + (closest.eHrp.Position - hrp.Position).Unit * math.min(speed * arg, closest.dist)) * (hrp.CFrame - hrp.Position)
                hrp.Velocity = Vector3.zero
            end
            
            if closest then UpdateActivity() end
        end
        return
    end
	
	if _G.Hunt_AutoFlee and hum.Health > 0 then
		local hp = hum.Health / hum.MaxHealth * 100
		if not flag6 and hp <= _G.Hunt_FleeHP then flag6, _G.Hunt_Fleeing, n12 = true, true, hrp.Position.Y
		elseif flag6 and hp >= _G.Hunt_FleeRecover then flag6, _G.Hunt_Fleeing = false, false end
		if flag6 then
			if hrp.Position.Y < n12 + _G.Hunt_FleeHeight then
				hrp.CFrame = CFrame.new(hrp.Position + Vector3.new(0, _G.Hunt_FlySpeed * arg, 0)) * (hrp.CFrame - hrp.Position)
				hrp.Velocity = Vector3.zero
			end
			return
		end
	end
	if not _G.Hunt_Auto or not v4 then return end
	local v6 = fn50(v4)
	if not v6 then return end
	
	local offset = Vector3.new(0, _G.Hunt_StopDist, 0)
	local mode = _G.Hunt_PositionMode or "Frente"
	
	if mode == "Frente" then offset = Vector3.new(0, 0, _G.Hunt_StopDist)
	elseif mode == "Atrás" then offset = Vector3.new(0, 0, -_G.Hunt_StopDist)
	elseif mode == "Cima" then offset = Vector3.new(0, _G.Hunt_StopDist, 0)
	elseif mode == "Baixo" then offset = Vector3.new(0, -_G.Hunt_StopDist, 0)
	elseif mode == "Lado" then offset = Vector3.new(_G.Hunt_StopDist, 0, 0)
	elseif mode == "Dentro" then offset = Vector3.new(0, 0, 0)
	end
	
	local targetPos = (v6.CFrame * CFrame.new(offset)).Position
	local mag = (targetPos - hrp.Position).Magnitude
	
	if mag > 1 then
		hrp.CFrame = CFrame.new(hrp.Position + (targetPos - hrp.Position).Unit * math.min(_G.Hunt_FlySpeed * arg, mag)) * (hrp.CFrame - hrp.Position)
		hrp.Velocity = Vector3.zero
	end
	
	if mag <= _G.Hunt_StopDist + 20 then fn31(); fn66(v4); UpdateActivity() end
	if mag <= _G.Hunt_StopDist + 6 then fn30(); UpdateActivity() end
end

RunService.Heartbeat:Connect(function(dt) pcall(fn67, dt) end)

-- ==========================================
-- PARTE 2: UI
-- ==========================================
Main = A.CreateMain({ Title = "bananaXpvp", Desc = " - Hub Completo" })

local PageV4 = Main.CreatePage({ Page_Name = "V4 Farm", Page_Title = "Auto V4 / Farm" })
local PageBounty = Main.CreatePage({ Page_Name = "Auto Bounty", Page_Title = "Auto Bounty" })
local PageCombat = Main.CreatePage({ Page_Name = "Combate", Page_Title = "Combate" })
local PageAura = Main.CreatePage({ Page_Name = "Kill Aura", Page_Title = "Kill Aura" })
local PageServer = Main.CreatePage({ Page_Name = "Servidor", Page_Title = "Servidor" })
local PageConfig = Main.CreatePage({ Page_Name = "Configurações", Page_Title = "Configurações" })
local PageClick = Main.CreatePage({ Page_Name = "Auto Click", Page_Title = "Auto Click" })

-- PREENCHENDO A ABA V4
local SecV4 = PageV4.CreateSection("Configurações V4 Farm")
SecV4.CreateToggle({ Title = "Ativar Auto V4 (Puxar NPCs)", Default = _G.V4_Enabled }, function(v) _G.V4_Enabled = v end)
SecV4.CreateToggle({ Title = "Ativar Transformação V4", Default = _G.V4_AutoActivate }, function(v) _G.V4_AutoActivate = v end)
SecV4.CreateSlider({ Title = "Velocidade do Voo (Tween)", Min = 50, Max = 99999, Default = _G.V4_TweenSpeed, Precise = true }, function(v) _G.V4_TweenSpeed = v end)
SecV4.CreateSlider({ Title = "Máximo de NPCs Puxados", Min = 1, Max = 30, Default = _G.V4_PullCount, Precise = true }, function(v) _G.V4_PullCount = v end)

-- ABA: AUTO BOUNTY
local SecBounty = PageBounty.CreateSection("Caça Recompensa")
SecBounty.CreateToggle({ Title = "Ativar Tudo (Auto Run)", Default = _G.Hunt_AutoRun }, function(v) _G.Hunt_AutoRun = v; fn36(v) end)
SecBounty.CreateToggle({ Title = "Auto Bounty", Default = _G.Hunt_Auto }, function(v) _G.Hunt_Auto = v end)
SecBounty.CreateSlider({ Title = "Espera Inicial (s)", Min = 0, Max = 60, Default = _G.Hunt_StartWait }, function(v) _G.Hunt_StartWait = v end)
SecBounty.CreateToggle({ Title = "Auto Equipar Armas (Se macro off)", Default = _G.Hunt_AutoEquip }, function(v) _G.Hunt_AutoEquip = v end)
for _, w in ipairs(tbl2) do SecBounty.CreateToggle({ Title = "Usar " .. w, Default = _G.Hunt_Weapons[w] }, function(v) _G.Hunt_Weapons[w] = v end) end

local SecSpeed = PageBounty.CreateSection("Configurações de Velocidade e Combate")
SecSpeed.CreateSlider({ Title = "Velocidade de Voo", Min = 50, Max = 99999, Default = _G.Hunt_FlySpeed }, function(v) _G.Hunt_FlySpeed = v end)
SecSpeed.CreateSlider({ Title = "Distância do Alvo (m)", Min = 2, Max = 40, Default = _G.Hunt_StopDist }, function(v) _G.Hunt_StopDist = v end)
SecSpeed.CreateSlider({ Title = "Tempo p/ Trocar Alvo (s)", Min = 0, Max = 10, Default = _G.Hunt_TargetSwitchDelay }, function(v) _G.Hunt_TargetSwitchDelay = v end)
SecSpeed.CreateSlider({ Title = "Velocidade do Ataque (s)", Min = 0.05, Max = 1, Default = _G.Hunt_AttackSpeed, Precise = true }, function(v) _G.Hunt_AttackSpeed = v end)
SecSpeed.CreateDropdown({ Title = "Ficar em Relação ao Alvo:", List = { "Frente", "Atrás", "Lado", "Cima", "Baixo", "Dentro" }, Default = _G.Hunt_PositionMode }, function(v) _G.Hunt_PositionMode = v end)

local SecFilter = PageBounty.CreateSection("Filtro de Alvos")
SecFilter.CreateToggle({ Title = "Filtro de Nível", Default = _G.Hunt_UseLevel }, function(v) _G.Hunt_UseLevel = v end)
SecFilter.CreateSlider({ Title = "Nível Mínimo", Min = 1, Max = 2800, Default = _G.Hunt_MinLevel }, function(v) _G.Hunt_MinLevel = v end)
SecFilter.CreateToggle({ Title = "Atacar Nível Desconhecido", Default = _G.Hunt_AllowUnknown }, function(v) _G.Hunt_AllowUnknown = v end)
SecFilter.CreateToggle({ Title = "Ignorar Zona Segura", Default = _G.Hunt_NoSafe }, function(v) _G.Hunt_NoSafe = v end)
SecFilter.CreateToggle({ Title = "Ignorar Arena", Default = _G.Hunt_NoArena }, function(v) _G.Hunt_NoArena = v end)
SecFilter.CreateToggle({ Title = "Ignorar Aliados (Marinha)", Default = _G.Hunt_NoMarineAlly }, function(v) _G.Hunt_NoMarineAlly = v end)
SecFilter.CreateSlider({ Title = "Distância de Troca (Max)", Min = 500, Max = 50000, Default = _G.Hunt_MaxDist }, function(v) _G.Hunt_MaxDist = v end)

-- ABA: COMBATE (COM MACRO)
local SecMacro = PageCombat.CreateSection("Macro de Armas (Combo)")
SecMacro.CreateToggle({ Title = "Ativar Macro (Troca Automática)", Default = _G.Macro_Enabled }, function(v) _G.Macro_Enabled = v end)
SecMacro.CreateSlider({ Title = "Tempo para Trocar (s)", Min = 0.5, Max = 10, Default = _G.Macro_Delay, Precise = true }, function(v) _G.Macro_Delay = v end)
SecMacro.CreateDropdown({ Title = "Armas no Macro (Ordem)", List = tbl2, Selected = true, Default = _G.Macro_Weapons }, function(v) 
    if type(v) == "table" then
        local newList = {}
        for _, wType in ipairs(tbl2) do if v[wType] then table.insert(newList, wType) end end
        _G.Macro_Weapons = newList
    end
end)

local SecSkills = PageCombat.CreateSection("Habilidades (Teclas por Arma)")
SecSkills.CreateToggle({ Title = "Auto Usar Habilidades (Apertar Teclas)", Default = _G.Hunt_AutoSkill }, function(v) _G.Hunt_AutoSkill = v end)
SecSkills.CreateSlider({ Title = "Intervalo entre Combo (s)", Min = 0.1, Max = 5, Default = _G.Hunt_SkillInterval, Precise = true }, function(v) _G.Hunt_SkillInterval = v end)
for _, t in ipairs(tbl2) do for _, key in ipairs(tbl3[t]) do SecSkills.CreateToggle({ Title = t .. " - " .. key, Default = _G.Hunt_TypeSkills[t][key] }, function(v) _G.Hunt_TypeSkills[t][key] = v end) end end

local SecCombat = PageCombat.CreateSection("Movimento e Estilo de Luta")
SecCombat.CreateToggle({ Title = "Auto Soru (Passo Relâmpago)", Default = _G.Hunt_AutoSoru }, function(v) _G.Hunt_AutoSoru = v end)
SecCombat.CreateSlider({ Title = "Intervalo do Soru (s)", Min = 1, Max = 60, Default = _G.Hunt_SoruInterval }, function(v) _G.Hunt_SoruInterval = v end)
SecCombat.CreateButton({ Title = "Soru Imediato" }, function() fn30() end)
SecCombat.CreateToggle({ Title = "Auto Buso (Armamento)", Default = _G.Hunt_AutoHaki }, function(v) _G.Hunt_AutoHaki = v end)
SecCombat.CreateToggle({ Title = "Auto Ken (Observação)", Default = _G.Hunt_AutoKen }, function(v) _G.Hunt_AutoKen = v end)
SecCombat.CreateToggle({ Title = "Auto V3", Default = _G.Hunt_AutoV3 }, function(v) _G.Hunt_AutoV3 = v end)
SecCombat.CreateToggle({ Title = "Auto V4", Default = _G.Hunt_AutoV4 }, function(v) _G.Hunt_AutoV4 = v end)

local SecFlee = PageCombat.CreateSection("Fuga (Vida Baixa)")
SecFlee.CreateToggle({ Title = "Voar ao ficar com Vida Baixa", Default = _G.Hunt_AutoFlee }, function(v) _G.Hunt_AutoFlee = v end)
SecFlee.CreateSlider({ Title = "Ativar com HP (%)", Min = 5, Max = 90, Default = _G.Hunt_FleeHP }, function(v) _G.Hunt_FleeHP = v end)
SecFlee.CreateSlider({ Title = "Altura de Fuga (m)", Min = 200, Max = 10000, Default = _G.Hunt_FleeHeight }, function(v) _G.Hunt_FleeHeight = v end)

-- ABA: KILL AURA
local SecAura = PageAura.CreateSection("Kill Aura / Fast Attack")
SecAura.CreateToggle({ Title = "Ativar Fast Attack", Default = _G.G_FastAttack }, function(v) _G.G_FastAttack = v end)
SecAura.CreateDropdown({ Title = "Modo do Fast Attack", List = { "Modo 1", "Modo 2 (Usar se o 1 falhar)" }, Default = _G.G_FastAttackMode }, function(v) _G.G_FastAttackMode = v end)
SecAura.CreateToggle({ Title = "Atacar Monstros (Mobs)", Default = _G.G_AttackMobs }, function(v) _G.G_AttackMobs = v end)
SecAura.CreateToggle({ Title = "Atacar Jogadores", Default = _G.G_AttackPlayers }, function(v) _G.G_AttackPlayers = v end)

-- ABA: SERVIDOR
local SecServer = PageServer.CreateSection("Auto Pular Servidor (Hop)")
SecServer.CreateToggle({ Title = "Auto Pular Servidor (Tempo)", Default = _G.Hunt_AutoHop }, function(v) _G.Hunt_AutoHop = v end)
SecServer.CreateSlider({ Title = "Intervalo do Hop (s)", Min = 60, Max = 1200, Default = _G.Hunt_HopInterval }, function(v) _G.Hunt_HopInterval = v end)
SecServer.CreateToggle({ Title = "Pular se Estiver Vazio", Default = _G.Hunt_HopIfEmpty }, function(v) _G.Hunt_HopIfEmpty = v end)
SecServer.CreateButton({ Title = "Pular Servidor Agora" }, function() ForceHopServer() end)
SecServer.CreateDropdown({ Title = "Região do Servidor", List = { "Singapore", "Brazil", "California", "Texas", "Florida", "Oregon", "Germany", "Hong Kong" }, Default = _G.G_ServerRegion }, function(v) _G.G_ServerRegion = v end)

local SecKillHop = PageServer.CreateSection("Pular Após Matar")
SecKillHop.CreateToggle({ Title = "Pular após Matar N vezes", Default = _G.Hunt_KillHop }, function(v) _G.Hunt_KillHop = v end)
SecKillHop.CreateSlider({ Title = "Qtd. de Mortes (N)", Min = 1, Max = 10, Default = _G.Hunt_KillHopCount }, function(v) _G.Hunt_KillHopCount = v end)
SecKillHop.CreateToggle({ Title = "Ignorar Jogadores já mortos N vezes", Default = _G.Hunt_NoKilledEnough }, function(v) _G.Hunt_NoKilledEnough = v end)

local SecTeam = PageServer.CreateSection("Time")
SecTeam.CreateDropdown({ Title = "Auto Escolher Time", List = { "Não trocar", "Pirata", "Marinha" }, Default = _G.Hunt_Team }, function(v) _G.Hunt_Team = v; if v ~= "Não trocar" then fn34() end end)

-- ABA: CONFIGURAÇÕES
local SecConfig = PageConfig.CreateSection("Configurações do Jogo")
SecConfig.CreateToggle({ Title = "Auto Salvar Configuração", Default = _G.Hunt_AutoSave }, function(v) _G.Hunt_AutoSave = v; if v then fn3() end end)
SecConfig.CreateButton({ Title = "Salvar Configuração Agora" }, function() fn3() end)
SecConfig.CreateToggle({ Title = "Noclip (Atravessar Paredes)", Default = _G.G_Noclip }, function(v) _G.G_Noclip = v end)
SecConfig.CreateToggle({ Title = "Remover Lava (Visual)", Default = _G.G_RemoveLava }, function(v) _G.G_RemoveLava = v end)
SecConfig.CreateToggle({ Title = "Andar na Água", Default = _G.G_WalkOnWater }, function(v) _G.G_WalkOnWater = v end)
SecConfig.CreateToggle({ Title = "Remover Efeitos (FPS Boost)", Default = _G.G_LowGraphics }, function(v) _G.G_LowGraphics = v end)

local SecInactive = PageConfig.CreateSection("Pular por Inatividade")
SecInactive.CreateToggle({ Title = "Pular se Inativo", Default = _G.G_InactivityHop }, function(v) _G.G_InactivityHop = v end)
SecInactive.CreateSlider({ Title = "Tempo de Inatividade (s)", Min = 10, Max = 120, Default = _G.G_InactivityTime }, function(v) _G.G_InactivityTime = v end)

-- ABA: AUTO CLICK
local ModulesFolder  = ReplicatedStorage:WaitForChild("Modules")
local NetModule      = require(ModulesFolder:WaitForChild("Net"))
local RegisterAttack = ModulesFolder.Net:WaitForChild("RE/RegisterAttack")
local RegisterHit    = NetModule:RemoteEvent("RegisterHit", true)
local CombatUtil     = require(ModulesFolder:WaitForChild("CombatUtil"))

local v_u_51 = 1
local validParts = { RightUpperArm=true, RightLowerArm=true, RightHand=true, RightUpperLeg=true, RightLowerLeg=true, RightFoot=true, LeftUpperArm=true, LeftLowerArm=true, LeftHand=true, LeftUpperLeg=true, LeftLowerLeg=true, LeftFoot=true, UpperTorso=true, LowerTorso=true, Head=true }

local function AttackAOE(radius, includePlayers)
    local hits, seen = {}, {}
    local char = localPlayer.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return nil end
    local origins = { char.HumanoidRootPart }

    local list = {}
    for _, e in pairs(workspace:WaitForChild("Enemies"):GetChildren()) do table.insert(list, e) end
    if includePlayers then for _, c in pairs(workspace:WaitForChild("Characters"):GetChildren()) do table.insert(list, c) end end

    for _, target in pairs(list) do
        if target:IsDescendantOf(workspace) and target ~= char and target:FindFirstChild("HumanoidRootPart") then
            local hrp = target.HumanoidRootPart
            local r = Players:GetPlayerFromCharacter(target) and radius / 1.5 or radius
            local positions = { hrp.Position }
            if hrp.Size.Y > 5 then table.insert(positions, (hrp.CFrame * CFrame.new(0, -hrp.Size.Y * 1.5 + 3, 0)).Position) end
            for _, pos in pairs(positions) do
                if (pos - origins[1].Position).Magnitude < 10 + r + hrp.Size.X / 2 then
                    for _, p in pairs(target:GetChildren()) do
                        if p:IsA("BasePart") and (p.Position - origins[1].Position).Magnitude <= r + hrp.Size.X / 2 then table.insert(hits, p) end
                    end
                    break
                end
            end
        end
    end
    local validHits = {}
    for _, part in pairs(hits) do
        local rig = CombatUtil:GetRigOfHitPart(part)
        if rig and not seen[rig] and validParts[part.Name] and CombatUtil:IsVulnerable(rig) then
            table.insert(validHits, { rig, part })
            seen[rig] = true
        end
    end
    return #validHits > 0 and validHits or nil
end

local function NameWeapon(tooltip, returnTool)
    local function scan(container)
        if not container then return nil end
        for _, tool in pairs(container:GetChildren()) do
            if tool:IsA("Tool") and tool.ToolTip == tooltip then return returnTool and tool or tool.Name end
        end
    end
    return scan(localPlayer.Backpack) or scan(localPlayer.Character)
end

local function UseFruitM1(target, useBoat)
    local char = localPlayer.Character
    local root = char and (char.PrimaryPart or char:FindFirstChild("HumanoidRootPart"))
    if not root or not target then return false end
    local targetPos = useBoat and target.Position or (target.PrimaryPart and target.PrimaryPart.Position)
    if not targetPos then return false end
    local dir = (targetPos - root.Position).Unit
    local toolName = NameWeapon("Blox Fruit")
    local tool = toolName and char:FindFirstChild(toolName)
    if not tool then return false end
    local leftClick = tool:FindFirstChild("LeftClickRemote")
    if leftClick then
        v_u_51 += 1
        if v_u_51 > 5 then v_u_51 = 1 end
        leftClick:FireServer(dir, v_u_51)
        return true
    end
    return false
end

local function AttackFunctionnhungSuperTrial()
    if not localPlayer.Character or not localPlayer.Character:FindFirstChild("Stun") or localPlayer.Character.Stun.Value ~= 0 then return end
    local hits = AttackAOE(getgenv().AutoClickRange, true)
    if not hits then return end
    RegisterAttack:FireServer(0)
    RegisterHit:FireServer(table.remove(hits, 1)[2], hits)
end

getgenv().AutoClickEnabled = false
getgenv().AutoClickRange = 80
getgenv().AutoClickAttacks = 0

-- AQUI ESTÁ A MUDANÇA: O LOOP AGORA É ULTRA RÁPIDO (HEARTBEAT + MULTI-HIT)
RunService.Heartbeat:Connect(function()
    if getgenv().AutoClickEnabled then
        pcall(function()
            local fruitName = NameWeapon("Blox Fruit")
            if fruitName and localPlayer.Character and localPlayer.Character:FindFirstChild(fruitName) then
                local hits = AttackAOE(getgenv().AutoClickRange, true)
                if hits then 
                    -- Ataca TODOS no raio, não apenas um
                    for _, hit in ipairs(hits) do
                        UseFruitM1(hit[1])
                    end
                    getgenv().AutoClickAttacks += 1
                    UpdateActivity() 
                end
            else
                -- Se não tiver fruta, o AOE hit já bate em todos de uma vez
                AttackFunctionnhungSuperTrial()
                getgenv().AutoClickAttacks += 1
                UpdateActivity()
            end
        end)
    end
end)

local SecClick = PageClick.CreateSection("Auto Click")
SecClick.CreateToggle({ Title = "Auto Click", Default = false }, function(v)
    getgenv().AutoClickEnabled = v
    A.CreateNoti({ Title = "bananaXpvp", Desc = "Auto Click: " .. (v and "Ligado" or "Desligado"), ShowTime = 3 })
end)
SecClick.CreateSlider({ Title = "Raio do Ataque (Range)", Min = 20, Max = 200, Default = getgenv().AutoClickRange }, function(v) getgenv().AutoClickRange = v end)
SecClick.CreateButton({ Title = "Resetar Contador" }, function() getgenv().AutoClickAttacks = 0 end)
local StatusLabel = SecClick.CreateLabel({ Title = "Status: Desligado | Ataques: 0" })

task.spawn(function()
    while true do
        task.wait(0.5)
        if StatusLabel and StatusLabel.SetText then
            pcall(function()
                StatusLabel.SetText("Status: " .. (getgenv().AutoClickEnabled and "Rodando" or "Desligado") .. " | Ataques: " .. tostring(getgenv().AutoClickAttacks))
            end)
        end
    end
end)

print("========================================")
print("  bananaXpvp Hub Carregado com Sucesso!")
print("  V4 Farm configurado com limite de NPCs.")
print("========================================")
