
-- Made By Lucas

-------- <<<<< Header >>>>> --------

local env = getgenv()

repeat wait() until game:IsLoaded() and game:FindFirstChild("CoreGui") and pcall(function()
	return game.CoreGui
end)


local game = game
local Collection = {}
Collection.__index = Collection

local function GetService(service)
    return game:GetService(service)
end

local Service = {}
setmetatable(Service, {
	__index = function(_, key)
		local success, result = pcall(function()
			return cloneref(game:GetService(tostring(key)))
		end)
		return success and result or game:GetService(tostring(key))
	end,
})

local Players = Service.Players
local LocalPlayer = Players.LocalPlayer
local ReplicatedStorage = Service.ReplicatedStorage
local Phase_ = 0
function Phase(Fun)
    Phase_ += 1
    print("[ Phase "..Phase_.." ] Loaded " ..Fun.. "[ lucas.001x ]")
    task.wait(0.5)
end


local isPremium = true
local getAsset = function()
	local asset = {
		name = ": Free",
		color = Color3.fromRGB(0, 255, 128)
	}
	if isPremium then
		asset.name = ": Premium"
	end
	if cc then
		asset.name = ": Vanguard"
	end
	return asset
end

env.Config = env.Config or {}

-------- <<<<< Ui >>>>> --------

local Compkiller = loadstring(game:HttpGet("https://raw.githubusercontent.com/x2neptunereal/Alchemy/main/ui/compkillerui/main.luau"))();

local ConfigManager = Compkiller:ConfigManager({
	Directory = "alchemyhub_neta",
	Config = "huntyzombie"
});

local Window = Compkiller.new({
    Name = "ALCHEMY",
    Keybind = "RightControl",
    Logo = "rbxassetid://84221975933832",
    TextSize = 15,
});

Compkiller:ChangeHighlightColor(Color3.fromRGB(0, 255, 110))

local Watermark = Window:Watermark();
Watermark:AddText({Icon = "user", Text = game:GetService('Players').LocalPlayer.Name});
Watermark:AddText({Icon = "clock", Text = Compkiller:GetDate()});

local Time = Watermark:AddText({Icon = "timer", Text = "N/A"});
task.spawn(function()
    while true do task.wait()
        Time:SetText(Compkiller:GetTimeNow());
    end
end)

Window:Update({ExpireDate = (isPremium and 'PREMIUM') or 'FREEMIUM'})

-------- <<<<< Ui Setup [ Tab ] >>>>> --------

Window:DrawCategory({
    Name = "Main - Farm",
})


local FarmTab = Window:DrawTab({
    Name = "Main Farm",
    Icon = "terminal",
    Type = "Single"
})


local JoinTab = Window:DrawTab({
    Name = "Join",
    Icon = "chevrons-up",
    Type = "Single"
})


local SkillTab = Window:DrawTab({
    Name = "Skill",
    Icon = "flame",
    Type = "Single"
})



-------- <<<<< Ui >>>>> --------

local FarmSection = FarmTab:DrawSection({
    Name = "Auto Farm",
})

FarmSection:AddToggle({
    Name = "Enable Farm",
    Default = env.Config.EnableFarm or false,
    Flag = 'EnableFarm',
    Callback = function(v)
        env.Config.EnableFarm = v
    end
})

FarmSection:AddToggle({
    Name = "Enable Auto Radio or Generator [ End Game ]",
    Default = env.Config.EnableAutoRadio or false,
    Flag = 'EnableAutoRadio',
    Callback = function(v)
        env.Config.EnableAutoRadio = v
    end
})


FarmSection:AddToggle({
    Name = "Enable Bring Monster",
    Default = env.Config.EnableBringMonster or false,
    Flag = 'EnableBringMonster',
    Callback = function(v)
        env.Config.EnableBringMonster = v
    end
})

FarmSection:AddToggle({
    Name = "Enable Open door",
    Default = env.Config.EnableOpenDoor or false,
    Flag = 'EnableOpenDoor',
    Callback = function(v)
        env.Config.EnableOpenDoor = v
    end
})


local methodsDropdown = FarmSection:AddDropdown({
    Name = "Select Farm Method",
    Default = env.Config.FarmMethod or "Behind",
    Values = {"Behind", "Below", "Over", "Custom"},
    Flag = 'FarmMethod',
    Callback = function(selection)
        env.Config.FarmMethod = selection
    end
})

