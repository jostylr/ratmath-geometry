# Geometry Package Plan

This document outlines the plan for the `geometry` package, providing exact geometric constructions and computations using rational arithmetic, with support for both coordinate-free and coordinate-based approaches.

## Package Structure

```
packages/geometry/
├── src/
│   ├── index.js              # Main exports and registration
│   ├── ratmath-module.js     # Combined VariableManager module
│   ├── primitives.js         # Points, lines, circles, segments, rays
│   ├── constructions.js      # Ruler and compass constructions
│   ├── intersections.js      # Intersection computations
│   ├── transformations.js    # Translations, rotations, reflections, scaling
│   ├── coordinates.js        # Coordinate systems and conversions
│   ├── predicates.js         # Geometric predicates (collinear, concurrent, etc.)
│   └── measures.js           # Distance, angle, area computations
├── help/
│   ├── geometry.txt          # Main package help
│   ├── primitives.txt        # Primitive objects help
│   ├── constructions.txt     # Construction operations help
│   └── transformations.txt   # Transformation help
├── tests/
│   ├── primitives.test.js
│   ├── constructions.test.js
│   ├── intersections.test.js
│   ├── transformations.test.js
│   └── coordinates.test.js
└── package.json
```

---

## Philosophy

The geometry package supports two complementary paradigms:

1. **Coordinate-Free (Synthetic)**: Objects defined by relationships (e.g., "line through A and B", "circle with center O passing through P"). No explicit coordinates until needed.

2. **Coordinate-Based (Analytic)**: Objects with explicit rational coordinates. Enables direct computation and integration with graphics.

Both paradigms interoperate: synthetic objects can be "realized" in a coordinate system, and coordinate objects can be queried for synthetic relationships.

---

## Category 1: Primitive Objects

### Point

| Function | Signature | Description |
|----------|-----------|-------------|
| `Point` | `Point(x, y)` | Create point from coordinates |
| `PointFree` | `PointFree(name)` | Create named free point (no coordinates) |
| `PointOn` | `PointOn(obj, param?)` | Point constrained to lie on object |
| `PointX` | `PointX(P)` | X-coordinate of point |
| `PointY` | `PointY(P)` | Y-coordinate of point |
| `PointCoords` | `PointCoords(P)` | Returns {x, y} pair |

### Line

| Function | Signature | Description |
|----------|-----------|-------------|
| `Line` | `Line(P, Q)` | Line through two points |
| `LineFromEq` | `LineFromEq(a, b, c)` | Line ax + by + c = 0 |
| `LineSlope` | `LineSlope(L)` | Slope of line (or undefined for vertical) |
| `LineIntercept` | `LineIntercept(L)` | Y-intercept |
| `LineEq` | `LineEq(L)` | Returns {a, b, c} for ax + by + c = 0 |
| `LinePerpAt` | `LinePerpAt(L, P)` | Perpendicular to L through P |
| `LineParallel` | `LineParallel(L, P)` | Line parallel to L through P |

### Circle

| Function | Signature | Description |
|----------|-----------|-------------|
| `Circle` | `Circle(center, radius)` | Circle from center and radius |
| `CircleThrough` | `CircleThrough(center, point)` | Circle through point |
| `Circle3` | `Circle3(P, Q, R)` | Circumcircle through three points |
| `CircleCenter` | `CircleCenter(C)` | Center of circle |
| `CircleRadius` | `CircleRadius(C)` | Radius (exact or oracle) |
| `CircleRadiusSq` | `CircleRadiusSq(C)` | Squared radius (always rational) |

### Segment, Ray, Arc

| Function | Signature | Description |
|----------|-----------|-------------|
| `Segment` | `Segment(P, Q)` | Line segment from P to Q |
| `SegmentLen` | `SegmentLen(S)` | Length of segment (oracle for irrationals) |
| `SegmentLenSq` | `SegmentLenSq(S)` | Squared length (rational) |
| `SegmentMid` | `SegmentMid(S)` | Midpoint |
| `Ray` | `Ray(origin, through)` | Ray from origin through point |
| `Arc` | `Arc(C, P, Q, dir?)` | Arc of circle from P to Q |

---

## Category 2: Ruler and Compass Constructions

These are the fundamental operations of classical Euclidean geometry.

### Basic Constructions

| Function | Signature | Description |
|----------|-----------|-------------|
| `Midpoint` | `Midpoint(P, Q)` | Midpoint of segment PQ |
| `PerpBisector` | `PerpBisector(P, Q)` | Perpendicular bisector of PQ |
| `AngleBisector` | `AngleBisector(A, B, C)` | Bisector of angle ABC |
| `Perpendicular` | `Perpendicular(L, P)` | Line perpendicular to L through P |
| `Parallel` | `Parallel(L, P)` | Line parallel to L through P |
| `CopySegment` | `CopySegment(S, P, dir)` | Copy segment length from P in direction |
| `CopyAngle` | `CopyAngle(ang, ray)` | Copy angle onto ray |

