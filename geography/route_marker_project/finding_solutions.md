---
layout: archive
title:  "Route Marker Project"
tagline: "Finding Solutions"
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

<h3 class="archive__subtitle">What Does a Solution Look Like?</h3>
Take the following simplified example:
The USA is made up of 4 states: Pennsylvania, Maryland, New Jersey, and New York.

The available highways in each are:
```python
PA: {1, 2, 3, 4}
MD: {2, 4}
NJ: {3}
NY: {1, 2}
```
We have 3 solutions (no state repeats, no number repeats)
```python
PA 1, MD 4, NJ 3, NY 2
PA 2, MD 4, NJ 3, NY 1
PA 4, ME 2, NJ 3, NY 1
```

<br>
<h3 class="archive__subtitle">The Process of Finding Solutions</h3>
First, I had to figure out all the sub-50 highway numbers in each state. This gives me a dictionary. Assume that states are **A**, **B**, **C**, **D**, and **E** and numbers I'm looking for over go through **5**. An example of would be as follows:
```python
	A: {1, 2, 3}
	B: {2, 3, 5}
	C: {2}
	D: {1, 4}
	E: {2, 4, 5}
```

From there, I can invert the dictionary to instead map the numbers to the states. I now have:
```python
	1: {A, D}
	2: {A, B, C, E}
	3: {A, B}
	4: {D, E}
	5: {B, E}
```
*I did this in my prototype python script so that I could quickly see if every number was found in at least one state.*


From here, I can take the cartesian product and filter out any sets that do not have only unique values or cover the whole group of letters. In the experiment above, the possible results look like:
```python
 1  2  3  4  5
[A, A, A, D, B]
[A, B, A, E, B]
[D, C, B, E, E]
``` 
where each list of letters is produced by choosing one letter from each number. Obviously, the three results above do not work, since they all repeat letters. In our analogy, the first result tells us we need highways 1, 2, and 3 in state A, highway 4 in state D, and highway 5 in state B. Where are states C and E represented??

As it turns out, there are only **2** solutions where we cover every letter without repeating:
```python
 1  2  3  4  5
[A, C, B, D, E]
[D, C, A, E, B]

SOLUTIONS: {1:A, 2:C, 3:B, 4:D, 5:E}, {1:D, 2:C, 3:A, 4:D, 5:B}
```

Seems simple enough to scale our little 5 by 5 analogy up to 50 numbers and 50 states, right? Here's where I ran into my first issue: *the sheer size of the solution space*. 

As a test, I doubled my set size: 10 states, 10 possible numbers. From on average about 2 states available per number, I bumped it up to about 6. For example, 2 of those 10 sets:
```python
State:  Highways:
PA      {1, 4, 5, 6, 8, 10}
TN      {1, 2, 3, 5, 7, 9}
```
From 2 small solutions in my 5-set example, I was now ending up with just under 20,000 solutions filtered out from who knows how many non-working results, and it took over a minute for my solution-finding program to run (which sounds quick but in computing time is an eternity). 

My problem was that I was generating all the possible results of the cartesian product of sets -- duplicates included -- and then checking to see which of them were valid solutions, which was just not viable. However, the cartesian product function I was using is a one-liner, and the filtering requirements come later. Although I tried, I simply could not come up with a way to filter as I calculated solutions.

In order for my prototype program to work efficiently, I need to incorporate the following logic:
1. Immediately scrap the current result and move on if I see a duplicate state (if I see Pennsylvania for both highways 5 and 6, it's an invalid result).
2. Don't fill a highway number with a state when by definition that number *has* to come from a different state (Arizona’s only option is 24, so all solutions will by necessity have Arizona in spot 24. Don't ever put Colorado there!). 

To pull this off, I needed a solution “kill switch” that saved on time and memory by stopping solution construction as soon as it is nonviable. Back to the drawing board!

[The Math of It All](/geography/route_marker_project/math_of_it_all)