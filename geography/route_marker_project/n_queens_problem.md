---
layout: archive
title:  "Route Marker Project"
tagline: "Dynamic Programming: The N-Queens Problem"
author: Jack Still
author_profile: true
header:
 teaser: "assets/images/north_of_sedona.jpg"
 overlay_image: "assets/images/north_of_sedona.jpg"
 # caption: "Photo credit: me"
 description: A picture I took north of Sedona, AZ in August 2022
---
<a href="javascript:window.history.back();">Go Back</a>

[Go to Highways Home](/geography/route_marker_project/highways_home)

<h3 class="archive__subtitle">What is the N-Queens Problem?</h3>
It was in the process of finding ways within python to implement the conditionality question (no number duplicates) that I remembered something I had done in my algorithms class during the spring of 2022: the “N-Queens problem.” The gist of it entails finding a way to place ```n``` number of queens on a chessboard such that no queen can attack another (by being in the same column, row, or diagonal). On a regular chessboard, the goal is 8 queens, but for our class we just talked about 4 (on a 4x4 chessboard). As it turns out, there are only 2 ways to do this:
```python
|-|Q|-|-|
|-|-|-|Q|
|Q|-|-|-|
|-|-|Q|-|
```
```python
|-|-|Q|-|
|Q|-|-|-|
|-|-|-|Q|
|-|Q|-|-|
```

A way that these solutions can be found is through a process called dynamic programming: filling the table row by row and trying to place queens. If there is a violation (a queen in the same column and/or diagonal), there is an attempt to place the queen on the next spot in the row. If a queen can be placed in that cell, we advance to the next row. In the case that the *entire* row is passed over without a suitable spot, the algorithm rewinds and moves the last suitable queen in the previous row to the next space, then once again goes through the process of placing a queen in the next cell and checking until a valid solution is found. In this back-and-forth manner, the board is filled until all solutions have been found.

In the context of my problem, I figured that rows could stand for highway numbers, states could be the columns, and “queens” would signify the chosen state-number pair. Consider the following example containing only four states and four numbers:

```
PA: {1, 2, 3, 4}
MD: {2, 4}
NJ: {3}
NY: {1, 2}
```

<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572> </td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572> </td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>

The green spaces represent the valid spaces that a state/highway combination can be chosen (In this example, MD 2 exists, but not MD 3). The goal is simple. Like the queen problem, we need to place one X in every row (one and only one state-number pair) in such a way that every column is also filled with *exactly* one X. 

We do have one notable change with our constraints. Unlike the queen problem, we have the added constraint of “valid” and “invalid” spaces (whether or not that highway exists). In our favor, though, we do have the ability to place X’s in the same diagonal, since snapping a PA 1 does not impact our ability to get MD 2, NJ 3, and so on.

Let's walk through the process of finding a solution to the above example:

<div style="float: left;margin-right:100px">
Step 1: Clean slate
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572> </td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 2: Bad column!
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td style="font-weight:bold; text-shadow: 1px 1px 1px red;" bgcolor=93C572>X</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572> </td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 3: Valid
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572>X</td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572> </td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 4: Bad column!
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572>X</td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>3</td><td style="font-weight:bold; text-shadow: 1px 1px 1px red;" bgcolor=93C572>X</td><td> </td><td bgcolor=93C572> </td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 5: Bad space!
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572>X</td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td style="font-weight:bold; text-shadow: 1px 1px 1px red;">X</td><td bgcolor=93C572> </td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 6: Valid
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572>X</td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572>X</td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 7: Bad column!
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572>X</td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572>X</td><td> </td>
  </tr>
  <tr>
    <td>4</td><td style="font-weight:bold; text-shadow: 1px 1px 1px red;" bgcolor=93C572>X</td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 8: Bad column!
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572>X</td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572>X</td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td style="font-weight:bold; text-shadow: 1px 1px 1px red;" bgcolor=93C572>X</td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 9: Bad space!
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572>X</td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572>X</td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td style="font-weight:bold; text-shadow: 1px 1px 1px red;">X</td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 10: Bad space!
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572>X</td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572>X</td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td style="font-weight:bold; text-shadow: 1px 1px 1px red;">X</td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 11: Bad column!
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572>X</td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572> </td><td style="font-weight:bold; text-shadow: 1px 1px 1px red;">X</td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 12: Bad column!
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td style="font-weight:bold; text-shadow: 1px 1px 1px red;">X</td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572> </td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 13: Valid.
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572>X</td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572> </td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 14: Bad column!
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572>X</td>
  </tr>
  <tr>
    <td>3</td><td style="font-weight:bold; text-shadow: 1px 1px 1px red;" bgcolor=93C572>X</td><td> </td><td bgcolor=93C572> </td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 15: Bad space!
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572>X</td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td style="font-weight:bold; text-shadow: 1px 1px 1px red;">X</td><td bgcolor=93C572> </td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 16: Valid.
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572>X</td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572>X</td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 17: Bad column!
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572>X</td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572>X</td><td> </td>
  </tr>
  <tr>
    <td>4</td><td style="font-weight:bold; text-shadow: 1px 1px 1px red;" bgcolor=93C572>X</td><td bgcolor=93C572> </td><td> </td><td> </td>
  </tr>
</table>
</div>

<div style="float: left;margin-right:100px">
Step 18: Solution!
<table>
  <tr>
    <th> </th><th>PA</th><th>MD</th><th>NJ</th><th>NY</th>
  </tr>
  <tr>
    <td>1</td><td bgcolor=93C572>X</td><td> </td><td> </td><td bgcolor=93C572> </td>
  </tr>
  <tr>
    <td>2</td><td bgcolor=93C572> </td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572>X</td>
  </tr>
  <tr>
    <td>3</td><td bgcolor=93C572> </td><td> </td><td bgcolor=93C572>X</td><td> </td>
  </tr>
  <tr>
    <td>4</td><td bgcolor=93C572> </td><td bgcolor=93C572>X</td><td> </td><td> </td>
  </tr>
</table>
</div>

<br style="clear:both" />
<br style="clear:both" />
Our final iteration gives us a valid case where we have a single X in every row and every column where the state-number pair exists! Reading our valid solution gives us:  
[PA 1, NY 4, NJ 3, MD 2].

We could now store this solution and continue our search for more by continuing this back-and-forth process of shifting our X's, but the theory has been proven!

*"But that took so many steps! How is it easier than the prior method of generating all combinations and filtering out invalid ones?"*

I'm glad you asked! The great thing about this method is that it throws out invalid solutions *as they're generated*, so in the case where we have millions upon billions upon trillions (and more) combinations of state-number pairs, the depth-first search (DFS) approach and use of backtracking allows us to find all possible solutions without checking *everything*, vastly reducing the time and storage needed as we do so.

Up Next: [The Improved Program](/geography/route_marker_project/the_improved_code)