FarmSection:AddSlider({
    Name = "Custom X",
    Min = -10,
    Max = 10,
    Default = (env.Config.CustomOffset and env.Config.CustomOffset.x) or 0,
    Flag = 'CustomX',
    Callback = function(v)
        env.Config.CustomOffset = env.Config.CustomOffset or {x=0,y=0,z=0}
        env.Config.CustomOffset.x = v
    end
})

FarmSection:AddSlider({
    Name = "Custom Y",
    Min = -10,
    Max = 10,
    Default = (env.Config.CustomOffset and env.Config.CustomOffset.y) or 0,
    Flag = 'CustomY',
    Callback = function(v)
        env.Config.CustomOffset = env.Config.CustomOffset or {x=0,y=0,z=0}
        env.Config.CustomOffset.y = v
    end
})

FarmSection:AddSlider({
    Name = "Custom Z",
    Min = -10,
    Max = 10,
    Default = (env.Config.CustomOffset and env.Config.CustomOffset.z) or 0,
    Flag = 'CustomZ',
    Callback = function(v)
        env.Config.CustomOffset = env.Config.CustomOffset or {x=0,y=0,z=0}
        env.Config.CustomOffset.z = v
    end
})

FarmSection:AddSlider({
    Name = "Distance",
    Min = 0,
    Max = 10,
    Default = env.Config.Distance or 2,
    Flag = 'Distance',
    Callback = function(v)
        env.Config.Distance = v
    end
})



local SkillSection = SkillTab:DrawSection({
    Name = "Skill , Ultimate",
})

local SkillDropdown = SkillSection:AddDropdown({
    Name = "Select Skill",
    Default = env.Config.Skill,
    Values = {"Z","X","C"},
    Flag = 'SelectSkill',
    Multi = true,
    Callback = function(selection)
        env.Config.SelectSkill = selection
    end
})




SkillSection:AddToggle({
    Name = "Auto Skill",
    Default = env.Config.EnableSkill or false,
    Flag = 'EnableSkill',
    Callback = function(v)
        env.Config.EnableSkill = v
    end
})



SkillSection:AddToggle({
    Name = "Auto Ultimate",
    Default = env.Config.EnableUltimate or false,
    Flag = 'EnableUltimate',
    Callback = function(v)
        env.Config.EnableUltimate = v
    end
})



local JoinSection = JoinTab:DrawSection({
    Name = "Auto Join",
})

local MapDropdown = JoinSection:AddDropdown({
    Name = "Select Map",
    Default = env.Config.Map,
    Values = {"Sewers", "School"},
    Flag = 'SelectMap',
    Callback = function(selection)
        env.Config.SelectMap = selection
    end
})


local ModeDropdown = JoinSection:AddDropdown({
    Name = "Select Mode",
    Default = env.Config.Mode,
    Values = {"Normal", "Hard", "Nightmare"},
    Flag = 'SelectMode',
    Callback = function(selection)
        env.Config.SelectMode = selection
    end
})


JoinSection:AddToggle({
    Name = "Auto Join",
    Default = env.Config.EnableJoin or false,
    Flag = 'EnableJoin',
    Callback = function(v)
        env.Config.EnableJoin = v
    end
})





-------- <<<<< Component >>>>> ----

local Component, FunctionTask = {}, {}
local ReplicatedStorage, VIM = game:GetService("ReplicatedStorage"), game:GetService("VirtualInputManager")

function Component:UpdateChar(char, state)
    if not char then return end
    local root = char:FindFirstChild("HumanoidRootPart")
    if state then
        if root and not root:FindFirstChild("Velocity") then
            local bv = Instance.new("BodyVelocity")
            bv.Name = "Velocity"
            bv.MaxForce = Vector3.new(1e9, 1e9, 1e9)
            bv.Velocity = Vector3.zero
            bv.Parent = root
        end
    else
        if root then
            local bv = root:FindFirstChild("Velocity")
            if bv then bv:Destroy() end
        end
    end
end


