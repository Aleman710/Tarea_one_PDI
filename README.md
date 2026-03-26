# 🚗 Análisis de Movimiento Vehicular — PDI 2026-1

**Universidad de Antioquia · Facultad de Ingeniería**  
Departamento de Electrónica y Telecomunicaciones  
Procesamiento Digital de Imágenes — Semestre 2026-1

---

## 👥 Integrantes

| Nombre |
|--------|
| Rafael Alemán |
| Esteban Correa |
| Isabela Bedoya Gaviria |

---

## 📋 Descripción

Este proyecto analiza el movimiento de un vehículo a partir de un video grabado con cámara estática lateral. Se aplican técnicas de **visión por computadora y procesamiento digital de imágenes** para estimar en cada instante la posición, velocidad y aceleración del vehículo, comparando los resultados con modelos cinemáticos teóricos (MRU / MRUA).

---

## 🔧 Pipeline de procesamiento

```
Video original
      │
      ▼
1. Calibración de escala (px → m)
      │
      ▼
2. Preprocesamiento
   └─ Escala de grises + Filtro Gaussiano
      │
      ▼
3. Segmentación — BackgroundSubtractorMOG2
   └─ Sustracción de fondo adaptativa
      │
      ▼
4. Morfología matemática
   └─ Apertura → Cierre → Dilatación → Erosión
      │
      ▼
5. Detección de contornos (cv2.findContours)
   └─ Cálculo de centroide con cv2.moments()
      │
      ▼
6. Tracking frame a frame — TrackerCSRT
   └─ Restricción al tramo A-B
      │
      ▼
7. Análisis cinemático
   └─ Distancia euclidiana acumulada
   └─ Ajuste polinomial (MRU / MRUA)
   └─ Velocidad y aceleración por derivada analítica
      │
      ▼
8. Visualización y video final anotado
```

---

## 🛠️ Tecnologías usadas

| Librería | Uso |
|----------|-----|
| `opencv-python` | Captura, segmentación, morfología, tracking, escritura de video |
| `numpy` | Operaciones matriciales, derivadas numéricas, ajuste polinomial |
| `matplotlib` | Gráficas de posición, velocidad y aceleración vs. tiempo |
| `google-colab` | Subida/descarga de archivos en el entorno Colab |

---

## 🚀 Cómo ejecutar

### Opción A — Google Colab (recomendado)

1. Abrir [`procesamiento_comentado.ipynb`](./procesamiento_comentado.ipynb) en [Google Colab](https://colab.research.google.com/).
2. Ejecutar las celdas en orden (`Entorno de ejecución → Ejecutar todo`).
3. Cuando se solicite, subir el archivo de video grabado por el grupo.
4. Al finalizar, se descargarán automáticamente `video_procesado.mp4` y `video_final.mp4`.

### Opción B — Entorno local

```bash
# 1. Clonar el repositorio
git clone https://github.com/<usuario>/<repositorio>.git
cd <repositorio>

# 2. Instalar dependencias
pip install opencv-python numpy matplotlib

# 3. Abrir el notebook (sin celdas de Colab)
jupyter notebook procesamiento_comentado.ipynb
```

> **Nota:** En entorno local, reemplazar `cv2_imshow` por `cv2.imshow` y `files.upload()` por la ruta local del video.

---

## 📁 Estructura del repositorio

```
📦 repositorio/
 ┣ 📓 procesamiento_comentado.ipynb   ← Código principal comentado
 ┣ 📄 README.md                        ← Este archivo
 ┣ 📂 resultados/
 ┃  ┣ 🎬 video_procesado.mp4           ← Video con contornos y trayectoria
 ┃  ┗ 🎬 video_final.mp4               ← Video final con velocidad superpuesta
 ┗ 📂 graficas/
    ┣ 🖼️ posicion_vs_tiempo.png
    ┣ 🖼️ velocidad_vs_tiempo.png
    ┗ 🖼️ aceleracion_vs_tiempo.png
```

---

## 📐 Parámetros de calibración

| Parámetro | Valor | Descripción |
|-----------|-------|-------------|
| `distancia_real` | 3.5 m | Ancho del carril (estándar Colombia) |
| `distancia_pixeles` | 180 px | Medición en el primer frame |
| `escala` | m/px | Relación metros por píxel |
| `LINEA_A` | 350 px | Coordenada Y del punto de inicio (A) |
| `LINEA_B` | 920 px | Coordenada Y del punto de fin (B) |
| `FRAME_INI` | 32 | Frame donde el vehículo entra al tramo |

---

## 📊 Resultados

El código genera automáticamente:

- **Gráfica de posición vs. tiempo** con ajuste polinomial (R²)
- **Gráfica de velocidad vs. tiempo** con velocidad media en km/h
- **Gráfica de aceleración vs. tiempo** con valor del modelo teórico
- **Video procesado** con contorno, centroide y trayectoria superpuestos
- **Video final** con velocidad instantánea en pantalla y barra de escala métrica

---

## 📚 Referencias

- Bradski, G. (2000). *The OpenCV Library*. Dr. Dobb's Journal.
- Lukas, B. D., & Kanade, T. (1981). An iterative image registration technique. *IJCAI*.
- Zivkovic, Z. (2004). Improved adaptive Gaussian mixture model for background subtraction. *ICPR*.

---

*Tarea 1 — Procesamiento Digital de Imágenes 2026-1 · Fecha de entrega: 18 de abril de 2026*
