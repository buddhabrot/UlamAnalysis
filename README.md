# Ulam spiral Analysis and the idea of Polynomial Generators of factors

This is a small document that collects some ruminations and findings done during hacking on the Ulam spiral. Although I spent two fun evenings hacking on this at the time of writing, I regret not being able to spend any time or energy on this and hope this document will lead to others taking this up together with me. 

## Abstract

I believe interesting findings will emerge by using cluster analysis to define, extract clusters of factor multiples in the Ulam spiral, and generate polynomials that capture a "cluster" completely, clusters which can be repeated to form the full pattern.  

This will bring in an amount of algebraic constraints to these 'generator' polynomials that will hopefully teach us more about them, and about the distributions of the multiples of factors they generate, and the prime numbers that are "left behind" by the set of numbers that can not generated by these polynomials. 

It would then be possible, for a certain size of the Ulam spiral, to define a set of polynomial generators, and define the prime numbers as being all of the numbers that cannot be represented by them. This would lead to a system of equations, and thus would represent the prime numbers by this system, which could possibly expand them into the continuous field in a new set of ideas. This might have implications for the treatment of the Riemann hypothesis.

## Definitions

The Ulam spiral is drawn by spiraling out numbers from the inside towards the outside, typically in clockwise fashion. There is not necessarily any official definition, but in my definition, I rotate it 90 degrees compared to the standard literature, to suit the ease of some formulae.

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

From rendering various Ulam spirals, the multiples of factors ("2n", or "7n"), form <b>clusters</b> in the spiral that are repeated in the horizontal and vertical direction.

For instance, the pattern for the multiples of of "7" (not including "7" in the pattern, as explained above),

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

It is a logical consequence that the pattern for a number like "30" will be given by intersecting the patterns of "2", "3", and "5". Hence, it would seem attractive to only study the factors of non-composite numbers.
However, this approach could hide important emerging structure, and also lead to problems with repeated factors, as for instance the pattern for the number "8" cannot simply be derived from the pattern for "2" (which is a checkerboard pattern), and any repeated factors cannot be converted into the action of composing patterns by intersection.

Note of course, that if the first factor of any series is not included in a pattern (so "7" is not part of the pattern emerging for 7n), the <b>prime numbers</b> will be distributed across the Ulam spiral by overlaying all factor patterns and looking at the leftover positions. Thus, any patterns emerging from the prime number distribution are related to the pattern distribution of regular factors.


## Patterns and polynomials

 The following table illustrates the patterns for more factors, showing how the multiples of factors are aligned in the Ulam spiral - note they are rendered slightly off-center; of course the center point is "1".
Note that for "2n" (not drawn), this simply would create a checkerboard pattern as odd/even numbers alternate on the Ulam grid. This drawing was unclear so I omitted it.

<table>
  <tr>
    <th>
      n
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

## Movement along the Ulam spiral and the resulting patterns for prime numbers

Movement up/down on the Ulam spiral, whether this causes positions to jump to new "turns", or not, always adds some offset that is dependent of the turn size, called the "turn dependent offset", and another offset that is independent of the current turn (as given by the "+0, "+2, "+4" and "+6" offset for each 'side of the turn'), called the "turn independent offset". Now, this leads to the following conclusion:

<h4>If a cluster of numbers is found, that is built from jumps either dependent on the turn size or not, then, there will be another cluster a little further to the right or top, built by jumping with a multiple of the independent offset (a multiple that is divisable by the factor of the pattern).</h4>

Because of this, clusters will always be oriented in a grid-like fashion. This also means the "leave behinds" in the clusters will be with higher probability located in gaps from these patterns which necessarily also need a 0, 45 or 90 degree orientation towards each other.<b>This can partially explain the tendency for the prime numbers to appear along diagonals and vertical/horizontals in the Ulam spiral. </b>

### Polynomial generators of patterns

For the multiples of "8", which show the diagonal pattern above, the explanation of the pattern can be given by the following polynomial:

<img src="pattern8_poly.jpg"/>

Which yields numbers that are divisable by 8 by plugging in n=1,2,...

This pattern then easily is connected to the factors of 8 by observing that the polynomial above is precisely the same as moving up from the right-bottom coordinate of any ulam spiral turn, and also by offsetting this pattern at any factor of 8 to the top or bottom. Hence, this pattern will capture the diagonal lines emerging in the "8" pattern - note however that this pattern is considerably different and more simple than most of the other clusters.

It is our belief each "cluster" of numbers can be represented by a polynomial.

## Further research

See Abstract above.
