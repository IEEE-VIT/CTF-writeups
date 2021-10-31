# Question-Name

![date](https://img.shields.io/badge/date-31.10.2021-brightgreen.svg)
<!-- Date you solved the question -->

![category](https://img.shields.io/badge/category-Misc-lightgrey.svg)
<!-- Category of the question -->

![score](https://img.shields.io/badge/score--/500-blue.svg)
<!-- Score of the question / Max Score possible in any question -->

## Description

Electric Lock has some Logic Gates. Unlock it. Image had a circuit and said:
```
Flag: "CTF{" +input that needs to be set, sorted + "}"
E.d.: If only inputs A, B and C must be set, the flag will be CTF{ABC}
```

[Link to Image ZIP](https://storage.googleapis.com/gctf-2021-attachments-project/419bcccb21e0773e1a7db7ddcb4d557c7d19b5a76cd421851d9e20ab451702b252de11e90d14c3992f14bb4c5b330ea5368f8c52eb1e4c8f82f153aea6566d56)

## Summary

Simplified the Logical Expression, and found out the variables that had to be set to 1.

## Detailed solution

- Logical expression from the circuit was:
```
((!(a || !b) && !((!c || d) || (e || !f))) && (!(g || h) && (((h && !i) || (!h && i))) && (i && j)))
```
- Simplified it using this [site](https://www.dcode.fr/boolean-expressions-calculator)
- Got this:
```
! a * b * c * ! d * ! e * f * ! g * ! h * i * j
```
- From the above expression, the variables that needs to be set are BCFIJ

## Flag

```
CTF{BCFIJ}
```