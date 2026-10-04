# Motion Planning Q1, Part C: Planner Design Under Competing Constraints

Two collision-free paths connect the same start and goal. Path 1 is shorter but has higher curvature and passes closer to obstacles. Path 2 is longer, smoother and keeps more clearance. This is the cost function that lets the buggy choose between them.

Cost function.** For a candidate path with points i = 0 to N, let Δs\_i be the spacing, δ\_i the steering angle, κ\_i = tan(δ\_i) / L the curvature (wheelbase L = 2.30 m) and d\_i the distance to the nearest obstacle. The planner picks the collision-free path with the lowest cost:

```latex
J = w_L \sum_i \Delta s_i + w_\kappa \sum_i \kappa_i^{2}\,\Delta s_i + w_c \sum_i \phi(d_i)\,\Delta s_i + w_\Delta \sum_i (\delta_{i+1}-\delta_i)^{2}
```

with φ(d) = (1/d − 1/d0)² for d < d0 and 0 otherwise (d0 is the distance beyond which an obstacle no longer matters). Three things are hard constraints, not traded off: the path is collision-free, |κ\_i| ≤ 1/3.68 m⁻¹ (the minimum turning radius from Part A), and the lateral acceleration v²|κ\_i| stays below the comfort and rollover limit. Each term is divided by a reference value (for example the straight-line length and the largest allowed curvature squared) so the weights are dimensionless and comparable.

Path 2 is chosen when what it saves in curvature, clearance and steering-change cost is larger than w\_L times its extra length.

| Term | What it does | Too heavily weighted |
| --- | --- | --- |
| Path length (w\_L) | prefers short, quick paths | cuts corners, hugs obstacles, takes turns near the steering limit |
| Curvature (w\_κ) | prefers gentle curves and low lateral acceleration | long wide detours, may leave the lane, or finds no path on a narrow road |
| Obstacle clearance (w\_c) | keeps distance from obstacles | swerves for small obstacles, drifts into the other lane, refuses narrow gaps |
| Steering change (w\_Δ) | smooth steering, less jerk | cannot follow a bend fast enough (tracking error); too small gives zigzag steering |

**5 km/h versus 20 km/h.** Lateral acceleration is v²κ. Using the tightest turn the buggy can make (κ = 1/3.68 m⁻¹) and an assumed comfort limit of 2 m/s² for unrestrained passengers:

| Speed | v (m/s) | Lateral acceleration at the tightest turn | Largest curvature for 2 m/s² | Stopping distance (τ = 0.15 s, a = 0.8 m/s²) |
| --- | --- | --- | --- | --- |
| 5 km/h | 1.39 | 0.52 m/s² | 0.27 m⁻¹ (the steering limit binds, R = 3.68 m) | 1.4 m |
| 20 km/h | 5.56 | 8.4 m/s² | 0.065 m⁻¹ (R of at least 15.4 m) | 20.1 m |

Rollover starts at about g·T/(2h) = 9.81 × 1.10 / (2 × 0.62) = 8.7 m/s² (track T = 1.10 m, laden CG height h = 0.62 m). At 20 km/h the tightest turn (8.4 m/s²) is close to that, so the higher-curvature Path 1 is unacceptable and the weights should favour Path 2: raise w\_κ with v² and make d0 grow with speed (for example d0 = minimum clearance plus the stopping distance). At 5 km/h even the tightest turn gives only 0.52 m/s² and the stopping distance is 1.4 m, so curvature and clearance weigh less, w\_L can be larger, and the shorter Path 1 can win as long as its clearance stays above the minimum.
