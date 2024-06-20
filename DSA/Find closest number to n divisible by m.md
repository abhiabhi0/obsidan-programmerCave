
```
closestNumber(n, m)
    Declare q, n1, n2
    q = n / m
    n1 = m * q

    if (n * m) > 0
        n2 = m * (q + 1)
    else
        n2 = m * (q - 1)

    if abs(n-n1) < abs(n-n2)
        return n1
    return n2
```

