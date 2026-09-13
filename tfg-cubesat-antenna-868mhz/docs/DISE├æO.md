# Diseño de la antena — Resumen ejecutivo

## Objetivo

Antena de parche microstrip de polarización circular (RHCP) para enlace de bajada de un CubeSat 1U a estación terrena en 868 MHz (banda LoRa europea).

## Decisiones de diseño

### 1. Geometría: Parche elíptico monofeed

**Justificación:**
- AR (axial ratio) es el parámetro crítico en esta aplicación, no S₁₁
- Parche elíptico perturbado permite generar CP sin componentes múltiples o complejos
- Monofeed reduce masa y complejidad en un CubeSat

**Referencia:** Balanis, *Antenna Theory*, Cap. 14 (1983–2016)

### 2. Sustrato: RO4360G2 (single sheet)

**Especificación:**
- εr = 6.15 (típica @ 10 GHz)
- tanδ = 0.0038 (baja pérdida)
- h = 1.524 mm (espesor)

**Alternativa rechazada:** Diseño A (2×1.524 mm + bondply)
- No fabricable en UC3M sin prensa de laminación
- Complejidad no justificada

**Fuente:** Rogers University Sample Program (órdenes en seguimiento)

### 3. Alimentación: Sonda coaxial en diagonal

**Posición:** (±7.637, ±7.637) mm en diagonal 45°

**Justificación:**
- Genera dos modos degenerados (TM₁₀ rotado 45°)
- Diferencia de fase 90° → CP
- Impedancia ≈ 50 Ω en el punto de óptimo AR

### 4. Método de síntesis: Balanis

**Pasos:**
1. Calcular Q_t desde ancho de banda simulado (ARBW)
2. Ellipticity: a/b = 1 + 1/Q_t
3. Feed en diagonal 45°
4. **Centrado de AR:** Escala simétrica de ambos ejes manteniendo a/b

## Geometría final (Diseño B — sustrato único)

| Parámetro | Valor |
|-----------|-------|
| Plano de masa | 95×95 mm |
| Semi-eje a | 39.51 mm |
| Semi-eje b | 39.17 mm |
| Aspecto a/b | 1.0087 |
| Feed X, Y | ±7.637 mm |
| Feed impedancia (50 Ω) | ~49.93 Ω |

## Resultados de simulación (aislado)

| Parámetro | Simulación | Meta |
|-----------|-----------|------|
| **S₁₁** | −12.53 dB | < −10 dB |
| **AR mín.** | 1.53 dB @ 868.5 MHz | < 3 dB |
| **ARBW (3 dB)** | 2.17 MHz | > 1 MHz |
| **Directividad** | 4.52 dBi | ~ 4 dBi |
| **Eficiencia** | 40% | > 30% |
| **Ganancia realizada** | 0.3 dBi | ~ 1 dBi (con pérdidas) |

**Nota sobre eficiencia:** El 40% es radiada + disipada en pérdidas del sustrato. La ganancia realizada (~0.3 dBi) es baja por las pérdidas dieléctricas de RO4360G2 a 868 MHz.

## Con cuerpo CubeSat (100×100×100 mm PEC)

| Parámetro | Valor | Cambio |
|-----------|-------|--------|
| AR mín. | 2.28 dB @ 867.7 MHz | +0.75 dB (degradación) |
| ARBW | 1.74 MHz | −0.43 MHz |
| Directividad | 3.93 dBi | −0.59 dB |
| Front-to-back | 4.56 dB | — |

**Análisis:** El cuerpo CubeSat degrada ligeramente AR y reduce directividad (acoplamiento capacitivo con ground). Aún dentro de límites aceptables.

## Trade-offs

| Aspecto | Ganancia | Pérdida | Decisión |
|---------|----------|--------|----------|
| **AR vs. S₁₁** | Bajo S₁₁ | Alto AR | Priorizar AR (−12 dB es suficiente) |
| **Eficiencia** | RO4360G2 bajo tanδ | Costo/disponibilidad | Aceptar tanδ = 0.0038 |
| **Masa** | Monofeed simple | AR ±0.5 dB vs. dual-feed | Aceptar variabilidad |
| **Fabricación** | Single sheet | Pérdidas dieléctricas | Single sheet (práctica) |

## Validaciones realizadas

✅ Método Balanis replicado en Pozar (2012) y literatura CP-patch  
✅ Artefacto ARBW identificado (resolución de farfield)  
✅ Eficiencia de radiación vs. pérdidas reconciliada  
✅ Geometría fabricable confirmada por Estrella Benito (UC3M lab)  

## Próximos pasos

1. **Fabricación:** Recibir sustrato RO4360G2 → DXF → Estrella
2. **Medidas:** Anecóica UC3M → parámetros S + radiación
3. **Validación:** Comparar simulación vs. medidas
4. **Futuro (post-TFG):** Antena LoRa dual-feed @ 2.4 GHz en FR-4 (demostración de técnica)

---

**Referencias principales:**
- Balanis, C. A. (2016). *Antenna Theory: Analysis and Design*, 4ª ed., Cap. 14.
- Pozar, D. M. (2012). *Microwave Engineering*, 4ª ed.
- Khotso, M., et al. (2011). "Circularly polarized circular microstrip patch antenna loaded with four H-shaped slots."

Última actualización: Septiembre 2026
