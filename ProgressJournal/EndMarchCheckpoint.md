# End of March Update

## Bigger Picture
- Deciding on the *outer divertor region* to define the volume average of $\bar{p}_0$ and $\bar{n}_N$ and a synthetic diagnostic (=start with FIR) $\rightarrow$  
- $f(\bar{p}_0, \bar{n}_N)$ $\rightarrow$  
- Using $f(\bar{p}_0, \bar{n}_N)$ part of the kalman filter update step becomes $K(y - f(\bar{p}_0, \bar{n}_N))$ where $y$ are the real sensor measurements and $K$ comes from (among other things) the process noise as defined by the RMSE of the fit on $f(\bar{p}_0, \bar{n}_N)$ $\rightarrow$  
- Validating/testing both Kalman Filter and $f(\bar{p}_0, \bar{n}_N)$ comes down to using experimental data and the FRF system identification of TCV ecahust to update the state (i.e., the "State Model" block).

![Observer_Block_Diagram](JournalImages/Observer_Block_Diagram.png)

## What I've Done

1. Using 55 SOLPS-ITER simulations $\rightarrow$ I averaged $p_0$ and $n_N$ over the outer divertor region:

![Outer_Divertor](JournalImages/p0_odiv_polview.svg)

![55_sims](JournalImages/p0_vs_nZ_data_labels.svg)

2. Identified three synthetic diagnostics (FIR, BOLO, possibly MANTIS via DSS) $\rightarrow$ Made mappings from $\bar{p}_0$ and $\bar{n}_N$ to $\langle n_e\rangle _{l,FIR}$ @R=Central Chord:

![FIR_mid_line_linear](JournalImages/FIR_mid_line_linear.svg)   
`Linear regression model:
    y ~ 1 + x1 + x2` 
|               | Estimate   | SE         |
|---------------|------------|------------|
| (Intercept)   | -0.06237   | 0.031404   |
| x1            | 0.08904    | 0.0084856  |
| x2            | 0.97773    | 0.031223   |  

| Metric                                 | Value            |
|----------------------------------------|------------------|
| Error degrees of freedom               | 52               |
| Root Mean Squared Error                | 0.0551           |
| R-squared                              | 0.952            |


![FIR_mid_line_poly21](JournalImages/FIR_mid_line_poly21.svg)  
`Linear regression model:
    y ~ 1 + x1^2 + x1*x2`
|               | Estimate   | SE        |
|---------------|------------|-----------|
| (Intercept)   | 0.052967   | 0.042103  |
| x1            | -0.048639  | 0.04172   |
| x2            | 0.85663    | 0.042837  |
| x1^2          | 0.0055376  | 0.0080438 |
| x1:x2         | 0.13892    | 0.037184  |  

| Metric                                 | Value            |
|----------------------------------------|------------------|
| Root Mean Squared Error                | 0.0496           |
| R-squared                              | 0.962            |


![FIR_mid_line_poly13](JournalImages/FIR_mid_line_poly13.svg)  
`Linear regression model:
    y ~ 1 + x1*x2 + x2^2 + x1:(x2^2) + x2^3`
|               | Estimate   | SE        |
|---------------|------------|-----------|
| (Intercept)   | 0.26839    | 0.12515   |
| x1            | -0.0088483 | 0.035641  |
| x2            | 0.70608    | 0.48978   |
| x1:x2         | 0.045457   | 0.091242  |
| x2^2          | -0.71913   | 0.59802   |
| x1:x2^2       | 0.091765   | 0.056481  |
| x2^3          | 0.59279    | 0.23138   |  

| Metric                                 | Value            |
|----------------------------------------|------------------|
| Root Mean Squared Error                | 0.0171           |
| R-squared                              | 0.996            |

3. I validated the numbers via physical inuition $\rightarrow$ Attempted to validate via experimental data:

![Validation_Attempt](JournalImages/map_validation_attempt.png)

Used GPR fits based on SOLPS-ITER. Probably doesn't work due to lack of dynamics information (i.e., doesn't take into account time dependence), or not isolating steady-state part of experiment correctly... 

![experiment_ne_lin_avg](JournalImages/Cntrl_FIR_ne_linAvg_exp.svg)  
![solps_ne_lin_avg](JournalImages/Cntrl_FIR_ne_linAvg_synth.svg)

GPR based on SOLPS-ITER critically underestimates $\bar{n}_{N}$ given experimental shots which leads to an order magnitude underestimation of $\langle n_e\rangle _{l,FIR}$ @R=Central Chord $\rightarrow$ Solution is to use transfer functions obtained from TCV (and ask what else might be going wrong):  

![DIFFER_TCV_sysID](JournalImages/TCV_Exhaust_TF.png)





