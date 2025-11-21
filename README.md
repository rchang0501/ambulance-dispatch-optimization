# Ambulance Dispatch Optimization  

---

## Introduction

$$
\begin{aligned}
\textbf{Objective:}\quad 
\min_{x^{(t)}}\quad 
    & \sum_{a \in A} \sum_{i \in R_t} 
        \frac{1}{w_i} 
        \Big( d(\text{pos}_a^{(t)}, i) 
            + d(i, h^*(i)) \Big) 
        x_{ai}^{(t)} \\[8pt]
\textbf{subject\ to:}\quad 
    & \sum_{i \in R_t} x_{ai}^{(t)} \le 1, 
        && \forall a \in A \\[4pt]
    & \sum_{a \in A} x_{ai}^{(t)} \le 1, 
        && \forall i \in R_t \\[4pt]
    & \sum_{a \in A} \sum_{i \in R_t} x_{ai}^{(t)} = k_t \\[4pt]
    & x_{ai}^{(t)} \in \{0,1\}, 
        && \forall a \in A,\, i \in R_t.
\end{aligned}
$$

The first constraint ensures each ambulance serves at most one incident, the second ensures each incident is served at most once, and the third ensures exactly \(k_t\) assignments per round.

---

### **Where**
- \(A\): set of available ambulances, indexed by \(a\)  
- \(R_t\): set of unserved incidents at round \(t\)  
- \(H\): set of hospitals (fixed locations), indexed by \(h\)  
- \(\text{pos}_a^{(t)}\): current position of ambulance \(a\) at round \(t\)  
- \(d(p, q)\): Google Distance Matrix API distance between coordinates \(p\) and \(q\)  
- \(h^*(i)\): nearest hospital to incident \(i\)  
- \(w_i \in \{1,2,4,8,16\}\): priority weight for incident \(i\) (larger = higher priority)  
- \(k_t = \min(|A|, |R_t|)\): number of assignments in round \(t\)  

\[
x_{ai}^{(t)} =
    \begin{cases}
        1 & \text{if ambulance } a \text{ is assigned to incident } i \text{ at round } t, \\
        0 & \text{otherwise}
    \end{cases}
\]

---

## Interpretation

The optimization seeks to minimize the **total weighted travel cost** of all ambulance–incident–hospital assignments during round \(t\).

Each term in the objective contains

$$
d(\text{pos}_a^{(t)}, i) + d(i, h^*(i)),
$$

which is the distance an ambulance travels to reach incident \(i\) and then transport the patient to the nearest hospital.

The factor \(1/w_i\) increases the influence of **high-priority incidents** by reducing their effective cost, ensuring they are favored in the assignment solution.

## Simulation
![ambulance simulation](coord_weighted_ambulance_hospital.gif)