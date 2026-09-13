# Metodología Balanis para antenas CP de parche monofeed

## Referencia

Balanis, C. A. (2016). *Antenna Theory: Analysis and Design*, 4ª ed., Capítulo 14: Microstrip Antennas.

## Resumen ejecutivo

El método Balanis permite diseñar antenas de parche **circularmente polarizadas (CP)** mediante:
1. Perturbación elíptica del parche cuadrado
2. Alimentación monofeed en diagonal 45°
3. Síntesis basada en factor de calidad (Q_t)

Ventaja: **Una sola alimentación**, sin acopladores híbridos o ramas múltiples.

## Fundamento teórico

### 1. Modos degenerados en parche cuadrado

Un parche cuadrado resonante en su dimensión L×L genera dos modos ortogonales degenerados:
- Modo TM₁₀ en dirección x (horizontal)
- Modo TM₁₀ en dirección y (vertical)
- Ambos resuenan a la **misma frecuencia** (degeneración)

**Frecuencia de resonancia:**
$$f_r = \frac{c}{2 \sqrt{\varepsilon_r}} \cdot \frac{1}{L}$$

### 2. Perturbación elíptica

Para romper la degeneración y generar CP:

Se transforma el parche cuadrado en **elipse** con semi-ejes a > b:
- Modo en dirección a (mayor) → resonancia a f₁ (más baja)
- Modo en dirección b (menor) → resonancia a f₂ (más alta)
- En la frecuencia central, ambos modos interfieren con **90° de desfase** → **Polarización circular**

### 3. Aspecto de elipse (ellipticity)

La relación entre semi-ejes determina el desfase:

$$\frac{a}{b} = 1 + \frac{1}{Q_t}$$

Donde **Q_t** es el factor de calidad, medido desde el ancho de banda:

$$Q_t = \frac{f_r}{\text{ARBW}}$$

**ARBW** = ancho de banda a 3 dB del axial ratio (AR).

### 4. Alimentación en diagonal 45°

Colocar la sonda de alimentación en la diagonal del parche (ángulo 45°) permite:
- Excitar **ambos modos ortogonales por igual** (sin preferencia x o y)
- Mantener impedancia de entrada cercana a 50 Ω
- Generar desfase natural de ~90° entre modos

Posición típica:
$$(x_f, y_f) = (\pm d, \pm d)$$

Donde d se ajusta para optimizar impedancia de entrada.

### 5. Centrado de AR

Una vez definidos a/b por Q_t, se **escalan ambos ejes simétricamente** (manteniendo a/b constante) para:
- Mover la resonancia de AR al objetivo (868 MHz)
- Optimizar S₁₁ sin cambiar a/b

**Fórmula de escala:**
$$a_{\text{new}} = a \cdot \lambda_s, \quad b_{\text{new}} = b \cdot \lambda_s$$

donde λ_s es el factor de escala (típicamente 0.95–1.05).

## Pasos de síntesis (nuestro proyecto)

### Paso 1: Dimensión inicial del parche

Partir de parche cuadrado resonante:
$$L = \frac{c}{2f_r\sqrt{\varepsilon_r}} \approx 80 \text{ mm (a 868 MHz)}$$

Pero con sustrato RO4360G2 (ε_r ≈ 6.15) y fringing, la dimensión real es menor:
$$L \approx 41 \text{ mm}$$

### Paso 2: Calcular Q_t desde ARBW objetivo

Objetivo: **ARBW ≥ 2 MHz** (ancho de banda a 3 dB del AR)

De simulaciones previas (parche cuadrado):
$$\text{ARBW}_{\text{sq}} \approx 2.5 \text{ MHz} \Rightarrow Q_t = \frac{868}{2.5} \approx 347$$

(Este valor inicial es bajo, pero se refina iterativamente.)

### Paso 3: Calcular aspecto elíptico

$$\frac{a}{b} = 1 + \frac{1}{347} \approx 1.0029$$

**Interpretación:** Elipse muy cerrada al cuadrado (diferencia ~0.3%).

**En nuestro caso**, después de varias iteraciones simulación-síntesis:

Converger a **a/b ≈ 1.0087** para lograr:
- AR mín. = 1.53 dB
- ARBW = 2.17 MHz
- Posición feed = (±7.637 mm, ±7.637 mm)

### Paso 4: Optimización iterativa

En CST o HFSS:

