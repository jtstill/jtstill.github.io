---
layout: archive
title:  "Route Marker Project"
tagline: "The Math of It All"
author: Jack Still
author_profile: true
header:
 teaser: "assets/images/north_of_sedona.jpg"
 overlay_image: "assets/images/north_of_sedona.jpg"
 # caption: "Photo credit: me"
 description: A picture I took north of Sedona, AZ in August 2022
---
<a href="javascript:window.history.back();">Go Back</a>

[Go to Highways Home](/petprojects/route_marker_project/)

<h3 class="archive__subtitle">Thinking Through My Next Steps</h3>
Before I dove head-first into trying to code up another solution-finding program, I at least wanted to know how many solutions were floating out there. Conceptually, I know it’s not a lost cause: I could manually sketch out at least a few solutions. But how to find them programmatically *and* efficiently?

On the upper bound, though -- even with the “doable state highway” constraints (i.e. those which I could conceivably get myself to) -- was I looking at something beyond the scope of my computer power? Could my laptop handle full computation? On the simplistic end, I could find *every* permutation and only print out those with 50 unique states, but would I be watching my computer print solutions line-by-line for the next 50 years? To get a better sense of my solution space, let’s think about the math of it all.

<br>
<h3 class="archive__subtitle">The Math of It All</h3>
In the case where every state contains every numbered highway between 1 and 50 inclusive, the calculation is relatively easy. Let’s say I organize them alphabetically:

I have 50 numbers to choose from for Alabama. After choosing one, I now have 49 possibilities from Alaska. Moving forward, I have 48 possibilities for Arizona. And so on and so forth until I am left with only 1 choice in Wyoming. Mathematically, that equates to:
```
50 * 49 * 48 * … * 2 * 1 
= 50! 
= 3.0414x10^64
```

Quick sidebar: The formula for permutations is ```(n!)/(n-r)!```, where ```n``` is number of choices and ```r``` is number of selections. When we have 50 states (n) and are picking 50 numbers (r), that calculation simplifies to just ```n!```.

Yikes. This is only 2,652 times less than 52!, an incomprehensibly large number and the subject of a very interesting [YouTube video](https://youtu.be/0DSclqnnC2s?si=2WL4STXciM35cV86). So... that’s not very promising.

<br>
<h3 class="archive__subtitle">Cutting Down the Solution Space</h3>
However, as I’ve previously mentioned, none of the states actually have all 50 state highways to choose from. In fact, some of them are fairly limited: Alaska has two I could conceivably get: 1 and 3 (the others are in far flung or remote regions I’ll probably never travel to). Arizona and Nevada only have one each: 24 and 28, respectively. Even the states where I can see myself having a decent amount of choice around home or on vacations (Pennsylvania, Georgia, California) only have maybe a dozen or so that would be worth going out of my way to get. 

So, I can calculate the total number of permutations as simply the product of however many valid highways are in each those 50 states: 
```
1 * 1 * 2 * 10 * 14 * 5 * … 
= something MUCH smaller than = 3.0414x10^64
```

With an arbitrary list of “doable” state highways that I made in 2022, that number looks more like ```10^23```. Considerably smaller, but still *way* too big.

But here's another thing going my way: this number doesn’t consider highway number duplicates and conditions based on my choosing. Some clarification on these two points is needed. Say I’ve set up arbitrary numbers for the 50 states, with these as my first three choices:
```
GA = {2, 6, 12}
FL = {3, 6}
AL = {6, 27}
```

Here I have three choices for Georgia, two for Florida, and two for Alabama (an unknown amount for the other 47 states, but that’s okay for now). By the first method (not taking into account my available choices), I would have ```50 * 49 * 48 = 117,600``` possible ways to make solutions out of the first three states. My second method looks a lot closer to reality, being ```3 * 2 * 2 = 12``` choices for the first 3 states. 

But while this accounts for a smaller availability space, it does *not* account for my other constraint, which is not repeating highway numbers either. For example, if I choose to get GA 6, my choices moving forward are severely reduced. 

And this is where I get beyond my own skill. I don’t how to mathematically apply this to a general set, since the solution space is conditional to the numbers themselves. In the case here, instead of 12 ways to make the first three choices, I actually have only 5 ways:
```
[GA 2, FL 3, AL 6]
[GA 2, FL 6, AL 27]
[GA 6, FL 3, AL 27]
[GA 12, FL 3, AL 6]
[GA 12, FL 6, AL 27]
```

So obviously, having both the constraint that A) the number must exist and B) the numbers mustn’t overlap will significantly reduce my solution space. In just those first three picks, the solution space was reduced drastically by adding in my constraints:
```
Pure Permutation: 50 * 49 * 48 = 117,600
Availability: 3 * 2 * 2 = 12
Number Dependence: 5 (in this example)
```

Expanding to the full 50 states is a balancing act. On one hand, I have hope that the entire solution space isn't be too extreme. On the other, the added constraints necessitate a heavier emphasis on manual computation. I just don’t know how to mathematically calculate the theoretical solution with both limited choice *and* the conditionality of numbers.

A Coding Breakthrough: [The N-Queens Problem](/geography/route_marker_project/n_queens_problem)