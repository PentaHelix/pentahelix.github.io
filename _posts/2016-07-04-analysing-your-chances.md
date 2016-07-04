---
published: true
layout: post
---

<!--excerpt-->

## Context

For some time now, there has been a lot of controversy surrounding the CS:GO community regarding underage gambling. Recently, this was escalated when youtubers were found to decieve their audiences with "fake" winnings on gambling sites. This inspired me to write up a guide on how to calculate the chances of you winning on various gambling sites/games.

## Disclaimer

This guide will not show you how to win on gambling sites. "Winning" is not something you will be doing while gambling. Sure, there might be a few amazing wins along the way, but in the end, the ones who will be winning are the owners of the gambling site.

### TL;DR

You will lose.

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

At the end of these 4 rounds, even after losing 3 times and only winning once, you are still 2 points in the plus, i.e. winning 2 points. With this strategy, no matter how many times you lose in a row, as long as you keep doubling your bets, you will evenentually win back everything, plus your starting bet. This means if you start with e.g. 8 points, you will eventually win 8 points. But here comes the catch: this strategy assumes that you can keep doubling your bet infinitly. Unfortuantly, noone has an endless amount of money, which means the strategy has a major flaw. Let's chack what the chances of winning anyway are. 

Let's assume you are playing on a roulette gambling site. First, I'll create a list of all the probabilities  we know of.

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

Alright, now for a concrete example. With 100 points in the bank, I decide to play roulette with this strategy, starting with a bid of 10 and doubling every time I lose. I will be betting on red, but betting on red or black doesn't make any difference as they both have the same probability.

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

\\( 0.5333.. \cdot 0.5333.. \cdot 0.5333.. = 0.5333...^3 \approx 0.15 = 15% \\)

This looks even more confusing, but is actually not that difficult. 

\\( {N \choose x} \cdot p^x \cdot (1 - p)^{N - x} \\)