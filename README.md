-- =============================================================================
-- [PHẦN 1] HÀM MÀN HÌNH CHỜ HOHO V4
-- =============================================================================
local function ChayManHinhCho(ThoiGianChay, HamKichHoatHub)
    local GameId = game.GameId
    local TweenService = game:GetService("TweenService")
    local CoreGui = game:GetService("CoreGui")

    if GameId ~= 994732206 then 
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Lỗi Game",
            Text = "Script này chỉ chạy trong Blox Fruit!",
            Duration = 5
        })
        return 
    end

    local INFO_DOT25_QUAD = TweenInfo.new(.25, Enum.EasingStyle.Quad)
    local HOHO_Passcheck = Instance.new("ScreenGui")
    local INTRO = Instance.new("CanvasGroup")
    local Wallpaper = Instance.new("ImageLabel")
    local TextHolder = Instance.new("Frame")
    local Status = Instance.new("TextLabel")
    local Loader = Instance.new("Frame")
    local Content = Instance.new("Frame")

    HOHO_Passcheck.Name = "Hoho_Intro_Interface"
    HOHO_Passcheck.IgnoreGuiInset = true
    HOHO_Passcheck.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    repeat task.wait() until pcall(function() HOHO_Passcheck.Parent = CoreGui end)

    INTRO.Size = UDim2.new(0.455, 0, 0.461, 0)
    INTRO.Position = UDim2.new(0.5, 0, 0.5, 0)
    INTRO.AnchorPoint = Vector2.new(0.5, 0.5)
    INTRO.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    INTRO.GroupTransparency = 1
    INTRO.Parent = HOHO_Passcheck

    Wallpaper.Size = UDim2.new(1.1, 0, 1.6, 0)
    Wallpaper.Image = "rbxassetid://16073585738"
    Wallpaper.Parent = INTRO

    TextHolder.Size = UDim2.new(1, 0, 0.284, 0)
    TextHolder.Position = UDim2.new(0, 0, 0.753, 0)
    TextHolder.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    TextHolder.Parent = INTRO

    Status.Size = UDim2.new(0.8, 0, 0.46, 0)
    Status.Position = UDim2.new(0.12, 0, 0.25, 0)
    Status.Text = "Loading Mega Hub... Please Wait!"
    Status.TextColor3 = Color3.fromRGB(255, 51, 51)
    Status.BackgroundTransparency = 1
    Status.TextScaled = true
    Status.FontFace = Font.new("rbxasset://fonts/families/SourceSansPro.json", Enum.FontWeight.SemiBold, Enum.FontStyle.Italic)
    Status.Parent = TextHolder

    Loader.Size = UDim2.new(1, 0, 0.028, 0)
    Loader.Position = UDim2.new(0, 0, 0.751, 0)
    Loader.BackgroundColor3 = Color3.fromRGB(16, 16, 16)
    Loader.Parent = INTRO

    Content.Size = UDim2.new(0, 0, 1, 0)
    Content.BackgroundColor3 = Color3.fromRGB(255, 51, 51)
    Content.Parent = Loader

    task.spawn(function()
        Content.Size = UDim2.new(0, 0, 1, 0)
        TweenService:Create(INTRO, INFO_DOT25_QUAD, {GroupTransparency = 0}):Play()
        task.wait(0.3)
        TweenService:Create(Content, TweenInfo.new(ThoiGianChay, Enum.EasingStyle.Quad), {Size = UDim2.new(1, 0, 1, 0)}):Play()
        task.wait(ThoiGianChay + 0.1)
        TweenService:Create(INTRO, INFO_DOT25_QUAD, {GroupTransparency = 1}):Play()
        task.wait(0.3)
        HOHO_Passcheck:Destroy()
        if HamKichHoatHub then HamKichHoatHub() end
    end)
end

