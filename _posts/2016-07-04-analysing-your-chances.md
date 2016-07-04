---
published: true
layout: post
---

<!--excerpt-->

## Context

For some time now, there has been a lot of controversy surrounding the CS:GO community regarding underage gambling. Recently, this was escalated when youtubers were found to decieve their audiences with "fake" winnings on gambling sites. This inspired me to write up a guide on how to calculate the chances of you winning on various gambling sites/games.

## Disclaimer

This guide will not show you how to win on gambling sites. "Winning" is not something you will be doing while gambling. Sure, there might be a few amazing wins along the way, but in the end, the ones who will be winning are the owners of the gambling site.

I should also mention that I have not completed any higher education in mathematics, but I can say with confidence that what I write here is correct.

### TL;DR

If you gamble, you will lose.

## The games

While there are hundreds of gambling sites, they all feature essentially the same games. In this guide I will disect their mechanics and show you how you can calculate the chances of you winning, as well as bust some common myths.

### Myths

To start this off, I would like to finally make one thing clear. Past results can NOT influence new results. This means that when you are, for example, playing roulette, no matter how many times in a row red has been drawn, the chance of red being drawn again is the same as always.

![](https://i.imgur.com/eB5tvmp.png)
*A streak of 10 reds*

A situation like this might lead many people to believe that red is less likely to be drawn now, since "What are the chances of red coming 11 times in a row?". Well it turns out that even after red 10 times in a row the chances are still the same. In order to understand this, imagina a coin flip. The first flip results in heads. Now, by the same logic, tails should be more likely to be the result of the next throw. However, how would the coin "know" what the previous result was? The conditions of the throw are exactly the same, nothing is different compared to the previous throw, which means the chances are exactly the same, as before. The same thing applies to roulettes, etc. Past results can NOT influence new results. 

## Roulette

Having cleared that up, we can move on to calculating actual probabilities. Let's start off with the most famous roulette strategy, the Martingale Roulette System. Basically this means that whenever you lose, you double your bet, until you win once, winning it all back.

One example of this strategy:

Round | Bet           | Win/Loss      | Earnings 
----- | ------------- | ------------- | ---- 
    1 | 2             | Loss          |   -2   
    2 | 4             | Loss          |   -6   
    3 | 8             | Loss          |  -14 
    4 | 16            | Win           |   +2 

At the end of these 4 rounds, even after losing 3 times and only winning once, you are still 2 points in the plus, i.e. winning 2 points. With this strategy, no matter how many times you lose in a row, as long as you keep doubling your bets, you will evenentually win back everything, plus your starting bet. This means if you start with e.g. 8 points, you will eventually win 8 points. 

NOTE: At this point it seems like you can lose multiple times, and winning only once will ensure that you get profit.

But here comes the catch: this strategy assumes that you can keep doubling your bet infinitly. Unfortuantly, noone has an endless amount of money, which means the strategy has a major flaw. Let's chack what the chances of winning anyway are. 

Let's assume I am playing on a roulette gambling site. First, I'll create a list of all the probabilities  we know of.

Event | Probability   
----- | ------------- 
  Red | 50%     
Black | 50%

Seems about right? Well, there is one thing that is not included in those possibilities: Green. As you might have realized, virtually all roulettes on gambling sites have an additional green field. In one example, there are 7 red fields, 7 red, and 1 green. So to calculate the probabilities, what you need to do is use a very simple formula. Divide the amount of fields of a color through the amount of all fields. This leaves us with:

Event | Probability   
----- | ------------- 
  Red | 7/15 = 0.46666.. ~= 46.5%  
Black | 7/15 = 0.46666.. ~= 46.5%
Green | 1/15 = 0.06666.. ~= 7%

Alright, now for a concrete example. With 100 points in the bank, I decide to play roulette with this strategy, starting with a bid of 10 and doubling every time I lose. I will be betting on red, but betting on red or black doesn't make any difference as they both have the same probability of being the result.

Round | Bet           | Win/Loss      | Earnings 
----- | ------------- | ------------- | ---- 
    1 | 10            | Loss          |   -10
    2 | 20            | Loss          |   -30  
    3 | 40            | Loss          |   -70 
    4 | 80            | Win           |   :(
   
After losing 3 times, I unfortunatley have no more points left to bet, and the strategy has failed me, costing me 70 points. But what are the chances of getting 3 losses in a row? I am betting on red, which means I win on 7 fields (red) and lose on 8 fields (black+green) of the 15 fields on the roulette.

Event | Probability   
----- | ------------- 
  Win | 7/15 = 0.46666.. ~= 46.5%  
 Loss | 8/15 = 0.53333.. ~= 53.5%
 
Now to calculate the probability of losing 3 times in a row, simply multiply the probability for losing.

\\( 0.5333.. \cdot 0.5333.. \cdot 0.5333.. = (0.5333...)^3 \approx 0.15 = 15% \\)

This means losing 3 times in a row has a probability of 15%. Whenever I am not losing, I am winning, so the chance of winning is the inverse of 15% => 85%. An 85% chance of winning doesn't seem too bad, odes it? However, I want to win more that 10 points, so I'll play multiple times. I'll set my goal as winning 50 points so that I end up with a total of 150 points and could double my bet once more after losing 3 in a row, bettering my chances of winnning. 

At this point it makes sense to stop looking at the game as individual bets, but instead as sets of bets, called "rounds". I can either win a round by winning the first or second bet I make, and lose it by losing 3 bets in a row.

NOTE: After looking at the game as "rounds", you see that it's is actually the other way around. You need to win multiple of these "rounds" in a row, and losing even one results in you losing almost all your points.

Now, to win 50 points, I need to win 5 "rounds". The chance of winning a round is about 85% so we can calculate the chance of winning 5 in a row.

\\( 0.85 \cdot 0.85 \cdot 0.85 \cdot 0.85 \cdot 0.85 = (0.85)^5 \approx 0.44 = 44% \\)

The chance of winning 50 points with this strategy is about 44%. This means that I would likely lose almost all of my points, instead of winning 50.

By this point I hope it is apparent that the game is a lot less in your favor than it seemed in the beginning. This is because the potential loss associated with a "round" is a lot bigger than the win. 

##Jackpot
Jackpot seems to have a lot of differences to roulette, but it is actually very similar. One of the popular strategies is to put a lot of money into the pot to have a large chance of winning. 

This essentially means you have a large chance to win a bit of money, and a small chance to lose it all. Sound familiar? I could give you an example using the exact same calculations as above to show you playing like this is a bad idea.

##Coinflip
There really isn't a lot to tell you here. The thing that gets you here is the "tax" that gambling sites collect from the coinflips. With a 50% chance of winning, you might think that every second person that plays on those sites makes a profit. This is wrong because this "tax" causes money to slowly move from the players to the site operators. With less money available to distribute to the players, there will be a lot more losers than winners.

##Betting on matches
There is not a lot of calculating to be done here, if you know how matches will end you can win a lot of money with betting. However, especially in csgo, there are an incredible amount of upsets (underdogs winning), that there is never really a "safe bet".  

Any errors? Anything missing? Please let me know.