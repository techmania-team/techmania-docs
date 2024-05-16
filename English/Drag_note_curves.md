Applies to version: 2.2

TECHMANIA supports 2 types of curves for drag notes: Bézier curve and B-spline. In the pattern editor, you can select a drag note and then change its curve type from the "note details" panel.

# Common terminology

Regardless of curve type, a drag note consists of at least 2 **anchors** (shown as squares in the workspace), and each anchor optionally has a left **control point** and right **control point** (shown as circles in the workspace). Anchors and control points are all points in 2D space.

The curve type determines how TECHMANIA calculates the curve's shape from the anchors and control points.

In the following sections, we assume a curve has `n` anchors, numbered `0` to `n-1`.

# Bézier curve

By default the curve is drawn as a Bézier curve, or more specifically, a segmented cubic Bézier curve.

A Bézier curve with `n` anchors contains `n-1` segments, numbered `0` to `n-2`. Segment `i` starts at anchor `i`, ends at anchor `i+1`, and its shape between these two anchors are influenced by the right control point of anchor `i` and left control point of anchor `i+1`.

If we refer to anchor `i` as `P0`, its right control point as `P1`, anchor `i+1` as `P3`, and its left control point as `P2`, the equation of the curve segment is:

```
P = c0*P0 + c1*P1 + c2*P2 + c3*P3, where
c0 = (1-t)^3
c1 = 3(1-t)^2 * t
c2 = 3(1-t) * t^2
c3 = t^3, where t is a parameter between 0 and 1
```

TECHMANIA picks 50 values of `t` (0.02, 0.04, ..., 1), calculates 50 points and connects them to form the curve segment. All segments concatenated together forms the entire curve.

# B-spline

A B-spline is also segmented, but each segment is controlled by 4 anchors, instead of 2 anchors and 2 control points. TECHMANIA uses B-splines of degree 3.

Since control points are ignored, the pattern editor will hide them when a drag note uses B-spline.

A B-spline with `n` anchors contains `n` segments:

* The first segment is controlled by anchors `0`, `0`, `0`, `1`. This is necessary for the curve to start at the note head.
* The second segment is controlled by anchors `0`, `0`, `1`, `2`.
* The next segments are controlled by anchors `0`, `1`, `2`, `3`, anchors `1`, `2`, `3`, `4`, and so on.
* The last segment is controlled by anchors `n-3`, `n-2`, `n-1`, `n-1`.

Note the lack of a segment controlled by anchors `n-2`, `n-1`, `n-1`, `n-1`. This is a deliberate choice to not let the final anchor have too much control on the curve.

