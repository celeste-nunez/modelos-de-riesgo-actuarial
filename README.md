# Modelos-de-Riesgo-Actuarial
Distribución de siniestros agregados (De Pril, Panjer y aproximaciones Normal, Gamma trasladada y Edgeworth), principios de cálculo de primas, reaseguro y teoría de la ruina en Python.

Cálculo de la distribución de los siniestros agregados de una cartera de seguros y su uso para fijar primas y evaluar reaseguro. Implementado en Python.

## Contenido

| Notebook | Tema |
|---|---|
| `01_distribucion_siniestros_agregados.ipynb` | Modelo individual y colectivo: De Pril, Panjer y aproximaciones |
| `02_primas_y_reaseguro.ipynb` | Principios de cálculo de primas y reaseguro de exceso de pérdida |

## Notebook 1. Distribución de los siniestros agregados

### Modelo individual: fórmula de De Pril

Cartera de 21 pólizas con montos de 2, 3, 4 y 5 y probabilidades de reclamación de 0.04, 0.05 y 0.06. Se calcula la distribución exacta de S con la fórmula de De Pril y el valor más pequeño de p tal que P(S > p) ≤ 0.05.

| E[S] | Var(S) | p |
|---:|---:|---:|
| 4.35 | 17.5635 | 12 |

### Percentil 95 de S: simulación vs aproximaciones

S es Poisson compuesta con λ = 50 y montos X ~ U(0, 10). Se busca p tal que P(S > p) = 0.05.

| Método | Percentil 95 |
|---|---:|
| Simulación (10,000 valores) | 320.20 |
| Normal | 317.15 |
| Gamma | 320.72 |
| Gamma trasladada | 319.21 |

### Modelo colectivo: fórmula de Panjer y aproximaciones

S es Poisson compuesta con λ = 10 y montos con f_X(x) = x/10 para x = 1, 2, 3, 4. Se calcula la distribución exacta de S con la fórmula de Panjer y se compara con las aproximaciones Normal, Gamma trasladada y Edgeworth.

| E[S] | Var(S) | α3 | α4 |
|---:|---:|---:|---:|
| 30 | 100 | 0.354 | 0.13 |

### Suma de dos Poisson compuestas independientes

S = S1 + S2, con S1 Poisson compuesta (λ1 = 10, montos 1, 2, 3, 10 equiprobables) y S2 Poisson compuesta (λ2 = 20, f_X(x) = x/10 para x = 1, 2, 3, 4). La distribución de cada una se obtiene con Panjer y la de S por convolución, y se compara con las aproximaciones Normal y Gamma trasladada.

| E[S] | Var(S) | α3 |
|---:|---:|---:|
| 100 | 485 | 0.3088 |

## Notebook 2. Primas y reaseguro

### Principios de cálculo de primas

N ~ Binomial Negativa(r = 10, p = 0.3) y montos geométricos con P(Y = y) = 0.6^(y−1) · 0.4. Se calcula la distribución de S con la fórmula de Panjer, su VaR al 95% y, para una prima de 103, el parámetro de dos principios de cálculo de primas:

- **Principio exponencial:** `p(α) = (1/α) · ln M_S(α)`
- **Principio de riesgo ajustado:** `p(ρ) = ∫ [1 − F_S(x)]^(1/ρ) dx`

| E[S] | VaR 95% | α (exponencial) | ρ (riesgo ajustado) |
|---:|---:|---:|---:|
| 58.33 | 102 | 0.0816 | 3.6195 |

### Reaseguro de exceso de pérdida

N ~ Binomial Negativa(r = 100, p = 0.5) y montos X ~ Exp(1/3), con E[S] = 300 y Var(S) = 2,700. Con un límite M = 200, la aseguradora retiene S^A = min(S, M). Se calculan los momentos de S^A con las aproximaciones Normal y Gamma trasladada.

| Aproximación | E[S^A] | σ(S^A) | P(S > M) |
|---|---:|---:|---:|
| Normal | 199.46 | 4.37 | 0.9729 |
| Gamma trasladada | 199.72 | 2.73 | 0.9810 |

## Notebook 3. Teoría de la ruina

### Probabilidad de ruina en tiempo discreto

Capital U_n = u + n − (S_1 + … + S_n), con prima de 1 por periodo y ruina cuando U_n ≤ 0. Siniestros agregados S = 0 con probabilidad 2/3 y S = 2 con probabilidad 1/3. Se calculan ψ(u), el coeficiente de ajuste, la cota de Lundberg y la severidad de la ruina φ(u, z).

| ψ(0) | ψ(10) | R |
|---:|---:|---:|
| 0.6667 | 0.000977 | 0.6931 |

Para u ≥ 1 la probabilidad de ruina coincide exactamente con la cota de Lundberg: ψ(u) = (1/2)^u.

### Probabilidad de ruina con siniestros binomial negativa

Mismo modelo, con f_S(0) = 0.8 + 0.2·g(0) y f_S(x) = 0.2·g(x), donde g es binomial negativa con r = 5 y q = 0.62. Se calculan ψ(u), la probabilidad de ruina en tiempo finito ψ(u, n), el coeficiente de ajuste y la severidad de la ruina.

| ψ(0) | ψ(10) | ψ(25) | R |
|---:|---:|---:|---:|
| 0.6129 | 0.0848 | 0.0036 | 0.2107 |

### Simulación Monte Carlo del modelo de Cramér-Lundberg

Prima c = 5, llegadas Poisson con λ = 2 y montos Gamma(α = 3, β = 4), con 100,000 trayectorias.

| Horizonte | u = 1 | u = 5 | u = 10 |
|---|---:|---:|---:|
| t ≤ 10 | 0.0739 | 0.00007 | 0.0 |
| t ≤ 1 | 0.0727 | 0.00007 | 0.0 |

## Cómo usarlo

1. Clona el repositorio.
2. Instala las dependencias: `pip install numpy scipy matplotlib`
3. Abre los notebooks en Jupyter y ejecuta las celdas en orden.

## Herramientas

Python (NumPy, SciPy, Matplotlib).

## Autoría

Celeste Núñez López, con la colaboración de Ana Ximena Bravo Colin.
