# Motion 101

### Vectors
They have a direction and a magnitud. Think of them as arrows $\vec{v}$. A normalized vector $\hat{v}$ is a vector with magnitud of one.

#### Direction of a vector
- angle relative to origin point

#### Magnitud of a vector
- and magnitud $|{\vec{v}}| = \sqrt{x^2+y^2}$

### Position
Position is a point in space.

```glsl
vec2 pos = vec2(x,y);
```
### Velocity
Use velocity to change the position of a point every render frame or through time.

```glsl
vec2 vel = vec2(x,y);
vec2 new_pos = pos + vel; // new position = position + velocity
```
This means that on each render frame, the point or position will move in a constant manner depending on the velocity vector.

### Acceleration
Acceleration can be considered as the change of the velocity vector through time.

```glsl
vec2 acc = vec2(x,y);
vec2 new_pos = pos + (vel + acc); // new position = position + (velocity + acceleration)
```

Because we are now adding the acceleration vector to the velocity vector on each render frame, the point movement will increase or decrease depending on the magnitud of this acceleration. Movement will no longer be constant unless the velocity remains the same (magnitud of acceleration is equal to 0)

### Composing the motion
new position = current position + (velocity + acceleration)

#### Direction between two points
If we have two points in space, lets say a vertex position $P(p_x,p_y)$ and an attractor position $A(a_x,a_y)$. And we want to get the direction between these two points. What we are actually saying is calculate the vector from point $P$ to point $A$. This is the same as calculating the vector $\vec{v}$ and getting its $vx$ and $vy$ components. To do this, we can use those two points and think of each of them as vectors from the origin. Now if we substract the attractor vector and the position vector we get the vector $\vec{v}$.

- $\vec{v} = (vx, vy)$
- $\vec{v} = \vec{attractor} - \vec{position}$
> the direction from point $A$ to $P$ is vector $\vec{v}$ and is also equal to the attractor vector minus the position vector.
Newton's second law

A force ${\vec{F}}$ is a vector that causes an object with mass to accelerate.
This force could be the wind, gravity or any force you would like to apply to the position of point in space.

${\vec{F}} = M \times {\vec{A}}$

We can also write this as:

${\vec{A}} = {\vec{F} \over M}$

```glsl
float m = 1.0; // mass
vec2 frc = vec2(x,y); // force, wind, gravity, etc
vec2 acc = frc / m; // acceleration
vec2 new_pos = pos + (vel + acc);
```

### Newton's law of universal gravitation
Say we have two objects or points in space, one is the attractor object $A(a_x,a_y)$ and the other is a vertex position $P(p_x,p_y)$. This vertex will experience a gravitational force $\vec{F}$ from the attractor, thus pulling it towards the attractor object.

The force can be calculated with the Newton's law of universal gravitation:

$\vec{F}={m_1 m_2 \over d^2}G$

- $\vec{F}$ = The force
- ${m_1 m_2}$ = The masses of both objects
- $G$ = The universal gravitational constant
- $d^2$ = The distance between the center of the objects

--- tags ---
##### :motion: :vectors: :acceleration: :velocity: :magnitud: :direction:

