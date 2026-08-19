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

When our sound wave hits the KY-038 sensor, it will return an AC signal at the analog output, due to sound waves being oscillatory. The intensity of the sound is directly proportional to the energy it transfers into the microphone, which we can assume to be directly proportional to the RMS voltage. Hence, we will say that

$\frac{I_L}{I_R} = \frac{V_{RMS_L}}{V_{RMS_R}}$

The voltage measured at the analog output also contains a constant DC signal, which is equivalent to the ambient voltage. The information about our soundwave is carried by the generated AC signal, so we only want to compare the AC component of the voltage, that is the difference between the total measured voltage and the ambient reading 

$V_{AC}$ = $\left| V_{measured} - V_{ambient} \right|$

The RMS voltage will be the square root of the average of squared voltage values, or more formally, if $(V_1,   V_2,   V_3,   ... ,   V_n)$ are our isolated AC components, then the RMS voltage is 

$V_{RMS} = \sqrt{\frac{1}{n}\sum_{i=1}^{n} V_i^2}$

We will take measurements and calculate this for both sensors, and thus calculate the ratio of distances.

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

$a = \frac{vΔt}{1 - k}$, and since now $k < 1$, $a > 0$

In both cases the only thing changing in the expression is the sign. We only want to find the positive values of $a$ and $b$ with a simple formula, so for both cases we can just say


$a = \frac{x}{\left| 1 - k \right|}$

And then $b = ak$


Now that we know all the side lengths of the triangle, we can apply law of cosines for each angle respectively

$AB = \cos^{-1}(\frac{a^2 + b^2 - d^2}{2ab})$


$AD = \cos^{-1}(\frac{a^2 + d^2 - b^2}{2ad})$


$BD = \cos^{-1}(\frac{b^2 + d^2 - a^2}{2bd})$


Our servo is located directly between the sensors, at a point we'll call $S$. The angle we want to calculate to move the servo is $α$, as depicted in the picture.

<img width="538" height="390" alt="kuva" src="https://github.com/user-attachments/assets/857fb71b-f7a0-424b-b414-90a645dc75fd" />



The new angle $α$ is the final angle we want to calculate for our servo, since in the picture below all the way to the left is $0°$ and all the way to the right $180°$


<img width="565" height="395" alt="kuva" src="https://github.com/user-attachments/assets/8454378e-8510-438e-8a9e-181591bda870" />




Let's draw some lines to make a right triangle. Since we already know one of the side lengths and angles, we can figure out the rest with sine and cosine.

<img width="465" height="345" alt="kuva" src="https://github.com/user-attachments/assets/4636d1e5-d68d-4550-a5e8-9121afc20f58" />



Keeping in mind that $S$ splits the line between $R$ and $L$, we know that $SR = SL = \frac{d}{2}$


 <img width="499" height="343" alt="kuva" src="https://github.com/user-attachments/assets/6d772138-a62a-42c4-9c0d-c4d0adc08a24" />

 
From this we can see that
 
 
 $\tan{α} = \frac{a \sin{AD}}{\frac{d}{2} - \cos{AD}}$


 And so
 
 
 $α = \tan^{-1}(\frac{a \sin{AD}}{\frac{d}{2} - \cos{AD}})$


If the sound is coming from the left but $P$ is to the right of $L$, we can derive the same formula by drawing the right angle triangle like so:


<img width="397" height="215" alt="kuva" src="https://github.com/user-attachments/assets/da364698-2742-4d60-bc68-eb52792aeba0" />

If the sound comes from the right, then we can do the same method but with $b$ and $BD$, and then calculating $180° - α$

And now we know which angle to write to the servo.
