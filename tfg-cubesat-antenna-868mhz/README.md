# Diseño experimental y medida de una antena en banda de microondas

**TFG — Ingeniería en Comunicaciones Móviles y Espaciales | UC3M**

**Autor:** Samuel  
**Supervisor:** Adrián Amor Martín (aamor@tsc.uc3m.es)  
**Año:** 2025–2026  

## Descripción general

Diseño, simulación, fabricación y medida de una **antena de parche microstrip de polarización circular (RHCP)** de alimentación única a **868 MHz** para una aplicación de satélite CubeSat 1U.

### Características clave

- **Frecuencia:** 868 MHz (banda LoRa europea)
- **Tipo:** Parche elíptico de alimentación única
- **Polarización:** Circular a derechas (RHCP)
- **Sustrato:** RO4360G2 (εr = 6.15, tanδ = 0.0038, h = 1.524 mm)
- **Herramientas:** CST Studio Suite 2022 (simulación), LaTeX/Overleaf (documentación)

### Parámetros de diseño final (Diseño B — sustrato único)

| Parámetro | Valor |
|-----------|-------|
| **S₁₁** | −12.53 dB |
| **AR mín.** | 1.53 dB @ 868.5 MHz |
| **ARBW** | 2.17 MHz |
| **Directividad** | 4.52 dBi |
| **Ganancia realizada** | ≈ 0.3 dBi |
| **Eficiencia** | 40% |

*Con cuerpo PEC CubeSat (100×100×100 mm): AR mín. = 2.28 dB @ 867.7 MHz*

## Contenido del repositorio

```
tfg-cubesat-antenna-868mhz/
├── README.md                          # Este archivo
├── LICENSE                            # MIT License
├── .gitignore
│
├── docs/                              # Documentación
│   ├── DISEÑO.md                      # Resumen ejecutivo del diseño
│   ├── FABRICACIÓN.md                 # Notas sobre fabricación y medidas
│   └── memoria.pdf                    # PDF compilado final (cuando esté disponible)
│
├── cst-simulation/                    # Modelos y resultados de simulación
│   ├── README.md
│   ├── diseño-b-isolated.cst
│   ├── diseño-b-with-cubesat.cst
│   └── resultados/
│       ├── s11-magnitude.png
│       ├── ar-frequency-response.png
│       ├── farfield-pattern.png
│       └── summary.txt
│
├── fabrication/                       # Archivos de fabricación
│   ├── README.md
│   ├── dxf/
│   │   ├── antenna-top-layer.dxf
│   │   └── antenna-bottom-layer.dxf
│   └── specifications.txt
│
├── measurements/                      # Datos experimentales
│   ├── README.md
│   ├── data/
│   │   ├── s-parameters.csv
│   │   ├── ar-measurements.csv
│   │   └── radiation-pattern.csv
│   └── plots/
│
├── theory/                            # Fundamentos teóricos
│   ├── balanis-methodology.md
│   ├── cp-patch-design-notes.md
│   └── references.bib
│
└── scripts/                           # Utilidades
    ├── parse-cst-results.py
    └── plot-results.py
```

## Estado actual

✅ Simulación completada (Diseño B validado)  
✅ Memoria LaTeX escrita y compilada  
⏳ Fabricación en progreso (sustrato RO4360G2 pendiente)  
⏳ Medidas en anecóica (programadas tras recibir sustrato)

## Cómo usar este repositorio

### Ver la memoria
La memoria compilada estará en `docs/memoria.pdf`.

### Abrir los modelos CST
1. Descargar CST Studio Suite 2022 (o versión compatible)
2. Abrir `cst-simulation/diseño-b-isolated.cst` o `.../diseño-b-with-cubesat.cst`
3. Ejecutar simulación

### Reproducir las medidas
Tras completar la fabricación, los datos experimentales estarán en `measurements/data/`.

## Metodología teórica

Este diseño se basa en el **método Balanis** (Capítulo 14 de *Antenna Theory: Analysis and Design*, 4ª ed.):

1. Calcular factor de calidad Q_t desde ancho de banda simulado
2. Determinar relación de ejes: a/b = 1 + 1/Q_t
3. Posicionar alimentador en diagonal 45°
4. Centrar AR por escala simétrica de ejes

Ver `theory/balanis-methodology.md` para detalles.

## Referencias principales

- Balanis, C. A. (2016). *Antenna Theory: Analysis and Design*, 4ª ed.
- Pozar, D. M. (2012). *Microwave Engineering*, 4ª ed.
- Nguyen, H. T., et al. (2023). Circular polarized antenna for LoRa applications

## Contacto

**Samuel** | 100472487@alumnos.uc3m.es  
**Supervisor:** Adrián Amor Martín | aamor@tsc.uc3m.es

## Licencia

Este proyecto está bajo licencia **MIT**. Ver `LICENSE` para detalles.

---

*Última actualización: Septiembre 2026*
