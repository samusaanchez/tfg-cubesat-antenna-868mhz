# CST Studio Suite — Modelos de simulación

## Contenido

- **diseño-b-isolated.cst** — Modelo del parche elíptico aislado (sin cuerpo CubeSat)
- **diseño-b-with-cubesat.cst** — Modelo con cuerpo PEC CubeSat 100×100×100 mm
- **resultados/** — Gráficos y datos de simulación exportados

## Cómo abrir los modelos

1. Abrir **CST Studio Suite 2022** (o versión compatible)
2. File → Open → seleccionar `.cst`
3. En el árbol del proyecto, expandir "Simulation" → "Farfield"
4. Click derecho → "Evaluate All" para ejecutar la simulación completa

## Parámetros de simulación

- **Solvedor:** Time Domain
- **Rango de frecuencia:** 0.75–0.98 GHz (estrecho para evitar excitación prolongada)
- **Muestras de farfield:** 500 puntos en banda estrecha (868 ± 50 MHz)
- **Sustrato:** RO4360G2 (εr = 6.15, tanδ = 0.0038, h = 1.524 mm)
- **Plano de masa:** 95×95 mm (aislado), 105×105 mm (con CubeSat)

## Resultados publicados

### Diseño B aislado

| Parámetro | Valor |
|-----------|-------|
| S₁₁ | −12.53 dB |
| AR mín. | 1.53 dB @ 868.5 MHz |
| ARBW | 2.17 MHz |
| Directividad | 4.52 dBi |
| Eficiencia | 40% |

### Diseño B con CubeSat

| Parámetro | Valor |
|-----------|-------|
| AR mín. | 2.28 dB @ 867.7 MHz |
| ARBW | 1.74 MHz |
| Front-to-back | 4.56 dB |
| Directividad | 3.93 dBi |

## Modificar y reproducir

Para cambiar parámetros (por ejemplo, posición del alimentador):

1. En el árbol, expandir "Modeling" → "Antenna" (o similar)
2. Seleccionar la geometría (patch, feed)
3. En la ventana de propiedades, ajustar valores
4. File → Save
5. Simulation → Run

## Exportar resultados

Para extraer datos a CSV o gráficos:

1. Results → Show farfield plot
2. File → Export → seleccionar formato (CSV, PNG)

## Notas técnicas

- **ARBW artefacto:** Los monitores de farfield con pocas muestras de frecuencia producen valores de ARBW engañosos. Usamos 500 muestras en banda estrecha para resultados coherentes.
- **Velocidad de simulación:** Un rango estrecho (0.75–0.98 GHz) es mucho más rápido que uno amplio en el solvedor de dominio del tiempo.

---

Última actualización: Septiembre 2026