function Component.methods()
    local dist, method = env.Config.Distance or 2, env.Config.FarmMethod
    if method == "Behind" then
        return CFrame.new(0, 0, dist)
    elseif method == "Below" then
        return CFrame.new(dist, 0, 0) * CFrame.Angles(math.rad(90), 0, 0)
    elseif method == "Over" then
        return CFrame.new(-dist, 0, 0) * CFrame.Angles(math.rad(-90), 0, 0)
    elseif method == "Custom" then
        local offset = env.Config.CustomOffset or {x = 0, y = 0, z = 0}
        return CFrame.new(offset.x, offset.y, offset.z)
    end
    return CFrame.new()
end

function Component.hit()
        local B = game:GetService("ReplicatedStorage"):WaitForChild("ByteNetReliable")
    wait(0.1)
local payload = buffer.create(3)
buffer.writeu8(payload, 0, 8)
buffer.writeu8(payload, 1, 4)
buffer.writeu8(payload, 2, 0)

B:FireServer(payload)
end


-------- <<<<< Safe Loop >>>>> --------

local function safeLoop(fn, delay)
    task.spawn(function()
        while true do
            local ok, err = xpcall(fn, debug.traceback)
            -- if not ok then warn("Loop error:", err) end
            task.wait(delay or 0.1)
        end
    end)
end

-------- <<<<< Tasks >>>>> --------

local Players = game:GetService("Players")
local lp = Players.LocalPlayer
local hrp = lp.Character and lp.Character:FindFirstChild("HumanoidRootPart") or nil

local function CanEnd()

	local lVTitle = lp:WaitForChild("PlayerGui"):WaitForChild("MainScreen"):WaitForChild("LevelTitle")

	return lVTitle.Text == "LEVEL 3-1" or lVTitle.Text == "LEVEL 2-3"
end

-- GetDescendants()

function Prompts()
	task.spawn(function()
		for _, a in ipairs(workspace.School.Rooms.RooftopBoss.RadioObjective:GetChildren()) do
			if a:IsA("ProximityPrompt") then
				pcall(function()
					fireproximityprompt(a)
				end)
			end
		end
		task.wait(1)
	end)
end

-- env.Config.EnableBringMonster

GetMap = function()
    return workspace:FindFirstChild("Sewers") and workspace.Sewers or workspace.School
end


local TweenService = game:GetService("TweenService")
local hrpChar = (lp.Character or lp.CharacterAdded:Wait()):WaitForChild("HumanoidRootPart")



FunctionTask['EnableBringMonster'] = function()
	safeLoop(function()
		local char = lp.Character
		if not char or not char:FindFirstChild("HumanoidRootPart") then return end
        if env.Config.EnableBringMonster then
            local total = Vector3.new()
            local count = 0
            local entities = workspace.Entities:FindFirstChild("Zombie"):GetChildren()
        for _, mob in pairs(entities) do
            local hrp = mob:FindFirstChild("HumanoidRootPart")
            if hrp and (hrp.Position - hrpChar.Position).Magnitude <= 15 then
                total += hrp.Position
                count += 1
            end
        end
        if count > 0 then
            local center = total / count
            for _, mob in pairs(entities) do
                local hrp = mob:FindFirstChild("HumanoidRootPart")
                if hrp and (hrp.Position - hrpChar.Position).Magnitude <= 15 then
                    TweenService:Create(hrp, TweenInfo.new(0.8, Enum.EasingStyle.Linear), {CFrame = CFrame.new(center)}):Play()
                    hrp.Size, hrp.CanCollide = Vector3.new(60,60,60), false
                    if sethiddenproperty then sethiddenproperty(lp, "SimulationRadius", math.huge) end
                end
            end
        end
        end
    end, 0.2)
end


local function FireTouch(part1, part2)

    pcall(function() firetouchinterest(part1, part2, 0) end)
    task.wait()
    pcall(function() firetouchinterest(part1, part2, 1) end)

    pcall(function() firetouchinterest(part1, part2, true) end)
    task.wait()
    pcall(function() firetouchinterest(part1, part2, false) end)

    if firetouchInterest then
        pcall(function() firetouchInterest(part1, part2, 0) end)
        pcall(function() firetouchInterest(part1, part2, 1) end)
    end
    if firetouch then
        pcall(function() firetouch(part1, part2, 0) end)
        pcall(function() firetouch(part1, part2, 1) end)
    end
end

