# Motion 101

### Vectors
- a direction (angle relative to origin point)
- and magnitud $\ |{\vec{v}}| = \sqrt{x^2+y^2}$

### Position
Position is a point in space.

```java
vec2 pos = vec2(x,y);
```
### Velocity
Use velocity to change the position of a point every render frame or through time.

```
vec2 vel = vec2(x,y);
vec2 new_pos = pos + vel; // new position = position + velocity
```
This means that on each render frame, the point or position will move in a constant manner depending on the velocity vector.

### Acceleration
Acceleration can be considered as the change of the velocity vector through time.

```
vec2 acc = vec2(x,y);
vec2 new_pos = pos + (vel + acc); // new position = position + (velocity + acceleration)
```

Because we are now adding the acceleration vector to the velocity vector on each render frame, the point movement will increase or decrease depending on the magnitud of this acceleration. Movement will no longer be constant unless the velocity remains the same (magnitud of acceleration is equal to 0)

### Composing the motion
new position = current position + (velocity + acceleration)

### Newtons second law

A force ${\vec{F}}$ is a vector that causes an object with mass to accelerate.

$\ {\vec{F}} = M \times {\vec{A}}$

We can also write this as:

$\ {\vec{A}} = {\vec{F} \over M}$

```
float m = 1.0; // mass
vec2 frc = vec2(x,y); // force, wind, gravity, etc
vec2 acc = frc / m; // acceleration
vec2 new_pos = pos + (vel + acc);
```

--- tags ---
##### :motion: :vectors: :acceleration: :velocity: :magnitud: :direction:

