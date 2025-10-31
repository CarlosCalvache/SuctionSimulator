# Suction Simulator

**Simulador Didáctico de Succión Neonatal**  
Succión Nutritiva (SN) y Succión No Nutritiva (SNN)

---

## Contexto Didáctico

Este simulador es una herramienta educativa desarrollada para la enseñanza de los patrones de succión neonatal en programas de fonoaudiología y ciencias de la salud. Permite visualizar y analizar en tiempo real:

- **Patrón de vacío intraoral** (presión negativa en kPa/mmHg)
- **Coordinación Succión-Deglución-Respiración (SDR)**
- **Métricas NOMAS** (Neonatal Oral-Motor Assessment Scale)
- **Diferencias entre SN y SNN** según edad gestacional corregida

### Fundamento Científico

La succión neonatal es un proceso neuromuscular complejo que integra tres subsistemas:
1. **Succión (S):** Generación de vacío intraoral para extracción de leche
2. **Deglución (D):** Transporte seguro del bolo hacia el esófago
3. **Respiración (R):** Pausas coordinadas para prevenir aspiración

La maduración de estos patrones es crítica para:
- Transición exitosa a alimentación oral en prematuros
- Detección temprana de disfagia neonatal
- Planificación de intervenciones terapéuticas

---

## Interfaz y Controles

### Panel de Control (Izquierda)

#### Configuración de Modo
- **Modo:**
  - **SN (Succión Nutritiva):** Patrón de alimentación al pecho/biberón
    - Frecuencia típica: 40-90 succ/min
    - Racimos prolongados (~10-20s)
    - Coordinación S:D determinista (1:1, 2:1, 3:1)
  - **SNN (Succión No Nutritiva):** Patrón con chupete/pacificador
    - Frecuencia alta: ~120 succ/min (2 Hz)
    - Racimos más cortos (~8s)
    - Deglución probabilística (saliva, no rítmica)

- **Edad / PMA (Edad Post-Menstrual Ajustada):**
  - 32–34 semanas: Prematuros tempranos
  - 34–36 semanas: Prematuros tardíos
  - ≥37 semanas: Término completo

#### Parámetros de Succión

1. **Unidades de vacío:**
   - kPa (kiloPascales) — predeterminado
   - mmHg (milímetros de mercurio) — clínico
   - Pa (Pascales) — científico

2. **Vacío máximo (Pmax):**
   - Rango: 0–4 kPa (~0–30 mmHg)
   - Valores típicos:
     - Al pecho: 13–20 kPa (100–150 mmHg)
     - Tetinas activadas por vacío: ~4 kPa (29 mmHg umbral)
   - **Referencia:** Geddes et al. (2008), Sakalidis et al. (2012)

3. **Frecuencia (succ/min):**
   - Rango: 20–180 succ/min
   - SN: 50–80/min (contexto dependiente)
   - SNN: ~120/min (≈2 Hz)
   - **Referencia:** Martens et al. (2020), Cunha et al. (2019)

4. **Duración de racimo (s):**
   - Tiempo de succión continua antes de pausa
   - SN: 10–20s típico
   - Pretérmino: racimos más cortos, pausas ≥ racimos
   - **Referencia:** Geddes et al. (2017)

5. **Pausa entre racimos (s):**
   - Descanso respiratorio entre episodios de succión
   - Criterio: pausas ≥2s separan racimos
   - Función: recuperación respiratoria, prevención de apnea
   - **Referencia:** Lagarde et al. (2019)

#### Relación SDR (Succión:Deglución:Respiración)

**Modo SN (Determinista):**
- **1:1:1** — Cada succión → deglución → respiración
- **2:1:1** — Dos succiones → deglución → respiración (PREDETERMINADO)
- **3:1:1** — Tres succiones → deglución → respiración
- Patrón maduro según flujo y edad gestacional

**Modo SNN (Probabilístico):**
- **Probabilidad baja** — Deglución ocasional (saliva escasa)
- **Probabilidad media** — Patrón intermedio
- **Probabilidad alta** — Deglución frecuente (saliva abundante)
- Variabilidad realista (seed fijo para reproducibilidad)

**Referencia:** Sakalidis et al. (2012), Lagarde et al. (2019)

#### Presets Rápidos

1. **SN típico:**
   - Frecuencia: 60/min
   - Pmax: 2.2 kPa (~16.5 mmHg)
   - Racimo: 12s, Pausa: 3s
   - SDR: 2:1:1

2. **SNN típico:**
   - Frecuencia: 110/min
   - Pmax: 1.0 kPa (~7.5 mmHg)
   - Racimo: 8s, Pausa: 3s
   - SDR: Probabilidad media

3. **Desorganizado:**
   - Frecuencia: 35/min (baja)
   - Pmax: 0.9 kPa (~6.8 mmHg, insuficiente)
   - Racimo: 5s (corto), Pausa: 6s (prolongada)
   - SDR: Sin overlay
   - Simula patrón inmaduro o disfuncional

---

### Panel de Visualización (Derecha)

#### Canvas Superior: Vacío Intraoral
- **Eje Y:** Presión negativa (0 a -4 kPa / 0 a -30 mmHg)
- **Eje X:** Tiempo (ventana fija de 20 segundos, desplazamiento continuo)
- **Forma de onda:** Valle senoidal suavizado (no diente de sierra)
  - Modelado: `valleyPulse()` con exponente 2.2 (realismo)
- **Límites de pausa:** Líneas amarillas verticales (inicio/fin de racimos)

#### Canvas Inferior: Respiración + Coordinación SDR
- **Respiración (verde):**
  - Solo visible en pausas de succión
  - Suprimida durante racimos y ±0.25s alrededor de deglución (apnea deglutoria)
  - Amplitud proporcional a flujo (0.8 mL/s base)
- **Deglución (púrpura):**
  - Líneas verticales punteadas
  - Sincronizadas con patrón SDR seleccionado
- **Apnea (símbolo ⊘):**
  - Banda gris ±0.25s alrededor de cada deglución
  - Supresión respiratoria protectora

---

## Métricas NOMAS (Educativo)

### KPIs Calculados en Tiempo Real

1. **Vacío máx. (abs):** Pico de presión negativa medido
2. **Frecuencia medida (succ/min):** Calculado desde inter-succión interval (ISI)
3. **Succ por racimo (mediana):** Robustez ante outliers
4. **Duración racimo (mediana, s):** Mediana de longitud de racimos
5. **Pausa media (s):** Promedio de pausas inter-racimo
6. **IC (Índice de Continuidad, 0–1):** `IC = T_succión / (T_succión + T_pausa)`
   - IC alto (>0.65): Alimentación eficiente
   - IC bajo (<0.4): Múltiples pausas, baja resistencia

### Bandas de Referencia por Edad

| Edad (sem) | Vacío mín. | Frecuencia SN | Frecuencia SNN | IC esperado |
|------------|------------|---------------|----------------|-------------|
| 32–34      | 0.7 kPa    | 30–50/min     | 80–120/min     | 0.4–0.6     |
| 34–36      | 1.1 kPa    | 40–70/min     | 90–130/min     | 0.5–0.7     |
| ≥37        | 1.6 kPa    | 40–90/min     | 90–150/min     | 0.65–0.85   |

### Checkboxes Interpretativos (No Diagnóstico)

El simulador marca automáticamente:
- **Continuidad baja:** IC < umbral de edad
- **Racimos cortos:** Duración mediana <2s
- **Pausas prolongadas:** Pausa media >2s
- **Vacío insuficiente:** Pmax < mínimo de edad
- **Frecuencia fuera de rango:** Según modo y edad
- **Incoordinación S-D-R:** Pausas ausentes + alta frecuencia
- **Respiración irregular:** Pausa=0 + frecuencia >120/min

**Clasificación Educativa:**
- **Normal/Maduro:** 0 criterios marcados
- **Desorganizado:** 1–2 criterios
- **Disfuncional:** ≥3 criterios

**⚠️ IMPORTANTE:** Esta clasificación es únicamente con fines didácticos. La evaluación clínica real requiere instrumentación calibrada y juicio profesional especializado.

---

## Convenciones Visuales

