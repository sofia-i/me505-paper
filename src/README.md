# Simulation Code

[Return to Project Root](..)

This directory contains a notebook (`.nb`) file with the Mathematica code used to create these simulated visualizations. Below, we will list the Mathematica code directly for convenience.

```mathematica
(* Wave Equation *)

wave[x_, t_] := (1/2) (wave0[x - v*t] + wave0[x + v*t]) + 
    (1/(2*v)) Integrate[wave1[s], {s, x - v*t, x + v*t}]

wavePlot[t_] := 
 Plot[wave[x, t], {x, -5, 5}, PlotStyle -> RGBColor[0.6, 0.8, 1]]

(* Wave Parameters *)
v = 2;
wave0[x_] := 0.5*Sin[x]
wave1[x_] := 0

(* Visualize Wave by Itself *)
Animate[Plot[wave[x, t], {x, -5, 5}, 
  PlotRange -> {{-5, 5}, {-1, 1}}], {t, 0, 5, 0.1}]

(* Cable and Coupling *)

yn[x_, n_, L_] := BesselJ[0, 2*mu[n, L]*Sqrt[x]]
ynnorm[x_, n_, L_] := 
  L*BesselJ[0, 2*mu[n, L]*Sqrt[L]]^2 + 
  L*BesselJ[1, 2*mu[n, L]*Sqrt[L]]^2
mu[n_, L_] := N[(1/2)*Sqrt[(BesselJZero[0, n])^2/L]]

un[t_, n_, L_] := (L*w*Derivative[1, 0, 0][yn][L, n, L])/mu[n, L] *
   Integrate[fl[t - tao]*Sin[w*mu[n, L]*tao], {tao, 0, t}] -
   (w/mu[n, L]) *
   Integrate[Sn[t - tao, n, L]*Sin[w*mu[n, L]*tao], {tao, 0, t}] +
   un0[t, n, L]*Cos[w*mu[n, L]*t] + 
   un1[t, n, L]/(w*mu[n, L])*Sin[w*mu[n, L]*t]
u[x_, t_, L_] := 
 Total[Table[N[un[t, n, L]*yn[x, n, L]/ynnorm[x, n, L]], {n, 1, 20}]]

(* Cable Parameters *)

L = 1; (* Cable Domain Length *)
w = 1; (* Propagation Speed *)
anchor[t_] := 0.3 (* Anchor point on wave (used for bounding) *)
fl[t_] := wave[anchor[t], t] (* Upper spatial bound *)
u0[x_, L_] := L - x - wave[anchor[0], 0] (* Initial shape *)
u1[x_, L_] := 0 (* Initial velocity *)
S[x_, t_] := 0 (* Source function *)

(* Initial Conditions, Source Under Integral Transform *)
Sn[t_, n_, L_] := Integrate[S[xx, t]*yn[xx, n, L], {xx, 0, L}]
un0[t_, n_, L_] := Integrate[u0[xx, L]*yn[xx, n, L], {xx, 0, L}]
un1[t_, n_, L_] := Integrate[u1[xx, L]*yn[xx, n, L], {xx, 0, L}]

(* Precompile u *)
cf = Compile[{{x, _Real}, {t, _Real}}, Evaluate[u[x, t, L1]]]

pcable[t_] := 
 ParametricPlot[{q - 1 + anchor[t], -cf[q, t]}, {q, 0, L1}, 
  PlotRange -> {{-2, 2}, {-2, 1}}, 
  PlotLabel -> StringJoin["t=", ToString@t], AxesLabel -> {u, x}]

anim = Animate[Show[pcable[t], wavePlot[t]], {t, 0, 10, 0.05}]
```