### Circle Constructions

| Function | Signature | Description |
|----------|-----------|-------------|
| `Circumcircle` | `Circumcircle(A, B, C)` | Circumscribed circle of triangle |
| `Incircle` | `Incircle(A, B, C)` | Inscribed circle of triangle |
| `Excircle` | `Excircle(A, B, C, vertex)` | Excircle opposite to vertex |
| `TangentLine` | `TangentLine(C, P)` | Tangent to circle at point P on circle |
| `TangentFrom` | `TangentFrom(C, P)` | Tangent lines from external point P |

### Advanced Constructions

| Function | Signature | Description |
|----------|-----------|-------------|
| `RegularPolygon` | `RegularPolygon(n, center, vertex)` | Regular n-gon (constructible n only) |
| `GoldenRatio` | `GoldenRatio(S)` | Golden section of segment |
| `SquareRoot` | `SquareRoot(S)` | Construct segment of length √|S| |
| `HarmonicConj` | `HarmonicConj(A, B, C)` | Harmonic conjugate of C wrt A, B |

---

## Category 3: Intersections

| Function | Signature | Description |
|----------|-----------|-------------|
| `Intersect` | `Intersect(obj1, obj2)` | Generic intersection |
| `LineLineInt` | `LineLineInt(L1, L2)` | Line-line intersection point |
| `LineCircleInt` | `LineCircleInt(L, C)` | Line-circle intersections (0, 1, or 2 points) |
| `CircleCircleInt` | `CircleCircleInt(C1, C2)` | Circle-circle intersections |
| `SegmentInt` | `SegmentInt(S1, S2)` | Segment intersection (may be empty) |
| `RayInt` | `RayInt(R, obj)` | Ray-object intersection |

**Note**: Intersections involving circles may yield irrational coordinates. These are represented as oracles or nested radicals when exact, falling back to interval bounds.

---

## Category 4: Transformations

### Isometries (Distance-Preserving)

| Function | Signature | Description |
|----------|-----------|-------------|
| `Translate` | `Translate(obj, vec)` | Translate by vector |
| `Rotate` | `Rotate(obj, center, angle)` | Rotate about center |
| `Reflect` | `Reflect(obj, axis)` | Reflect across line |
| `RotateQuarter` | `RotateQuarter(obj, center, n)` | Rotate by n×90° (exact) |

### Similarities

| Function | Signature | Description |
|----------|-----------|-------------|
| `Scale` | `Scale(obj, center, factor)` | Scale from center |
| `Dilate` | `Dilate(obj, center, factor)` | Alias for Scale |
| `Spiral` | `Spiral(obj, center, factor, angle)` | Combined scale + rotate |

### Coordinate Transformations

| Function | Signature | Description |
|----------|-----------|-------------|
| `ToCoords` | `ToCoords(obj, coordSys)` | Express object in coordinate system |
| `FromCoords` | `FromCoords(coords, coordSys)` | Create object from coordinates |
| `ChangeCoords` | `ChangeCoords(obj, from, to)` | Transform between coordinate systems |
| `CoordSystem` | `CoordSystem(origin, xAxis, yAxis?)` | Define coordinate system |

---

## Category 5: Predicates (Boolean Tests)

| Function | Signature | Description |
|----------|-----------|-------------|
| `Collinear` | `Collinear(P, Q, R, ...)` | Are points collinear? |
| `Concurrent` | `Concurrent(L1, L2, L3, ...)` | Do lines meet at a point? |
| `Concyclic` | `Concyclic(P, Q, R, S, ...)` | Do points lie on a circle? |
| `OnLine` | `OnLine(P, L)` | Is point on line? |
| `OnCircle` | `OnCircle(P, C)` | Is point on circle? |
| `Inside` | `Inside(P, region)` | Is point inside region? |
| `Parallel` | `Parallel(L1, L2)` | Are lines parallel? |
| `Perpendicular` | `Perpendicular(L1, L2)` | Are lines perpendicular? |
| `Tangent` | `Tangent(C1, C2)` | Are circles tangent? |
| `Congruent` | `Congruent(obj1, obj2)` | Are objects congruent? |
| `Similar` | `Similar(obj1, obj2)` | Are objects similar? |

---

## Category 6: Measures

| Function | Signature | Description |
|----------|-----------|-------------|
| `Distance` | `Distance(P, Q)` | Distance between points |
| `DistanceSq` | `DistanceSq(P, Q)` | Squared distance (always rational) |
| `DistToLine` | `DistToLine(P, L)` | Distance from point to line |
| `Angle` | `Angle(A, B, C)` | Angle ABC (oracle for irrational) |
| `AngleCos` | `AngleCos(A, B, C)` | Cosine of angle (rational) |
| `AngleSin` | `AngleSin(A, B, C)` | Sine of angle (may need oracle) |
| `Area` | `Area(polygon)` | Area of polygon |
| `AreaTriangle` | `AreaTriangle(A, B, C)` | Signed area of triangle |
| `Perimeter` | `Perimeter(polygon)` | Perimeter (sum of side lengths) |
| `ArcLength` | `ArcLength(arc)` | Length of arc (oracle) |