If we refer to the 4 anchors that control a segment as `P0`, `P1`, `P2` and `P3` (some of them may refer to the same point but it doesn't matter), the equation of the curve segment is:

```
P = (c0*P0 + c1*P1 + c2*P2 + c3*P3) / 6, where
c0 = -t^3 + 3t^2 - 3t + 1
c1 = 3t^3 - 6t^2 + 4
c2 = -3t^3 + 3t^2 + 3t + 1
c3 = t^3, where t is a parameter between 0 and 1
```

TECHMANIA picks 10 values of `t` (0.1, 0.2, ..., 1), calculates 10 points and connects them to form the curve segment. All segments concatenated together forms the entire curve.

Note that even when `t` takes the extreme values of `0` or `1`, no point on the curve is entirely determined by one anchor (except for the first point in the first segment because `P0`, `P1` and `P2` are the same point). This is what causes B-splines to be smoother but at the same time less controllable than Bézier curves, since most anchors influence 4 segments instead of 2.

# Behind the equation: Bézier curve

A cubic Bézier curve is simply a linear interpolation of linear interpoloations of linear interpolations of the input points. If we define `lerp(A, B, t)` as `(1-t)*A + t*B`, then a cubic Bézier curve is defined as:

```
Q0 = lerp(P0, P1, t)
Q1 = lerp(P1, P2, t)
Q2 = lerp(P2, P3, t)

R0 = lerp(Q0, Q1, t)
R1 = lerp(Q1, Q2, t)

P = lerp(R0, R1, t)
```

Expand these equations to arrive at the ones in the Bézier curve section.

# Behind the equation: B-spline

If you are still here get ready for some real maths.

The equations in the B-spline section are derived using the [De Boor's algorithm](https://en.wikipedia.org/wiki/De_Boor%27s_algorithm). In TECHMANIA's flavor of B-splines:

* The input points are anchor `0` repeated 3 times, then anchors `1` through `n-2`, then anchor `n-1` repeated 2 times, for a total of `n+3` points. We refer to the input points as `P(0)` through `P(s)`, where `s=n+2`.
* `d=3`
* The knot sequence is `{t(0), t(1), ..., t(N)}`, where `t(i)=i` and `N = s+d+1 = s+4`.
* The curve is defined on `t(d) <= t < t(N-d)`, or `3 <= t < N-3`.

For any value of `t`, we first find an integer `J` so that `t(J) <= t < t(J+1)`. Since `t(J)=J`, we simply floor `t` down to the nearest integer to get `J`.

We now apply De Boor's algorithm to calculate the point on the B-spline corresponding to `t`.

**Step 0**: Set `P(i)[0] = P(i)` for all values of `i`.

**Step 1**:

```
P(i)[1] = [(t - t(i)) / (t(i+3) - t(i))] * P(i)[0] + [(t(i+3) - t) / (t(i+3) - t(i))] * P(i-1)[0]
        = (t - i)P(i)[0] / 3 + (i + 3 - t)P(i-1)[0] / 3
```

**Step 2**:

```
P(i)[2] = [(t - t(i)) / (t(i+2) - t(i))] * P(i)[1] + [(t(i+2) - t) / (t(i+2) - t(i))] * P(i-1)[1]
        = (t - i)P(i)[1] / 2 + (i + 2 - t)P(i-1)[1] / 2
```

**Step 3**:

```
P(i)[3] = [(t - t(i)) / (t(i+1) - t(i))] * P(i)[2] + [(t(i+1) - t) / (t(i+1) - t(i))] * P(i-1)[2]
        = (t - i)P(i)[2] + (i + 1 - t)P(i-1)[2]
```

**Step 4**: The point on curve `B(t)` is equal to `P(J)[3]`, so:

`B(t) = (t - J)P(J)[2] + (J + 1 - t)P(J-1)[2]`

Note the repeated occurance of `t-J`. To simplify this equation, we define `x=t-J` so `x` is a periodic function of `t` and its value is always between 0 and 1. We then get:

```
B(t) = xP(J)[2] + (1-x)P(J-1)[2]

P(J)[2] = (t-J)P(J)[1] / 2 + (J+2-t)P(J-1)[1] / 2
        = xP(J)[1] / 2 + (2-x)P(J-1)[1] / 2
P(J-1)[2] = (t-(J-1))P(J-1)[1] / 2 + (J-1+2-t)P(J-2)[1] / 2
          = (x+1)P(J-1)[1] / 2 + (1-x)P(J-2)[1] / 2

P(J)[1] = (t-J)P(J)[0] / 3 + (J+3-t)P(J-1)[0] / 3
        = xP(J)[0] / 3 + (3-x)P(J-1)[0] / 3
P(J-1)[1] = (t-(J-1))P(J-1)[0] / 3 + (J-1+3-t)P(J-2)[0] / 3
          = (x+1)P(J-1)[0] / 3 + (2-x)P(J-2)[0] / 3
P(J-2)[1] = (t-(J-2))P(J-2)[0] / 3 + (J-2+3-t)P(J-3)[0] / 3
          = (x+2)P(J-2)[0] / 3 + (1-x)P(J-3)[0] / 3

P(J)[0] = P(J)
P(J-1)[0] = P(J-1)
P(J-2)[0] = P(J-2)
P(J-3)[0] = P(J-3)
```

By substituting everything into `B(t)` (prepare a lot of paper if you want to reproduce this step) we finally get to:

```
B(t) = (c0*P(J) + c1*P(J-1) + c2*P(J-2) * c3*P(J-3)) / 6, where
c0 = x^3
c1 = -3x^3 + 3x^2 + 3x + 1
c2 = 3x^3 - 6x^2 + 4
c3 = -x^3 + 3x^2 - 3x + 1
```

Notice that the right side of this equation contains `J` and `x` but not `t`. This means for each segment of `t`, we get a different value of `J`, therefore a different set of input points, but the coefficients on these points are periodic, i.e. identical for each segment. This is why in the B-spline section we can give the same definition for each segment.