### Código de Colores
- 🔵 **Azul (#38bdf8):** Succión (vacío)
- 🟢 **Verde (#10b981):** Respiración
- 🟣 **Púrpura (#a855f7):** Deglución
- ⚫ **Gris (#94a3b8):** Apnea (bandas)
- 🟡 **Amarillo (#facc15):** Límites de pausa

### Principios de Diseño
- **Panel superior limpio:** Solo succión + límites de pausa
- **Panel inferior semántico:** Toda la información SDR superpuesta
- **Sin ruido visual:** Leyenda en tooltips, no permanente
- **Accesibilidad:** Alto contraste, fuentes ≥12px, ARIA labels

---

## Soporte Bibliográfico

### Vacío Máximo (Pmax)
- **Geddes DT, et al.** *Early Human Development.* 2008  
  [PubMed: 18262736](https://pubmed.ncbi.nlm.nih.gov/18262736/)  
  *Picos al pecho: ~100–150 mmHg (13–20 kPa)*

- **Sakalidis VS, et al.** *International Journal of Pediatrics.* 2012  
  [PMC3398629](https://pmc.ncbi.nlm.nih.gov/articles/PMC3398629/)  
  *Tetinas activadas por vacío: umbral ~29 mmHg*

### Frecuencia (SN/SNN)
- **Martens A, et al.** 2020  
  [PMC8943411](https://pmc.ncbi.nlm.nih.gov/articles/PMC8943411/)  
  *SNN ~2 Hz (120/min) en pretérminos*

- **Cunha M, et al.** *Journal of Pediatric and Neonatal Individualized Medicine.* 2019  
  [JPnim 090227](https://www.jpnim.com/index.php/jpnim/article/view/090227)  
  *SN contextual: 50–80/min*

### Racimos y Pausas
- **Geddes DT, et al.** *BMC Pregnancy and Childbirth.* 2017  
  [PMC5693509](https://pmc.ncbi.nlm.nih.gov/articles/PMC5693509/)  
  *Racimos SN: ~10–20s; pretérmino: pausas ≥ racimos*

- **Lagarde MLJ, et al.** *BMC Pediatrics.* 2019  
  [DOI: 10.1186/s12887-019-1445-3](https://bmcpediatr.biomedcentral.com/articles/10.1186/s12887-019-1445-3)  
  *Criterios de segmentación: pausas ≥2s*

### Coordinación S–D–R
- **Sakalidis VS, et al.** 2012 (citado arriba)  
  *Relaciones S:D de 1:1 a 4:1 según flujo/maduración*

- **Lagarde MLJ, et al.** 2019 (citado arriba)  
  *Tetinas SSwB: coordinación comparable al pecho*

### Medición NNS
- **JoVE Protocols.** 2024  
  [DOI: 10.3791/66273](https://www.jove.com/t/66273/non-nutritive-suck-parameters-measurements-using-custom-pressure)  
  *Protocolo: pacificador + transductor de presión*

---

## Uso Técnico

### Requisitos del Sistema
- **Navegador:** Chrome 118+, Firefox 119+, Safari 17+
- **JavaScript:** ES6+ nativo (sin transpilación)
- **Canvas:** Soporte 2D context (estándar HTML5)
- **Rendimiento:** 60 FPS en hardware ≥2018

### Ejecución Local

#### Opción 1: Servidor Python
```bash
python3 -m http.server 5000
# Abrir http://localhost:5000
```

#### Opción 2: Servidor Node.js
```bash
npx serve -p 5000
```

#### Opción 3: Live Server (VS Code)
```bash
# Extensión: Live Server
# Click derecho en index.html → "Open with Live Server"
```

### Despliegue en Producción

#### GitHub Pages
```bash
# 1. Push a repositorio GitHub
git push origin main

# 2. Settings → Pages → Source: main branch
# 3. URL: https://username.github.io/suction-simulator/
```

#### Vercel (Estático)
```bash
# Archivo vercel.json (opcional):
{
  "version": 2,
  "builds": [{ "src": "index.html", "use": "@vercel/static" }]
}

# Deploy:
vercel --prod
```

#### Replit (Hosting Integrado)
```yaml
# replit.yaml (ya incluido)
run: ["sh", "-c", "python3 -m http.server 5000"]
```

---

## Arquitectura Técnica

### Estructura de Archivos
```
/
├── index.html          # Aplicación completa (HTML + CSS + JS inline)
├── assets/             # Recursos estáticos
│   └── logo_Ibero.png  # Logo institucional (fallback SVG inline)
├── docs/
│   ├── README.md       # Este archivo
│   └── perf-notes.md   # Documentación de rendimiento
├── CITATION.cff        # Metadatos de citación académica
├── LICENSE             # Licencia de uso
├── .gitignore          # Exclusiones de Git
└── replit.yaml         # Configuración de hosting Replit
```

### Optimizaciones de Rendimiento
Ver [docs/perf-notes.md](./perf-notes.md) para análisis completo:
- **Offscreen canvas buffers:** Grillas cacheadas (40% reducción)
- **Pre-cálculo de coordenadas:** xPixTop/xPixBot (Float32Array)
- **Throttling de análisis:** 10Hz en lugar de 60Hz
- **Buffer reutilizable:** Elimina GC pressure
- **Debounce de resize:** 100ms

**Resultado:** 58-60 FPS sostenidos en hardware modesto (2019 mid-range)

---

## Limitaciones y Disclaimer

### Propósito Educativo
Este simulador es una **herramienta didáctica** para la enseñanza de conceptos de succión neonatal. **NO** debe usarse para:
- Diagnóstico clínico real
- Toma de decisiones terapéuticas
- Evaluación de pacientes sin instrumentación calibrada

### Simplificaciones del Modelo
- **Forma de onda:** Aproximación senoidal (real: más variabilidad)
- **SDR determinista:** SN usa ratios fijos (real: variabilidad temporal)
- **Presión constante:** Pmax uniforme (real: fluctuaciones intra-racimo)
- **Sin ruido:** Señal ideal (real: artefactos de movimiento, EMG)

### Validación Clínica
Los parámetros del simulador están **basados en evidencia científica publicada**, pero:
- Rangos de normalidad son **orientativos** (poblaciones varían)
- Criterios NOMAS simplificados (escala real: 28 ítems)
- Clasificación educativa ≠ diagnóstico profesional

**Recomendación:** Usar como complemento a evaluación instrumental real (manometría, videofluoroscopia, FEES).

---

## Cómo Citar Este Simulador

### Formato APA
```
Calvache, C. (2025). Suction Simulator SN/SNN: Simulador Didáctico de Succión Neonatal
(Versión 1.0) [Software]. Universidad Iberoamericana.
https://github.com/username/suction-simulator
```

### BibTeX
```bibtex
@software{calvache2025suction,
  author = {Calvache, Carlos},
  title = {Suction Simulator SN/SNN: Simulador Didáctico de Succión Neonatal},
  year = {2025},
  version = {1.0},
  publisher = {Universidad Iberoamericana},
  url = {https://github.com/username/suction-simulator}
}
```

Ver [CITATION.cff](../CITATION.cff) para formato completo.

---

## Contacto y Contribuciones

**Investigador Principal:**  
Flgo. Carlos Calvache, Ph.D.  
Docente de Carrera, Programa de Fonoaudiología  
Facultad Ciencias de la Salud  
Corporación Universitaria Iberoamericana (IBERO)

**Repositorio:**  
[https://github.com/username/suction-simulator](https://github.com/username/suction-simulator)

**Reportar Problemas:**  
[GitHub Issues](https://github.com/username/suction-simulator/issues)

**Contribuir:**
1. Fork del repositorio
2. Crear rama de feature (`git checkout -b feature/mejora`)
3. Commit cambios (`git commit -m 'Añade mejora X'`)
4. Push a la rama (`git push origin feature/mejora`)
5. Abrir Pull Request

---

## Licencia

[Ver LICENSE](../LICENSE) para términos de uso.

**Sugerencia:** MIT (máxima adopción) o CC BY-NC-SA 4.0 (uso no comercial).

---

**Última Actualización:** Octubre 31, 2025  
**Versión:** 1.0  
**Estado:** Producción — Uso Educativo