---

## Category 7: Triangles & Polygons

### Triangle Centers

| Function | Signature | Description |
|----------|-----------|-------------|
| `Centroid` | `Centroid(A, B, C)` | Centroid (always rational) |
| `Circumcenter` | `Circumcenter(A, B, C)` | Circumcenter |
| `Incenter` | `Incenter(A, B, C)` | Incenter |
| `Orthocenter` | `Orthocenter(A, B, C)` | Orthocenter |
| `NinePointCenter` | `NinePointCenter(A, B, C)` | Nine-point circle center |
| `Euler Line` | `EulerLine(A, B, C)` | Euler line of triangle |

### Polygon Operations

| Function | Signature | Description |
|----------|-----------|-------------|
| `Polygon` | `Polygon(P1, P2, ..., Pn)` | Create polygon from vertices |
| `PolygonVertices` | `PolygonVertices(poly)` | Get vertices |
| `PolygonEdges` | `PolygonEdges(poly)` | Get edges as segments |
| `ConvexHull` | `ConvexHull(points)` | Convex hull of point set |
| `IsConvex` | `IsConvex(poly)` | Is polygon convex? |

---

## Data Representation

### Decorated Objects

Geometric objects are stored as decorated functions/objects:

```
P := Point(3, 4)
Set(P, "type", "point")
Set(P, "x", 3)
Set(P, "y", 4)

L := Line(P, Q)
Set(L, "type", "line")
Set(L, "p1", P)
Set(L, "p2", Q)
Set(L, "eq", {a, b, c})  # Computed on demand
```

### Coordinate-Free Objects

```
P := PointFree("P")
Set(P, "type", "point")
Set(P, "free", 1)
Set(P, "name", "P")

L := Line(P, Q)  # Line through two free points
Set(L, "synthetic", 1)  # No coordinates yet
```

### Realization

```
CoordSys := CoordSystem(Point(0, 0), Point(1, 0))
Realize(L, CoordSys, {P: Point(1, 2), Q: Point(3, 5)})
# Now L has explicit equation
```

---

## Integration with Graphics

The geometry package produces **abstract geometric objects**. These are consumed by:

- **graphics-core**: Converts to drawable primitives with labels, colors
- **graphics-interactive**: Adds drag handles, dynamic updates for webcalc
- **graphics-text**: Exports to LaTeX TikZ, SVG, terminal ASCII

```
# Example pipeline:
tri := Triangle(A, B, C)
gfx := ToGraphics(tri)          # -> graphics-core object
Set(gfx, "fillColor", "blue")
Render(gfx, "svg")              # -> graphics-text export
```

---

## Implementation Priority

### Phase 1: Core Primitives
- [ ] Point (coordinate and free)
- [ ] Line (two-point and equation forms)
- [ ] Circle (center-radius and three-point)
- [ ] Basic accessors (coordinates, slope, radius, etc.)

### Phase 2: Basic Constructions
- [ ] Midpoint, PerpBisector
- [ ] Perpendicular, Parallel
- [ ] CopySegment

### Phase 3: Intersections
- [ ] Line-Line
- [ ] Line-Circle (with oracle/interval support)
- [ ] Circle-Circle

### Phase 4: Transformations
- [ ] Translate, Reflect
- [ ] Rotate (quarter turns exact, general via oracle)
- [ ] Scale

### Phase 5: Predicates & Measures
- [ ] Collinear, OnLine, OnCircle
- [ ] Distance, DistanceSq
- [ ] Area (shoelace formula)

### Phase 6: Advanced
- [ ] Triangle centers
- [ ] Coordinate system transformations
- [ ] Constructibility checks

---

## Dependencies

- `@ratmath/core`: Integer, Rational, RationalInterval
- `@ratmath/oracles`: Oracle interface for irrational values
- `@ratmath/algebra`: Polynomial root finding (for intersections)
- `@ratmath/arith-funs`: Square roots, basic arithmetic

---

## Open Questions

1. **Oracle vs Exact**: When to use oracles vs nested radical expressions?
   - Proposed: Use oracles by default, provide `exact` flag for symbolic

2. **Constructibility**: Should we enforce ruler-compass constructibility?
   - Proposed: Warn but don't prevent non-constructible operations

3. **Degenerate Cases**: How to handle collinear points for circles, etc.?
   - Proposed: Return special "degenerate" object or error

4. **Orientation**: Consistent orientation for signed areas, angles?
   - Proposed: Counter-clockwise positive, following math convention

5. **Coordinate Precision**: Store as exact rationals or allow intervals?
   - Proposed: Exact rationals for base coordinates, oracles for derived