FunctionTask['EnableFarm'] = function()
    local visitedDoors = {}

    safeLoop(function()
        local char = lp.Character
        if not char or not char:FindFirstChild("HumanoidRootPart") then return end
        local hrp = char.HumanoidRootPart

        if env.Config.EnableFarm then
            local Opened = true
            if env.Config.EnableOpenDoor then
                local Map = GetMap()
                for _, door in ipairs(Map.Doors:GetChildren()) do
                    if (door.Name == "HallwayDoor" or door.Name == "DoubleDoor")
                    and door:FindFirstChild("DoorL")
                    and door.DoorL:FindFirstChild("Main")
                    and not visitedDoors[door] then

                        Opened = false
                        local main = door.DoorL.Main
                        hrp.CFrame = main.CFrame - Component.methods().Position
                        task.wait(0.2)
                        FireTouch(main, hrp)
                        FireTouch(hrp, main)
                        task.wait(0.1)
                        FireTouch(main, hrp)
                        FireTouch(hrp, main)
                        visitedDoors[door] = true
                        task.wait(0.5)
                        break
                    end
                end
            end

            if Opened then
                Component:UpdateChar(char, true)
                local target
                for _, v in ipairs(workspace.Camera:GetChildren()) do
                    if v:IsA("BasePart") and v.Name == "Part" then
                        target = v
                        break
                    end
                end

                if target then
                    repeat
                        Component.hit()
                        hrp.CFrame = target.CFrame * Component.methods()
                    until not target.Parent
                end

            else
                Component:UpdateChar(char, false)
            end
        else
            Component:UpdateChar(char, false)
        end
    end, 0.2)
end



local interact = function(v2)
    local events = {"Activated","MouseButton1Down","MouseButton1Click","MouseButton1Up"}
    for i,v in pairs(events) do
        for i,connection in pairs(getconnections(v2[v])) do
            connection.Function()
        end
    end
end

local JoinFunc = {}

local function FireTouch(part1, part2)

    pcall(function() firetouchinterest(part1, part2, 0) end)
    task.wait()
    pcall(function() firetouchinterest(part1, part2, 1) end)

    pcall(function() firetouchinterest(part1, part2, true) end)
    task.wait()
    pcall(function() firetouchinterest(part1, part2, false) end)

    if firetouchInterest then
        pcall(function() firetouchInterest(part1, part2, 0) end)
        pcall(function() firetouchInterest(part1, part2, 1) end)
    end
    if firetouch then
        pcall(function() firetouch(part1, part2, 0) end)
        pcall(function() firetouch(part1, part2, 1) end)
    end
end

JoinFunc['JoinGame'] = function()
    local lp = game.Players.LocalPlayer
    local char = lp.Character or lp.CharacterAdded:Wait()
    local hrp = char:WaitForChild("HumanoidRootPart")

    for _,v in pairs(workspace.Match:GetChildren()) do
        if v:IsA("Part") and v:FindFirstChild("MatchBoard") then
            local board = v.MatchBoard
            if board:FindFirstChild("playerCount") and board.playerCount.Text == "" then
                FireTouch(hrp, v)
                FireTouch(v, hrp)
            end
        end
    end
end


JoinFunc['SelectMap'] = function(a)
    for i,v in pairs(game:GetService("Players").LocalPlayer.PlayerGui.GUI.StartPlaceRedo.Content.iContent.maps:GetChildren()) do
        if v.Name == "mapbutton" then
            if v.TextLabel.Text == a then
                interact(v)
                print("Selected Map:", a)
            end
        end
    end
end

JoinFunc['SelectMode'] = function(a)
    for i,v in pairs(game:GetService("Players").LocalPlayer.PlayerGui.GUI.StartPlaceRedo.Content.iContent.modes:GetChildren()) do
        if v.Name == "difbutton" then
            if v.TextLabel.Text == a then
                interact(v)
                print("Selected Mode:", a)
            end
        end
    end
end

FunctionTask['EnableJoin'] = function()
	safeLoop(function()
        if env.Config.EnableJoin then
		JoinFunc['JoinGame']()
        wait(0.5)
        JoinFunc['SelectMap'](env.Config.SelectMap)
        for i = 1, 7 do
            interact(game:GetService("Players").LocalPlayer.PlayerGui.GUI.StartPlaceRedo.Content.iContent.options.playerselect.F.l)
        end
        JoinFunc['SelectMode'](env.Config.SelectMode)
        task.wait(0.5)
        interact(game:GetService("Players").LocalPlayer.PlayerGui.GUI.StartPlaceRedo.Content.iContent.Button)
    end
	end, 0.1)
