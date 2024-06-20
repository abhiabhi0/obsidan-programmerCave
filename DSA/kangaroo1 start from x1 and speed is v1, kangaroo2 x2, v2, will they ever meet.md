
Without any idea in mind, I decided to lurk in the _Discussions_ section of the problem, in which I found my answer. If we were to mathematically equate the meeting of the kangaroos, it would more or less look like the following:

`x1 + n*v1 = x2 + n*v2`

Where `n`is defined at the number of steps both kangaroos have jumped.

Using that logic, we can change the equation to find the value of n as follows;

`x1 + n*v1 = x2 + n*v2`

`n*v1 — n*v2 = x2 — x1`

`n(v1-v2) = x2-x1`

`n = (x2-x1)/(v1-v2)`

Since n needs to be a whole number without any remainder, we can write a code that could find if the modulus of `n` is equal to zero. So, `if (x2-x1)%(v1-v2) == 0`, then we can safely print out `YES`.

Of course, from that solution alone we can find another problem; if the value of both speeds is the same, then wouldn’t it return a runtime error (dividing by zero is the cardinal sin)? To combat this, I created another if statement that checks to see whether the value of both speeds is the same or different. Doing so lead me to this code:

```
if v1 == v2:  
	return "NO"
```

![[Pasted image 20240604120711.png]]

