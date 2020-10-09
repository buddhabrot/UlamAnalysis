# Ulam Divisor Analysis

This is a small document that collects some findings done during hacking on the Ulam spiral. Although I spent two fun evenings hacking on this at the time of writing, I regret not being able to spend any time or energy on this and hope this document will lead to others taking this up together.

## Definitions

The Ulam spiral is drawn by spiraling out numbers from the inside towards the outside, typically in counter-clockwise fashion. There is not necessarily any official definition, but in my definition, I rotate it 90 degrees compared to the standard literature, to suit the ease of some formulae.

This makes the spiral look like this:

<img src="spiral.png" width="60%">

Note the right-bottom point of each spiral turn: <h3><b>1,9,25,49,...</b></h3>

This right-bottom point is given by the series 

<img src="2nplus1.jpg">

This corner-point is one way to generate the spiral, and yields the following naive formula to convert spiral grid coordinates into spiral numbers. Note that it is perfectly possible to create more standard formulae but this construction is handy to illustrate some of the findings.

```python
def spiral_cell(x,y):
    m = max(abs(x),abs(y)) # square that point belongs to
    p = (2*m+1)*(2*m+1) # right-bottom point of square

    # return the point by retracing from the right-bottom point
    if x == m:
        # right side
        return p - (m + y)
    if y == m:
        # top side
        return p - 2*m - (m - x)
    if x == -m:
        # left side
        return p - 4*m - (m - y)
    if y == -m:
        # bottom side
        return p - 6*m - (m + x)
```

One of the main findings is the appearance of "lines" in the Ulam spiral when <b>prime numbers</b> are marked on it. Some research such as [https://www.researchgate.net/publication/307965102_Finding_Line_Segments_in_the_Ulam_Square_with_the_Hough_Transform] supports this quantitatively.
This analysis will partially demistify this, but will also open up more questions.

## Moving across the Ulam spiral

It is important to establish what movement along the Ulam spiral means, from a formulaic point. 

At each point in the Ulam spiral, you belong to a certain "turn", turns being indexed starting with turn "0" being the center of the spiral. It is clear that for each turn, the right-bottom point of the turn equals 

<img src="2nplus1.jpg">

Now, moving right of left from a number can mean three things:

- Either it means subtracting or adding "1" to the number, depending if you are on the top, bottom, right, or left of the curve.
- If you jump down from the right-bottom point to the next turn, you simply add a number (and reversely, you subtract only one if you retreat to a previous turn from this position)
- Or, it means jumping to a point on the next or previous turn. The change in number will be dependent on the side of the turn you are at. If you are at the bottom of a turn and jump down, to a next turn, you will simply jump with the same amount as the right-bottom side of that turn. For bottom, left, top, right sides this sequence of distances is given by 

<img src="turns_added.jpg">


## Main Findings

From rendering various Ulam spirals, the factors of numbers (the factors of "2" being "2n", the factors of "7" being 7n), form <b>clusters</b> in the spiral that are repeated in the horizontal and vertical direction.

For instance, the pattern for the factors of "7" (not including "7" in the pattern, as explained above),

<img src="pattern_7.png">

Zoomed out, the pattern becomes clear:

<img src="pattern_7_zoomed.png">

Teh pattern for <b>22n</b>

<img src="pattern_22.jpeg">

For <b>30n</b>

<img src="pattern_30.jpeg">

For <b>31n</b>

<img src="pattern_31.jpeg">


## Composite factors and pattern overlays

It is a logical consequence that the factor pattern for a number like "30" will be given by overlaying the pattern of the factors of "2", "3", and "5". Hence, it would seem attractive to only study the factors of non-composite numbers. However, while we will try staying aware of this fact for some of the polynomial constructions we'll be building, we will visually draw all the patterns, since focusing on prime factors alone will hide too much things going.

Note of course, that if the first factor of any series is not included in a pattern (so "7" is not part of the pattern emerging for 7n), the prime numbers will be distributed across the Ulam spiral by overlaying all factor patterns and looking at the leftover positions. Thus, any patterns emerging from the prime number distribution are related to the pattern distribution of regular factors.

## Patterns and polynomials

 The following table illustrates the patterns for more factors, showing how the factors of numbers are aligned in the Ulam spiral - note they are rendered slightly off-center; of course the center point is "1".
Note that for "2n" (not drawn), this simply would create a checkerboard pattern as odd/even numbers alternate on the Ulam grid. This drawing was unclear so I omitted it.

<table>
  <tr>
    <th>
      Factors of ...
    </th>
    <th>
      Pattern
    </th>
  </tr>
  
  <tr>
    <td>
      <h3>3n</h3>
    </td>
    <td>
      <img src="crops/out_1.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  
   <tr>
    <td>
      <h3>4n</h3>
    </td>
    <td>
      <img src="crops/out_2.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  
   <tr>
    <td>
      <h3>5n</h3>
    </td>
    <td>
      <img src="crops/out_3.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  
   <tr>
    <td>
      <h3>6n</h3>
    </td>
    <td>
      <img src="crops/out_4.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  
  
   <tr>
    <td>
      <h3>7n</h3>
    </td>
    <td>
      <img src="crops/out_5.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  
  
   tr>
    <td>
      <h3>8n</h3>
    </td>
    <td>
      <img src="crops/out_6.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  
  
  
   <tr>
    <td>
      <h3>9n</h3>
    </td>
    <td>
      <img src="crops/out_7.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  
  
   <tr>
    <td>
      <h3>10n</h3>
    </td>
    <td>
      <img src="crops/out_8.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  
  <tr>
    <td>
      <h3>11n</h3>
    </td>
    <td>
      <img src="crops/out_9.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  
  <tr>
    <td>
      <h3>12n</h3>
    </td>
    <td>
      <img src="crops/out_10.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  <tr>
    <td>
      <h3>13n</h3>
    </td>
    <td>
      <img src="crops/out_11.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  <tr>
    <td>
      <h3>14n</h3>
    </td>
    <td>
      <img src="crops/out_12.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  <tr>
    <td>
      <h3>15n</h3>
    </td>
    <td>
      <img src="crops/out_13.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  <tr>
    <td>
      <h3>16n</h3>
    </td>
    <td>
      <img src="crops/out_14.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  <tr>
    <td>
      <h3>17n</h3>
    </td>
    <td>
      <img src="crops/out_15.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  <tr>
    <td>
      <h3>18n</h3>
    </td>
    <td>
      <img src="crops/out_16.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  <tr>
    <td>
      <h3>19n</h3>
    </td>
    <td>
      <img src="crops/out_17.png" width="70%" style="padding:-100px">
    </td>
  </tr>
  </table>

#### Polynomial expansion of patterns

For the factors of "8", which show the diagonal pattern above, the explanation of the pattern can be given by the following polynomial:

<img src="pattern8_poly.jpg"/>

Which yields numbers that are divisable by 8 by plugging in n=1,2,...

This pattern then easily emerges by observing that the polynomial above is precisely the same as moving up from the right-bottom coordinate of any ulam spiral turn, and also by offsetting this pattern at any factor of 8 to the top or bottom.




