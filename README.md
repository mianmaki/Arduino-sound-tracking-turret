# Arduino Sound Tracking Turret

## Overview

A turret that turns a servo motor to point roughly in the same direction as the direction a sound comes from. It utilizes 2 sound sensors and compares the differences in measured sound intensity and sound arrival time.
I was interested in making some kind of turret as a personal project that can take an input (sound, light, heat, etc.) and figure out the direction it comes from. This is a modified version of my face tracking nerf gun turret.

## Photos & Videos

<img width="560" height="745" alt="kuva" src="https://github.com/user-attachments/assets/02f6733f-9799-4e21-969d-b09e8af0a090" />


Demonstration video https://drive.google.com/file/d/1pTEz_3CLvrkCv_ZFWbNey6ct3iofEjy3/view?usp=sharing

## How it works

This is a brief summary of the core principles. For a full technical breakdown, see theory. For complete hardware breakdown, see schematic.png

### Measuring sound intensity

When the KY-038 detects a sufficiently loud sound, the digital output will give a HIGH signal and we can measure the relative intensity of the sound as a voltage from the analog output. We can do this for both sensors and measure slightly different intensities.

### Measuring arrival time difference

Unless the sound source is directly between the sensors, the sound will hit the sensors at different times. By constantly checking the waveform at the analog output for each sensor, we can see when each sensor gets triggered and thus measure the difference in arrival time.

### Pointing servo

By drawing a triangle with the 2 sensors and the assumed point source of the sound, we can calculate the angles of said triangle with trigonometry, the ratio of measured intensities and the arrival time difference. Using this information we can also calculate the angle we want to point the servo towards.

## Challenges

### Sensor sensitivity matching

The sensors had to be tuned very precisely to not trigger from ambient noise, but from louder noises, such as a clap. Since the sensitivity was not suitable out of the box, they had to be tuned by hand by adjusting the potentiometer. Hence, there is a still small difference in sensitivity between sensors. To combat this a correction factor is introduced.

### Measuring arrival time difference

With the sensors being 15 cm apart, arrival times can differ by ~440 μs at most. At this scale, program runtime gets in the way of measurement accuracy, and must hence be optimized.

### Detecting when sound first arrives

In theory, this should be easy to detect by checking when the digital output turns from LOW to HIGH, either constantly reading the digital output or with a hardware interrupt (both were implemented in earlier versions without success). The problem is that, based on observations, the analog waveform has already started rising by the time the digital output turns HIGH, so we can't rely on the digital signal for precise timing. Since the digital signal relies on the reading crossing a threshold value, the sensitivity mismatch also contributes to this problem, since the rising waveforms may have different slopes.

Due to the oscillatory nature of sound, the analog output returns an AC signal. This means that when sound is detected, we measure both lower and higher values compared to ambient conditions. If the hardware only checks for, for example, a low enough value, depending on how the sound's waveform hits the sensor, it may go high before going lower and finally triggering the digital pin to go HIGH, causing a delay.

### Measuring intensity accurately

Due to the oscillating analog output, we can't just reliably measure one peak value. It would theoretically work, but capturing the exact moment it reaches the peak value is not reliable. Instead, we collect multiple samples over a short period of time and analyze the deviations from ambient readings statistically.

### Echoes

The model assumes that all the sound hitting each sensor is only the sound coming directly from the source. In reality, reflections off the walls will skew the measurements. To mitigate this, we need to collect samples from a short enough period of time, so that most reflections won't have reached the sensors yet. However, it's hard to clearly see the exact time when reflections start taking over from a graph.

### 3D Geometry

The model draws a triangle in a 2D plane and uses angles within that plane. If we make a sound from a different height than the turret's, we will be using angles from a 3D plane as if they were the same when reflected onto the plane where the servo turns. This means that the turret only works when the sound source is at the same height as the turret. For small deviations however, the error is definitely not the biggest concern.

### Accuracy

The biggest problem with the turret is its inaccuracy. Due to sensor sensitivity mismatch, difficulty measuring arrival time difference, echoes, difficulty measuring sound intensity ratios and limitations of the KY-038 modules, the turret has trouble with finding the right direction accurately.

## Results

The turret can sometimes find generally the right direction, but with a lot of deviation and some measurements are completely off. Sometimes there is also a measurement error, shown by the red LED lighting up, which is when something in the program goes wrong and nonsensical values are measured.

## What I learned

- Signal processing
- Analyzing data from sensors
- Optimizing program runtime
- Optimizing measurement accuracy
- Hardware interrupts
- Controlling motors with software

## Future improvements

- Taking into account height:
  - Being able to measure accurately even when sound comes from a different height
  - Being able to also figure out which height it comes from; finding the source in 3D space
  - Implementing at least a third sensor and a second servo to accomplish this (more sensors could also help accuracy since you can compare multiple time differences)
- Having better suited microphones for accurate measurements (KY-038 not ideal for this project)
- Processing AC signal with a different method
- Having properly tuned microphones
