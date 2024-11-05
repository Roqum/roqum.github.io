+++
date = '2023-12-27T16:12:33+02:00'
draft = false
title = 'Charming Hell'
categories = ['C++', 'Unreal Engine', 'Photoshop']
projectDescription = ["As an evil landlord, the player has to invest his gold wisely and quickly to plunder as much money as possible from the tenants before his land faces its doom.", "", "Submission for the three-day game jam 'Melon Jam 2023'"]
+++

## Project Info

GitHub: [github.com/Roqum/CharmingHell](https://github.com/Roqum/CharmingHell_3DayGameJam)\
Play in Browser: [roqum.itch.io/charming-hell](https://roqum.itch.io/charming-hell)\
Working Hours: ~30h


## The Beginning

The theme for the Winter Melon Jam 2023 was "Charm." I was disappointed. I already had several cool game genres in mind that I was excited to try out. So my anticipation for the jam was huge. But "Charm"? What should I do with that? It did not fit any idea at all.

However, I had already set aside the three days, so I decided to give it a shot. The theme was announced around 6 pm., so I chose not to start programming that evening. Instead, I spent the rest of the night planning the following days and, most importantly, coming up with a game idea.

###### First Idea

After some brainstorming, inspiration struck me in the shower—I had an idea I was excited to pursue. I envisioned a small, city-building game where players attract residents to their plot of land. The goal would be to increase the "charm" of the area to encourage more people to settle there.

However, I was missing a clear game objective. In traditional city-building games, the enjoyment comes from complex systems that players explore and use to expand their city. It quickly became clear that I wouldn’t have the time to develop intricate, functional systems within a three-day window. So, I needed to define a different game objective to keep the project scope manageable.

###### The Shift to a High-Score Challenge

I decided to narrow the project’s scope by shifting the game’s main objective to collecting points within a set time limit. Rather than a relaxed city-builder, it became the complete opposite — a stressful race against the clock. Resources needed for construction also serve as points, and the available land for building (and generating income) shrinks rapidly. What first appears to be a "charming" game quickly turns into the exact opposite — hell. I am not sure but maybe this twist reflects my frustration with this year’s game jam theme.

The player’s high score is determined by the amount of gold they collect. However, spending gold to construct buildings is essential to increase gold income, creating a careful balance. Meanwhile, frequent natural disasters devastate the land, intensifying the challenge.

Now my idea was ready to be refined so that development can begin.

## Game Plot

As the player, you take on the role of the villain — a ruthless landlord who just snagged a prime piece of land at a bargain price. Your goal? To rent it out and make as much gold as possible. To attract tenants willing to pay top dollar, you’ll need to make the land as “charming” as possible. The trick is to invest wisely so you can walk away with a hefty profit. Oh, and there’s an active volcano right next door that’s allready erupting, so you’ll have to act fast to collect rent before all the buildings are destroyed.

## Gameplay Mechanics


Here’s a smooth translation for this section:

In this game, there are five key parameters:

- Gold
- Gold Income
- Population Count
- Maximum Population Capacity
- Total Charm
###### Gold
Gold serves a dual purpose: it’s the currency you need to build new structures, and it’s also your final score at the end of each round. To achieve the highest possible score, you’ll want to keep as much gold as possible by the end of the game. At the same time, you’ll need to spend gold to increase your gold income.

This creates a challenging balance for the player, who must carefully consider whether each investment will be worthwhile in the long run or end up costing more than it returns.
###### Gold Income
Gold income is straightforward: every 5 seconds, the player receives their income payout. The income amount is directly tied to the population count at a 1:1 ratio.
For example, if the player has 100 residents, they’ll earn 100 gold every 5 seconds.
###### Population Count
As mentioned, the population count is essential for generating gold. You can increase the number of residents by constructing housing. However, the population is limited by the maximum population capacity. Once this limit is reached, building additional housing is no longer possible.
###### Maximum Population Capacity
The maximum population capacity is calculated based on the "Total Charm" using the following formula.
$$  max\_villagers = \frac{{(total\_charm)}^2}{400} $$
As the population grows, the "total Charm" will be more effectivly at increasing the capacity. 
###### Total Charm
The overall charm is responsible for the maximum population capacity. This value is the foundation for kickstarting the gold flow and can be increased by constructing specific buildings. 