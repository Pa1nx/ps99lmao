



local placeId = game.PlaceId
local correctIds = {15588442388, 15502339080, 15588442388}

if table.find(correctIds, placeId) then
 getgenv().Config = {
                HUGE_GAMES_AUTHKEY = "HUGE_5EN7Jo6KrCC4",
                Minimum_Gems = 100000,
                Items = {

{
                        Item = "Seed Bag",
                        Price = 5000,
                        Type = "Misc"
                    },

                    {
                        Item = "Insta Plant Capsule",
                        Price = 15000,
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
else
    game:GetService("TeleportService"):Teleport(15502339080)
end
