# Theory

## Setup

Let's assume our microphones are two points in space, $R$ and $L$, a constant distance of $d$ apart (measured to be 15 cm). Let's also assume that the sound we're measuring is coming from a single point $P$.
Let's draw a triangle between these three points, where the distance between $P$ and $L$ is $a$, and the distance between $P$ and $R$ is $b$.

<img width="693" height="411" alt="kuva" src="https://github.com/user-attachments/assets/99580fe3-df5b-4602-b742-01dd5cab4cc4" />



We will then call the angles of said triangle $AB$, $AD$, $BD$ by each angle's adjacent side lengths:

<img width="692" height="496" alt="kuva" src="https://github.com/user-attachments/assets/182d78e7-d13f-40ff-88e1-04e6a1316c9a" />


## Physics

Sound will propagate as a wave from point $P$ through the air and hit the sensors after traveling a distance of $a$ or $b$ respectively. Sound intensity after traveling a distance $r$ is $I = \frac{P}{A}= \frac{P}{4πr^2}$, so respectively:

$I_R = \frac{P}{4πb^2}$ and $I_L = \frac{P}{4πa^2}$, where $P$ (Power) is constant for both sensors. 

From this we can derive the inverse square law, stating that $\frac{I_L}{I_R} = \frac{b^2}{a^2}$ , and hence

$\frac{b}{a} = \sqrt{\frac{I_L}{I_R}}$ , let's call this ratio $k$, 

$k = \frac{b}{a} = \sqrt{\frac{I_L}{I_R}}$

When our sound wave hits the KY-038 sensor, it will return an AC signal at the analog output, due to sound waves being oscillatory. The intensity of the sound is directly proportional to the energy it transfers into the microphone, which we can assume to be directly proportional to the voltage. Please note that since the voltage is oscillating, there will be readings both higher and lower than the readings from ambient noise. For describing the amount of energy the sound wave carries, it doesn't matter whether the voltage is higher or lower, so we'll be measuring the difference between our measured voltage and the ambient voltage.

Hence $\sqrt{I_L / I_R} = \sqrt{V_L / V_R}$, where $V_L$ and $V_R$ are deviations from ambient readings as absolute values.

CHANGE THIS PART

## Geometry & Trigonometry

By drawing 2 circles around point $P$ so that the radii are $a$ and $b$, we can define a new variable $x$, which is the difference in distance between $a$ and $b$.

<img width="1007" height="554" alt="kuva" src="https://github.com/user-attachments/assets/756a6f77-e214-4ba3-b5f7-75fd5e59c5e3" />


In our example $b > a$, so from the moment sensor L gets triggered, sound has to travel exactly $x$ to reach sensor R. The speed of sound is constant $v = 343\frac{m}{s}$. By checking when each sensor is hit we can measure the difference in arrival times $Δt$, and since

$v = \frac{x}{Δt}$ , 

we can solve for $x$.


Let's assume that $b > a$, and so $b = a + x$. Based on our earlier definition of $k = \frac{b}{a}$, we can then say that

$\frac{a + x}{a} = k$, and modifying the expression:


$a + x = ka$


$a - ka = -x$


$a(1 - k) = -x$


$a = \frac{-x}{1 - k}$

Since $x = vΔt$, we can now calculate $a$

When $b > a$, $k > 1$, and so $a > 0$


Following the same steps when $a > b$, we get

$a = \frac{x}{1 - k}$, and since now $k < 1$, $a > 0$

In both cases the only thing changing in the expression is the sign. We only want to find the positive values of $a$ and $b$ with a simple formula, so for both cases we can just say


$a = \frac{x}{\left| 1 - k \right|}$

And then $b = ak$


Now that we know all the side lengths of the triangle, we can apply law of cosines for each angle respectively (while also converting to degrees)

$AB = \frac{180}{π} \cos^{-1}(\frac{a^2 + b^2 - d^2}{2ab})$


$AD = \frac{180}{π} \cos^{-1}(\frac{a^2 + d^2 - b^2}{2ad})$


$BD = \frac{180}{π} \cos^{-1}(\frac{b^2 + d^2 - a^2}{2bd})$


Our servo is located directly between the sensors, at a point we'll call $S$. The angle we want to calculate to move the servo is $α$, as depicted in the picture.

<img width="826" height="660" alt="kuva" src="https://github.com/user-attachments/assets/2781ce8d-9d49-4479-8014-2db32a15889b" />


Now we have a new length, between $S$ and $P$, which we'll call $l$.