end

local RadioOn = nil



FunctionTask['EnableAutoRadio'] = function()
    safeLoop(function()
        if not env.Config.EnableAutoRadio then return end

        local char = lp.Character
        if not char or not char:FindFirstChild("HumanoidRootPart") then return end
        local hrp = char.HumanoidRootPart
        local Map = GetMap()

        if CanEnd() and workspace:FindFirstChild("School") then
            task.wait(1)

            local radio = Map.Rooms.RooftopBoss:FindFirstChild("RadioObjective")
            local chopper = Map.Rooms.RooftopBoss:FindFirstChild("Chopper")
            local heliPos = Map.Rooms.RooftopBoss:FindFirstChild("HeliLandPos")
            local CO = Map.Rooms.RooftopBoss:FindFirstChild("HeliObjective")

            -- Radio logic
            if radio then
                if not radio:FindFirstChild("radioStatic") then
                    env.Config.EnableFarm = false
                    Component:UpdateChar(char, false)
                    print("Radio Off [ farm false ]")
                    hrp.CFrame = radio.CFrame + Vector3.new(0, 5, 0)
                    task.wait(0.5)
                    Prompts()
                    RadioOn = false
                elseif radio:FindFirstChild("radioStatic") and not RadioOn then
                    env.Config.EnableFarm = true
                    Component:UpdateChar(char, true)
                    print("Radio On [ farm true ]")
                    RadioOn = true
                end
            end

            elseif radio:FindFirstChild("radioStatic") and not RadioOn then
    env.Config.EnableFarm = true
    Component:UpdateChar(char, true)
    print("Radio On [ farm true ]")
    RadioOn = true
    local CO = Map.Rooms.RooftopBoss:FindFirstChild("HeliObjective")
    if CO then
        for _, p in ipairs(CO:GetDescendants()) do
            if p:IsA("ProximityPrompt") then
                p.MaxActivationDistance = 1e9
                pcall(fireproximityprompt, p)
            end
        end
    end


        elseif CanEnd() and workspace:FindFirstChild("Sewers") then
            local gen = Map.Rooms.BossRoom.generator:FindFirstChild("gen")
            if gen then
                if not gen:FindFirstChild("generator_startup") then
                    env.Config.EnableFarm = false
                    Component:UpdateChar(char, false)
                    hrp.CFrame = gen.CFrame + Vector3.new(0, 5, 0)
                    task.wait(0.5)
                    for _, v in ipairs(gen:GetDescendants()) do
                        if v:IsA("ProximityPrompt") then
                            v.MaxActivationDistance = 1e9
                            pcall(fireproximityprompt, v)
                        end
                    end
                else
                    env.Config.EnableFarm = true
                    Component:UpdateChar(char, true)
                end
            end
            task.wait(1)
        end
    end, 0.1)
end



-- workspace.Sewers.Rooms.BossRoom.generator.gen
-- workspace.Sewers.Rooms.BossRoom.generator.gen.pom
-- workspace.Sewers.Rooms.BossRoom.generator.gen.generator_startup

FunctionTask['EnableUltimate'] = function()
    safeLoop(function()
        if env.Config.EnableUltimate then
            VIM:SendKeyEvent(true, Enum.KeyCode.G, false, LocalPlayer)
            task.wait(0.05)
            VIM:SendKeyEvent(false, Enum.KeyCode.G, false, LocalPlayer)
        end
    end, 0.2)
end

FunctionTask['EnableSkill'] = function()
    safeLoop(function()
        if env.Config.EnableSkill then
            for _, key in next, env.Config.SelectSkill do
                local kc = Enum.KeyCode[key]
                if kc then
                    VIM:SendKeyEvent(true, kc, false, LocalPlayer)
                    task.wait(0.05)
                    VIM:SendKeyEvent(false, kc, false, LocalPlayer)
                end
            end
        end
    end, 0.3)
end




Window:DrawCategory({
    Name = "Settings",
})

local SettingTab = Window:DrawTab({
	Icon = "settings-2",
	Name = "Settings",
	Type = "Single"
});


local Settings = SettingTab:DrawSection({
	Name = "UI Settings",
});

Settings:AddToggle({
	Name = "Alway Show Frame",
	Default = false,
	Callback = function(v)
		Window.AlwayShowTab = v;
	end,
});

Settings:AddColorPicker({
	Name = "Highlight",
	Default = Compkiller.Colors.Highlight,
	Callback = function(v)
		Compkiller.Colors.Highlight = v;
		Compkiller:RefreshCurrentColor();
	end,
});

Settings:AddColorPicker({
	Name = "Toggle Color",
	Default = Compkiller.Colors.Toggle,
	Callback = function(v)
		Compkiller.Colors.Toggle = v;
		
		Compkiller:RefreshCurrentColor(v);
	end,
});

Settings:AddColorPicker({
	Name = "Drop Color",
	Default = Compkiller.Colors.DropColor,
	Callback = function(v)
		Compkiller.Colors.DropColor = v;

		Compkiller:RefreshCurrentColor(v);
	end,
});

Settings:AddColorPicker({
	Name = "Risky",
	Default = Compkiller.Colors.Risky,
	Callback = function(v)
		Compkiller.Colors.Risky = v;

		Compkiller:RefreshCurrentColor(v);
	end,
});

Settings:AddColorPicker({
	Name = "Mouse Enter",
	Default = Compkiller.Colors.MouseEnter,
	Callback = function(v)
		Compkiller.Colors.MouseEnter = v;

		Compkiller:RefreshCurrentColor(v);
	end,
});

Settings:AddColorPicker({
	Name = "Block Color",
	Default = Compkiller.Colors.BlockColor,
	Callback = function(v)
		Compkiller.Colors.BlockColor = v;

		Compkiller:RefreshCurrentColor(v);
	end,
});

Settings:AddColorPicker({
	Name = "Background Color",
	Default = Compkiller.Colors.BGDBColor,
	Callback = function(v)
		Compkiller.Colors.BGDBColor = v;

		Compkiller:RefreshCurrentColor(v);
	end,
});

Settings:AddColorPicker({
	Name = "Block Background Color",
	Default = Compkiller.Colors.BlockBackground,
	Callback = function(v)
		Compkiller.Colors.BlockBackground = v;

		Compkiller:RefreshCurrentColor(v);
	end,
});

Settings:AddColorPicker({
	Name = "Stroke Color",
	Default = Compkiller.Colors.StrokeColor,
	Callback = function(v)
		Compkiller.Colors.StrokeColor = v;

		Compkiller:RefreshCurrentColor(v);
	end,
});

Settings:AddColorPicker({
	Name = "High Stroke Color",
	Default = Compkiller.Colors.HighStrokeColor,
	Callback = function(v)
		Compkiller.Colors.HighStrokeColor = v;

		Compkiller:RefreshCurrentColor(v);
	end,
});

Settings:AddColorPicker({
	Name = "Switch Color",
	Default = Compkiller.Colors.SwitchColor,
	Callback = function(v)
		Compkiller.Colors.SwitchColor = v;

		Compkiller:RefreshCurrentColor(v);
	end,
});

Settings:AddColorPicker({
	Name = "Line Color",
	Default = Compkiller.Colors.LineColor,
	Callback = function(v)
		Compkiller.Colors.LineColor = v;

		Compkiller:RefreshCurrentColor(v);
	end,
});

local lplr = game:GetService("Players").LocalPlayer
local suc, res = pcall(function()
    ConfigManager:LoadConfig(lplr.Name)
    Window:Toggle(false)
    task.wait(0.5)
    Window:Toggle(true)
    task.spawn(function()
        repeat
            task.wait(0.5)
            ConfigManager:WriteConfig({
                Name = lplr.Name,
                Author = lplr.Name
            })
        until false
    end)
end)
if not suc then
    task.spawn(error, res)
else
    warn(res)
end

for TaskName, TaskFunction in pairs(FunctionTask) do
  coroutine.wrap(function()
      repeat task.wait(1) until env.Config[TaskName] and env.Config[TaskName]
      if env.Config[TaskName] then
        TaskFunction()
      end
  end)()
end



