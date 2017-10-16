# Learning Journal

## 09/10/2017

Phil and I were working on creating a circle text effect. I knew that I needed to calculate the distance around the circle equal to the distance from the centre of the text. And then calculate the angle turning about the centre of the circle to reach that distance.

We could see that if the distance happened to be the same as the circumference of the circle, that the angle should therefore be 360 degrees. Equally, if it was half the circumference of the circle, the angle should be 180 degrees. In other words, the ratio of the distance to the circumference of the circle should be the same as the ratio of the angle to 360 degrees:
```
angle / 360 = distance / circumference
```

The circumference we know is `2 * pi * radius`. So we could also write this as:
```
angle / 360 = distance / (2 * pi * radius)
```

Which we can re-arrange by multiplying by 360:
```
angle = (360 * distance) / (2 * pi * radius)
```

And this is where we had a eureka moment. We realised that if we measured the angle in radians, then the 360 would become (2 * pi) and that would cancel.
```
angle = (2 * pi * distance) / (2 * pi * radius)
```
And so...
```
angle = distance / radius
```
Could it be that easy?