1. **Variar a y b** manteniendo a/b = 1.0087
2. **Variar posición de feed** (x_f, y_f)
3. **Monitorizar:**
   - S₁₁ (objetivo < −10 dB)
   - AR mín. (objetivo < 3 dB)
   - Frecuencia de resonancia AR (objetivo 868 ± 5 MHz)
4. **Iterar** hasta convergencia

### Paso 5: Validación teórica

Comparar con literatura:
- ✅ Khotso et al. (2011): Parche CP circular con perturbación
- ✅ Pozar (2012): Aproximación a elemento de parche
- ✅ Balanis (2016): Capítulo 14, ejemplos trabajados

## Artefactos y trampas

### 🚨 ARBW artefacto de interpolación

**Problema:** Si el monitor de farfield tiene pocas muestras de frecuencia (ej: 50 puntos en banda 868±50 MHz), la interpolación de AR vs. frecuencia es imprecisa → **ARBW simulado puede ser engañoso**.

**Solución:** Usar **500 puntos de frecuencia en banda estrecha** (868±30 MHz) para resolución coherente.

### 🚨 S₁₁ vs. AR son antagónicos

**Observación:** En monofeed CP, S₁₁ y AR mín. tienen máximos diferentes:
- S₁₁ mín. @ 867.8 MHz (típicamente)
- AR mín. @ 868.5 MHz (típicamente)
- No se pueden optimizar ambos simultáneamente

**Solución:** **Priorizar AR** sobre S₁₁ (S₁₁ = −12 dB es suficiente).

### 🚨 Perturbación vs. asimetría

**Terminología:** Balanis usa "asymmetry" (asimetría), pero la literatura moderna CP-patch usa "perturbation" (perturbación). Son equivalentes.

**Cita correcta:**
> "Balanis (Capítulo 14) describe elliptical patches as a method for generating CP via asymmetry..."

**No citar como:** "Balanis usa el término perturbation..." (Balanis no lo usa literalmente).

### 🚨 Rango de frecuencia simulación

**Problema:** Un rango de frecuencia muy estrecho en solvedor Time Domain → excitación muy larga → simulación lenta.

**Solución:** Usar rango moderadamente ancho: **0.75–0.98 GHz** (no 868±5 MHz).

Esto acelera la simulación >10× sin perder precisión en 868 MHz.

## Ejemplo numérico (nuestro diseño)

### Entrada

- Objetivo: 868 MHz, RHCP, monofeed
- Sustrato: RO4360G2 (ε_r = 6.15, tan δ = 0.0038, h = 1.524 mm)
- Plano de masa: 95×95 mm

### Síntesis

1. **Parche inicial (cuadrado):** L ≈ 41 mm
2. **Q_t estimado:** 118 (de iteraciones tempranas)
3. **a/b calculado:** 1 + 1/118 = 1.0085
4. **Semi-ejes iniciales:** a = 39.5 mm, b = 39.1 mm
5. **Feed posición:** Optimizar en diagonal hasta S₁₁ ≈ −12 dB

### Salida final

| Parámetro | Valor |
|-----------|-------|
| a | 39.51 mm |
| b | 39.17 mm |
| a/b | 1.0087 |
| Feed (x_f, y_f) | (7.637, 7.637) mm |
| **S₁₁** | −12.53 dB |
| **AR mín.** | 1.53 dB @ 868.5 MHz |
| **ARBW** | 2.17 MHz |

## Referencia de implementación

**Herramienta:** CST Studio Suite 2022 (Time Domain)

**Proceso en CST:**
1. Crear parche elíptico (Geometric Primitives → Ellipse)
2. Especificar a, b, z=0, material conductor perfecto
3. Sustrato: Layer (RO4360G2 con parámetros reales)
4. Feed: Point Source en (x_f, y_f)
5. Boundary Conditions: Open Space (PML)
6. Frequency Range: 0.75–0.98 GHz
7. Farfield Monitor: 500 puntos en 868±30 MHz
8. Simulate → Export Results

## Comparación con dual-feed

| Aspecto | Monofeed (Balanis) | Dual-feed |
|--------|-------|----------|
| Feeds | 1 | 2 |
| Acoplador | No | Sí (híbrido 90°) |
| Simplicidad | Alta | Baja |
| AR | ±0.5 dB típico | ±0.2 dB (mejor) |
| Masa | Baja | Media |
| Costo | Bajo | Medio |
| Impedancia | 50 Ω directo | Requiere transformación |

**Decisión (TFG):** Monofeed (simplicidad, masa baja).  
**Post-TFG (propuesta):** Dual-feed en 2.4 GHz para validar técnica.

---

Última actualización: Septiembre 2026
