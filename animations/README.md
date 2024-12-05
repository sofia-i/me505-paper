# Animations

[Return to Project Root](..)

This folder contains simulation animations made using Wolfram.

## Animation Details
Here are the details of the parameters which created each animation:

#### `animation0_bounded_horse_hair.gif`
```mathematica
(*Cable settings*)
L = 1
w = 1
anchor[t_] := 0.3
fl[t_] := wave[anchor[t], t]
u0[x_] := L - x - wave[anchor[0], 0]
u1[x_] := 0
S[x_, t_] := 0
```

#### `animation02_three.gif`

```mathematica
L1 = 1;
w1 = 1;
anchor1[t_] := .1
fl1[t_] := wave[anchor1[t], t]
u01[x_, L_] := L - x - wave[anchor1[0], 0]
u11[x_, L_] := 0
S1[x_, t_] := 0

L2 = 1;
w2 = 1;
anchor2[t_] := 1
fl2[t_] := wave[anchor2[t], t]
u02[x_, L_] := L - x - wave[anchor2[0], 0]
u12[x_, L_] := 0
S2[x_, t_] := 0

L3 = 1;
w3 = 1;
anchor3[t_] := -1
fl3[t_] := wave[anchor3[t], t]
u03[x_, L_] := L - x - wave[anchor3[0], 0]
u13[x_, L_] := 0
S3[x_, t_] := 0
```

#### `animation01_six_sources.gif`

```mathematica
L = 1;
w = 1;
anchor[t_] := .3
fl[t_] := 0
u0[x_, L_] := L - x
u1[x_, L_] := 0
S0 = 0.75;
S[x_, t_, L_] := Total[Table[
    S0 * DiracDelta[x - k*L] * wave[k*L, t], 
    {k, {0, 0.2, 0.4, 0.6, 0.8, 0.99}}]]
```
