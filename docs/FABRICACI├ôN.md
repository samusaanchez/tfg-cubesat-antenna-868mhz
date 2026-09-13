# Fabricación y medida — Notas técnicas

## Estado actual (Septiembre 2026)

✅ Simulación completada y validada  
✅ Memoria LaTeX compilada (Capítulo 7)  
⏳ Sustrato RO4360G2 en seguimiento (Rogers University Sample Program)  
⏳ Fabricación programada para Q4 2026  
⏳ Medidas en anecóica (UC3M) tras fabricación  

## Sustrato: RO4360G2

**Especificación Rogers:**
- Material: Woven Glass Reinforced PTFE Composite
- Espesor: 1.524 mm (60 mil) ±0.127 mm
- Pérdida de inserción @ 10 GHz: < 0.10 dB/in
- Permitividad relative (Dk) @ 10 GHz: 6.4 (típica), 6.15 (nuestro modelo)
- Tangente de pérdidas (Df) @ 10 GHz: 0.0038

**Órdenes en seguimiento:**
- Orden #2607128419 (Samuel)
- Orden #2608129113 (Adrián Amor)
- Contacto: ACS-Europe.Samples@rogerscorporation.com

**Alternativa:**
- IMCD España (distribuidor Rogers): Cristina Bernad (cristina.bernad@imcd.es)

## Proceso de fabricación

### Paso 1: Archivo DXF

**Pendiente:** Exportar desde CST o CAD

**Capas necesarias:**
- Top layer (patch)
- Bottom layer (ground plane)
- Mounting holes (si aplica)
- Dimension/outline

**Tolerancia:** ±0.1 mm (microfresado)

### Paso 2: Técnica de fabricación

**Opción principal (solicitada a Estrella):**
- Fresadora CNC de UC3M
- Método subtractive (mecanización de placa laminada)
- No requiere prensa ni laminación
- Tiempo estimado: 2-3 días de labor

**Sustrato base:** 
- RO4360G2 laminado sobre cobre (0.5 oz, 17 μm) en ambas caras
- Status: Pendiente recepción Rogers

### Paso 3: Ensamblaje del conector

**Tipo:** Conector SMA macho (50 Ω, impedancia característica)

**Sonda de alimentación:**
- Micro-sonda coaxial o mediante perno/orificio central
- Posición: (±7.637, ±7.637) mm en diagonal
- Impedancia objetivo: 50 Ω (simulada: 49.93 Ω)

**Soldadura:**
- Temperatura: 260°C máximo (Tg de RO4360G2 = 280°C)
- Tiempo: Minimizar (PTFE es sensible a temperatura prolongada)

## Medidas experimentales

### Equipamiento disponible (UC3M)

**Analizador de red:**
- Modelo: [A confirmar con Adrián / lab técnico]
- Rango: DC–6 GHz (cubre 868 MHz)
- Calibración: SOLT (Short-Open-Load-Thru)

**Cámara anecóica:**
- Disponible en instalaciones UC3M
- Rango: HF–18 GHz (apropiado)
- Posición: Diagonal de espacio de medida

### Protocolo de medida

**1. Parámetros S (S₁₁, S₂₁)**

```
Frecuencia: 850–900 MHz (50 MHz alrededor de 868)
Puntos: 401 (resolución 0.1 MHz)
Potencia: 0 dBm
Ancho de banda IF: 100 Hz (para SNR)
Modo: Calibración SOLT en plano SMA
```

**2. Patrón de radiación (farfield)**

```
Posición: Cámara anecóica, ~ 2 m de distancia
Frecuencia: 868 MHz (fundamental)
Polarización: Lineal (horizontal) en dos orientaciones
Rango angular: 0° a 360° (plano horizontal), 0° a 180° (elevación)
Paso angular: 5°
```

**3. Polarización circular (axial ratio)**

```
Método 1 — Sonda de polarización lineal rotante:
  - Girar sonda linealmente en plano de radiación
  - Registrar magnitud vs. ángulo de rotación
  - Calcular AR = (E_max + E_min) / (E_max − E_min)

Método 2 — Dos antenas ortogonales (si disponibles):
  - Medir componentes E_h y E_v simultáneamente
  - AR = √[(E_h² + E_v²) / (E_h − E_v)²]
```

### Resultados esperados

| Medida | Simulado | Tolerancia | Anotación |
|--------|----------|-----------|-----------|
| **S₁₁ @ 868 MHz** | −12.53 dB | ±1.5 dB | Variación de sustrato Dk ±2% |
| **AR mín.** | 1.53 dB @ 868.5 MHz | ±0.5 dB | Sensibilidad: alineación feed |
| **ARBW (3 dB)** | 2.17 MHz | ±0.3 MHz | Pérdidas disipativas afectan |
| **Gain realizado** | ~0.3 dBi | ±2 dB | Muy dependiente de pérdidas |

## Incertidumbres y posibles desviaciones

### Fabricación

- **Tolerancia Dk:** Rogers especifica ±2% en Dk
  - Efecto: Resonancia desplazada ±1–2 MHz, AR degradado ~0.2 dB
  - Mitigación: Post-tuning mecánico (ajuste de patch)
- **Sustrato espesor:** ±0.127 mm en 1.524 mm (±8.3%)
  - Efecto: Cambio en Zin, requiere recentrado AR
- **Tolerancia de fresado:** ±0.05–0.1 mm (típica CNC)
  - Efecto: Menor; semi-ejes a/b insensibles a escala pequeña

### Medidas

- **Pérdidas por conector:** Típicamente −0.3 a −0.5 dB @ SMA macho
- **Acoplamiento cámara anecóica:** Reflexiones de paredes < −20 dB (si bien calibrada)
- **Calibración SOLT:** Repetibilidad típica ±0.2 dB en magnitud

## Documentación post-fabricación

Tras obtener medidas, actualizar en el repositorio:

1. **measurements/data/:**
   - `s-parameters.csv` (frecuencia, S11_mag, S11_fase)
   - `ar-measurements.csv` (frecuencia, AR_dB)
   - `farfield-pattern.csv` (ángulo, ganancia, polarización)

2. **measurements/plots/:**
   - Gráficos PNG comparando simulación vs. medidas
   - Residuos (meas − sim)

3. **Actualizar README principal** con estados finales y lecciones aprendidas

## Próximos pasos post-TFG

**Propuesta de Adrián:** Antena LoRa 2.4 GHz en FR-4
- Objetivo: Validar proceso de fabricación/medida en material más barato
- Tipo: Parche circular CP dual-feed (más estable que monofeed)
- Sustrato: FR-4 (ε_r = 4.5, tan δ ≈ 0.02, costo bajo)
- Beneficio: Documentación de técnica completa

---

Última actualización: Septiembre 2026