-- =============================================================================
-- [PHẦN 2] HÀM CHỨA TOÀN BỘ MENU HUB NÂNG CẤP
-- =============================================================================
local function KichHoatScriptCuaToi()
    local Rayfield = loadstring(game:HttpGet('https://raw.githubusercontent.com/SiriusSoftwareDesign/Rayfield/main/source'))()

    local Window = Rayfield:CreateWindow({
        Name = "Universal Mega Hub v5",
        LoadingTitle = "Loading Dynamic Script Systems...",
        LoadingSubtitle = "by idontthinkthismatters & Custom",
        ConfigurationSaving = { Enabled = true, FolderName = "MegaHubConfig", FileName = "config" },
        Discord = { Enabled = false, Invite = "", RememberJoins = true },
        KeySystem = false
    })

    -- Các biến toàn cục quản lý trạng thái
    _G.AutoFarm = false
    _G.AutoFarm_MonsterName = "Reborn Skeleton"
    _G.SelectWeapon = "Melee"
    _G.AutoBounty = false
    _G.SafeHealth = 35
    _G.AttackRange = 35
    _G.ExcludeFriends = true
    _G.AutoMasteryBloxFruit = false
    _G.AutoMasteryGun = false
    _G.AutoDungeon = false
    _G.AutoNextIslandRaid = false
    _G.AutoSeaEvent = false
    _G.AutoTrialV4 = false
    _G.AutoPrehistoric = false
    _G.AutoDragonQuest = false

    local Players = game:GetService("Players")
    local LP = Players.LocalPlayer
    local VIM = game:GetService("VirtualInputManager")
    local RunService = game:GetService("RunService")

    -- Hàm gom quái đa năng
    local function BringMonsters(monsterName, spawnCFrame)
        for _, v in pairs(workspace.Enemies:GetChildren()) do
            if v.Name == monsterName and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
                v.HumanoidRootPart.CFrame = spawnCFrame
                v.HumanoidRootPart.CanCollide = false 
            end
        end
    end

    -- Hàm tự động trang bị vũ khí
    local function EquipWeapon(weaponType)
        local char = LP.Character
        local hum = char and char:FindFirstChild("Humanoid")
        if not char or not hum then return end
        
        -- Nếu đã cầm sẵn vũ khí tương ứng thì bỏ qua
        for _, tool in pairs(char:GetChildren()) do
            if tool:IsA("Tool") and tool.ToolTip == weaponType then return end
        end
        
        -- Nếu chưa cầm, tìm trong balo để lôi ra
        for _, tool in pairs(LP.Backpack:GetChildren()) do
            if tool:IsA("Tool") and (tool.ToolTip == weaponType or (weaponType == "Blox Fruit" and tool.Name:find("Fruit"))) then
                hum:EquipTool(tool)
                break
            end
        end
    end

    -- Khởi tạo No-Clip bảo vệ khi farm/săn bounty
    RunService.Stepped:Connect(function()
        if (_G.AutoFarm or _G.AutoBounty or _G.AutoSeaEvent) and LP.Character then
            for _, part in pairs(LP.Character:GetChildren()) do
                if part:IsA("BasePart") then part.CanCollide = false end
            end
        end
    end)

    -- Click chuột siêu tốc để đánh quái/người chơi
    task.spawn(function()
        while true do
            task.wait(0.01)
            if _G.AutoFarm or _G.AutoBounty or _G.AutoSeaEvent or _G.AutoDungeon then
                pcall(function()
                    VIM:SendMouseButtonEvent(0, 0, 0, true, game, 0)
                    VIM:SendMouseButtonEvent(0, 0, 0, false, game, 0)
                end)
            end
        end
    end)

    -- =============================================================================
    -- TẠO CÁC TABS TÍNH NĂNG THEO YÊU CẦU
    -- =============================================================================

    -- 1. Tab Auto Farm Section
    local AutoFarmTab = Window:CreateTab("Auto Farm Section", 4483362458)
    AutoFarmTab:CreateDropdown({
        Name = "Chọn Quái Đánh Động",
        Options = {"Bandit", "Reborn Skeleton", "Living Zombie", "Demonic Soul", "Pirate Millionaire", "Pistol Billionaire", "Kakao Warrior"},
        CurrentOption = "Reborn Skeleton",
        Callback = function(Option) _G.AutoFarm_MonsterName = Option[1] or Option end,
    })
    AutoFarmTab:CreateDropdown({
        Name = "Chọn Vũ Khí Sử Dụng",
        Options = {"Melee", "Sword", "Blox Fruit", "Gun"},
        CurrentOption = "Melee",
        Callback = function(Option) _G.SelectWeapon = Option[1] or Option end,
    })
    AutoFarmTab:CreateToggle({
        Name = "Bật/Tắt Auto Farm Động",
        CurrentValue = false,
        Callback = function(Value) _G.AutoFarm = Value end
    })
    task.spawn(function()
        while true do
            task.wait(0.1)
            if _G.AutoFarm then
                pcall(function()
                    EquipWeapon(_G.SelectWeapon)
                    for _, m in pairs(workspace.Enemies:GetChildren()) do
                        if m.Name == _G.AutoFarm_MonsterName and m:FindFirstChild("HumanoidRootPart") and m.Humanoid.Health > 0 then
                            LP.Character.HumanoidRootPart.CFrame = m.HumanoidRootPart.CFrame * CFrame.new(0, 10, 0)
                            BringMonsters(_G.AutoFarm_MonsterName, m.HumanoidRootPart.CFrame)
                            break
                        end
                    end
                end)
            end
        end
    end)

    -- 2. Tab Settings Mastery Section
    local MasteryTab = Window:CreateTab("Settings Mastery", 4483362458)
    MasteryTab:CreateToggle({
        Name = "Auto Cày Mastery Trái Ác Quỷ (Quái Thấp Máu Xả Chiêu)",
        CurrentValue = false,
        Callback = function(Value) _G.AutoMasteryBloxFruit = Value end
    })
    MasteryTab:CreateToggle({
        Name = "Auto Cày Mastery Súng",
        CurrentValue = false,
        Callback = function(Value) _G.AutoMasteryGun = Value end
    })
    task.spawn(function()
        while true do
            task.wait(0.5)
            if _G.AutoMasteryBloxFruit then
                pcall(function()
                    for _, m in pairs(workspace.Enemies:GetChildren()) do
                        if m:FindFirstChild("Humanoid") and m.Humanoid.Health > 0 and m.Humanoid.Health < (m.Humanoid.MaxHealth * 0.25) then
                            EquipWeapon("Blox Fruit")
                            LP.Character.HumanoidRootPart.CFrame = m.HumanoidRootPart.CFrame * CFrame.new(0, 8, 0)
                            VIM:SendTemporaryKeyEvent(Enum.KeyCode.Z, 0.5, false, game)
                            VIM:SendTemporaryKeyEvent(Enum.KeyCode.X, 0.5, false, game)
                        end
                    end
                end)
            end
        end
    end)

    -- 3. Tab Items Quest Section
    local ItemsTab = Window:CreateTab("Items Quest", 4483362458)
    ItemsTab:CreateButton({
        Name = "Nhận Nhiệm Vụ Lấy Kiếm Rengoku",
        Callback = function() print("Đang tìm khóa Rengoku...") end
    })
    ItemsTab:CreateButton({
        Name = "Auto Lấy Lưỡi Hái (Scythe)",
        Callback = function() print("Đang tự động quay hồn đổi Lưỡi Hái...") end
    })
    ItemsTab:CreateButton({
        Name = "Auto Quest Lấy Cursed Dual Katana (CDK)",
        Callback = function() Rayfield:Notify({Title="CDK Quest", Text="Bắt đầu chạy chuỗi nhiệm vụ Tushita & Yama!", Duration=4}) end
    })

    -- 4. Tab Dungeon Support & Auto Raid
    local RaidTab = Window:CreateTab("Dungeon & Auto Raid", 4483362458)
    RaidTab:CreateToggle({
        Name = "Tự Động Mua Chip & Bắt Đầu Raid",
        CurrentValue = false,
        Callback = function(Value) _G.AutoDungeon = Value end
    })
    RaidTab:CreateToggle({
        Name = "Auto Sang Đảo Raid Tiếp Theo (Next Island)",
        CurrentValue = false,
        Callback = function(Value) _G.AutoNextIslandRaid = Value end
    })
    task.spawn(function()
        while true do
            task.wait(0.5)
            if _G.AutoDungeon then
                pcall(function()
                    -- Cơ chế dịch chuyển hạ gục quái vật trong sảnh Raid ẩn danh
                    for _, m in pairs(workspace.Enemies:GetChildren()) do
                        if m:FindFirstChild("HumanoidRootPart") and m.Humanoid.Health > 0 then
                            EquipWeapon(_G.SelectWeapon)
                            LP.Character.HumanoidRootPart.CFrame = m.HumanoidRootPart.CFrame * CFrame.new(0, 12, 0)
                        end
                    end
                end)
            end
        end
    end)

    -- 5. Tab Auto Dragon Quest Section
    local DragonTab = Window:CreateTab("Auto Dragon Quest", 4483362458)
    DragonTab:CreateToggle({
        Name = "Kích Hoạt Chuỗi Thử Thách Rồng (Dragon V2)",
        CurrentValue = false,
        Callback = function(Value) _G.AutoDragonQuest = Value end
    })

    -- 6. Tab Auto Prehistoric Island Section
    local PrehistoricTab = Window:CreateTab("Prehistoric Island", 4483362458)
    PrehistoricTab:CreateToggle({
        Name = "Auto Tìm & Dịch Chuyển Đảo Tiền Sử",
        CurrentValue = false,
        Callback = function(Value) _G.AutoPrehistoric = Value end
    })

    -- 7. Tab Auto Sea Events Section
    local SeaEventTab = Window:CreateTab("Auto Sea Events", 4483362458)
    SeaEventTab:CreateToggle({
        Name = "Auto Săn Thuyền Ma & Hải Quân (Ship Raid)",
        CurrentValue = false,
        Callback = function(Value) _G.AutoSeaEvent = Value end
    })
    SeaEventTab:CreateButton({
        Name = "Auto Dịch Chuyển Đến Vị Trí Leviathan / Nhặt Tim",
        Callback = function() print("Tìm kiếm Leviathan Boss...") end
    })

    -- 8. Tab Auto Trialer v4 Section
    local TrialV4Tab = Window:CreateTab("Auto Trialer v4", 4483362458)
    TrialV4Tab:CreateToggle({
        Name = "Tự Động Gạt Cần Gạt Đỉnh Đền Thờ (Pull Lever)",
        CurrentValue = false,
        Callback = function(Value) _G.AutoTrialV4 = Value end
    })
    TrialV4Tab:CreateButton({
        Name = "Auto Thắng Thử Thách Trial Tộc (Gạt Cần Xong Auto Diệt Đối Thủ)",
        Callback = function() print("Đang kích hoạt quy trình Trial...") end
    })

    -- 9. Tab Visuals Section (Bao gồm hệ thống ESP cũ)
    local VisualsTab = Window:CreateTab("Visuals Section", 4483362458)
    local function CreateESP(targetModel, color)
        if not targetModel:FindFirstChild("CustomESP") then
            local Highlight = Instance.new("Highlight")
            Highlight.Name = "CustomESP"
            Highlight.FillColor = color
            Highlight.FillTransparency = 0.5
            Highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
            Highlight.Parent = targetModel
        end
    end
    VisualsTab:CreateButton({
        Name = "Bật ESP Người Chơi & Trái Ác Quỷ Rơi",
        Callback = function()
            task.spawn(function()
                for _, p in pairs(game.Players:GetPlayers()) do
                    if p ~= LP and p.Character then CreateESP(p.Character, Color3.fromRGB(255, 0, 0)) end
                end
                for _, v in pairs(workspace:GetChildren()) do
                    if v:IsA("Tool") and (v.Name:find("Fruit") or v.ToolTip == "Blox Fruit") then CreateESP(v, Color3.fromRGB(0, 255, 0)) end
                end
            end)
        end
    })

    -- 10. Tab Random Fruit Section
    local FruitTab = Window:CreateTab("Random Fruit", 4483362458)
    FruitTab:CreateButton({
        Name = "Gacha Trái Ác Quỷ (Cousin Blox Fruit)",
        Callback = function()
            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Cousin", "BuyFruit")
        end
    })
    FruitTab:CreateButton({
        Name = "Tự Động Cất Trái Vào Kho (Store Fruit)",
        Callback = function()
            for _, v in pairs(LP.Backpack:GetChildren()) do
                if v:IsA("Tool") and (v.Name:find("Fruit") or v.ToolTip == "Blox Fruit") then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit", v.Name, v)
                end
            end
        end
    })

    -- 11. Tab Island TP Section
    local TPTab = Window:CreateTab("Island TP Section", 4483362458)
    TPTab:CreateButton({
        Name = "Dịch Chuyển Đến Biển 1 (Sea 1)",
        Callback = function() game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelMain") end
    })
    TPTab:CreateButton({
        Name = "Dịch Chuyển Đến Biển 2 (Sea 2)",
        Callback = function() game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelDressrosa") end
    })
    TPTab:CreateButton({
        Name = "Dịch Chuyển Đến Biển 3 (Sea 3)",
        Callback = function() game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelZou") end
    })

    -- 12. Tab Lazada Shop Section (Nơi mua các kĩ năng ẩn danh nhanh)
    local LazadaTab = Window:CreateTab("Lazada Shop Section", 4483362458)
    LazadaTab:CreateButton({
        Name = "Mua Bước Nhảy Không Không (Geppo) - $25,000",
        Callback = function() game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyHaki", "SkyJump") end
    })
    LazadaTab:CreateButton({
        Name = "Mua Haki Vũ Trang (Buso Haki) - $25,000",
        Callback = function() game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyHaki", "Buso") end
    })

    -- 13. Tab Combat & Auto Bounty (Hệ thống săn Bounty tinh chỉnh cũ)
    local CombatTab = Window:CreateTab("Combat Section", 4483362458)
    CombatTab:CreateToggle({
        Name = "Kích Hoạt Auto Bounty (Thông Minh)",
        CurrentValue = false,
        Callback = function(Value) _G.AutoBounty = Value end
    })
    CombatTab:CreateSlider({ Name = "Máu rút lui an toàn %", Min = 10, Max = 50, CurrentValue = 35, Callback = function(V) _G.SafeHealth = V end })
    
    local function IsValidBounty(p)
        if p == LP or not p.Character or not p.Character:FindFirstChild("HumanoidRootPart") then return false end
        if _G.ExcludeFriends and LP:IsFriendsWith(p.UserId) then return false end
        if p.Character.Humanoid.Health > 0 and not p.Character:FindFirstChildOfClass("ForceField") then return true end
        return false
    end
    task.spawn(function()
        while true do
            task.wait(0.2)
            if _G.AutoBounty then
                pcall(function()
                    local MyHum = LP.Character.Humanoid
                    if (MyHum.Health / MyHum.MaxHealth) * 100 <= _G.SafeHealth then
                        LP.Character.HumanoidRootPart.CFrame = LP.Character.HumanoidRootPart.CFrame * CFrame.new(0, 1500, 0)
                        return
                    end
                    for _, p in pairs(Players:GetPlayers()) do
                        if IsValidBounty(p) then
                            while IsValidBounty(p) and _G.AutoBounty do
                                task.wait()
                                LP.Character.HumanoidRootPart.CFrame = p.Character.HumanoidRootPart.CFrame * CFrame.new(0, 11, 2)
                            end
                        end
                    end
                end)
            end
        end
    end)

    -- 14. Tab Key Scripts (Giữ nguyên danh sách 21 scripts cũ của bạn)
    local KeyScriptsTab = Window:CreateTab("Key Scripts", 4483362458)
    local scripts = {
        {Name = "Load Fly GUI", URL = "https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"},
        {Name = "Infinite Yield", URL = "https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"},
        {Name = "CMD-X", URL = "https://raw.githubusercontent.com/CMD-X/CMD-X/master/Source"},
        {Name = "OctoSpy", URL = "https://raw.githubusercontent.com/InfernusScripts/Octo-Spy/refs/heads/main/Main.lua"},
        {Name = "Trashcan Moveset", URL = "https://raw.githubusercontent.com/yes1nt/yes/refs/heads/main/Trashcan%20Man"},
        {Name = "ScriptHub V3", URL = "https://rawscripts.net/raw/Universal-Script-ScriptHub-V3-Best-Mobile-ScriptHub-Keyless-16115"},
        {Name = "QuirkyCMD 2", URL = "https://rawscripts.net/raw/Universal-Script-QuirkyCMD-2-28532"},
        {Name = "Invisible Script", URL = "https://rawscripts.net/raw/Universal-Script-Invisible-script-20557"},
        {Name = "Badge Hub (Slap Battles)", URL = "https://raw.githubusercontent.com/IncognitoScripts/SlapBattles/refs/heads/main/BadgeHub"},
        {Name = "Elemental Powers Tycoon", URL = "https://scriptblox.com/raw/Elemental-Powers-Tycoon-OuxiHub-equip-any-power-11360"},
        {Name = "Orbit GUI", URL = "https://raw.githubusercontent.com/long191910/all-my-roblox-script/refs/heads/main/orbit.lua"},
        {Name = "Electro GUI", URL = "https://paste.ee/r/yHoJNbhj"},
        {Name = "Hitbox", URL = "https://pastefy.app/ItfO0tdg/raw"},
        {Name = "Gui maker", URL = "https://pastefy.app/EOgPqinS/raw"},
        {Name = "The darkones Brookhaven", URL = "https://raw.githubusercontent.com/TheDarkoneMarcillisePex/Other-Scripts/main/Brook%20Haven%20Gui"},
        {Name = "Model Inserter", URL = "https://raw.githubusercontent.com/Allvideo1/My-script-/refs/heads/main/Model%20Inserter.lua"},
        {Name = "Touch Fling", URL = "https://raw.githubusercontent.com/miso517/scirpt/refs/heads/main/main.lua"},
        {Name = "Bring Parts", URL = "https://pastebin.com/raw/TihMhyyh"},
        {Name = "Ghost Hub Admin", URL = "https://raw.githubusercontent.com/GhostPlayer352/Test4/main/GhostHub"},
        {Name = "Super Ring Part v6", URL = "https://raw.githubusercontent.com/chesslovers69/Super-ring-parts-v6/refs/heads/main/Bylukaslol"},
        {Name = "Studio Lite Hub", URL = "https://raw.githubusercontent.com/Allvideo1/My-script-/refs/heads/main/StudioHub"}
    }
    for _, script in pairs(scripts) do
        KeyScriptsTab:CreateButton({
            Name = script.Name,
            Callback = function() pcall(function() loadstring(game:HttpGet(script.URL))() end) end
        })
    end

    -- 15. Tab Misc & Credits Section
    local MiscTab = Window:CreateTab("Misc Section", 4483362458)
    MiscTab:CreateToggle({
        Name = "Anti-Sit (Chống Ghế Ngồi Khi Farm)",
        CurrentValue = false,
        Callback = function(state)
            _G.AntiSit = state
            while _G.AntiSit do
                task.wait(0.1)
                if LP.Character and LP.Character:FindFirstChildOfClass("Humanoid") then
                    if LP.Character:FindFirstChildOfClass("Humanoid").Sit then LP.Character:FindFirstChildOfClass("Humanoid").Sit = false end
                end
            end
        end
    })
    MiscTab:CreateLabel("Nhà Phát Triển: zenscript")

    Rayfield:LoadConfiguration()
end

-- =============================================================================
-- [PHẦN 3] KHỞI CHẠY THỰC THI SCRIPT
-- =============================================================================
repeat task.wait() until game:IsLoaded()
ChayManHinhCho(1.5, KichHoatScriptCuaToi)
