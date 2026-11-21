# Mecanismos de atención y Transformers aplicados a series de tiempo

Repositorio de la actividad **R2-A3-S12 Transformers** de la asignatura *Deep Learning – Conceptos* (604013), Especialización en Inteligencia Artificial, Universidad de Cundinamarca.

Aquí están los dos notebooks que desarrollé en Google Colab para poner en práctica lo que plantea el paper *Attention Is All You Need* (Vaswani et al., 2017):

| Notebook | Contenido | Abrir en Colab |
|---|---|---|
| `01_atencion_series_tiempo.ipynb` | LSTM con atención aditiva (Bahdanau) implementada a mano, comparada con una LSTM sin atención y una línea base de persistencia. Incluye el análisis de los pesos de atención. | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Juanpatula2361/deep-learning-transformers-series-tiempo/blob/main/notebooks/01_atencion_series_tiempo.ipynb) |
| `02_transformer_basico.ipynb` | Encoder Transformer construido desde cero en Keras: atención por producto punto escalado, atención multi-cabeza, codificación posicional y bloques *Add & Norm*. Incluye una prueba sin codificación posicional y mapas de auto-atención por cabeza. | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Juanpatula2361/deep-learning-transformers-series-tiempo/blob/main/notebooks/02_transformer_basico.ipynb) |

## Datos

Usé las **temperaturas mínimas diarias de Melbourne, Australia (1981–1990)**: 3.650 registros y ningún valor faltante. Los notebooks descargan el archivo directamente desde [este enlace](https://raw.githubusercontent.com/jbrownlee/Datasets/master/daily-min-temperatures.csv), así que no hay que subir nada a mano.

- **Tarea:** predecir la temperatura mínima del día siguiente a partir de los últimos 30 días.
- **División temporal:** 1981–1987 para entrenamiento, 1988 para validación y 1989–1990 para prueba.

## Resultados en el conjunto de prueba

| Modelo | MAE (°C) | RMSE (°C) | R² |
|---|---|---|---|
| Persistencia (línea base) | 1,953 | 2,481 | 0,635 |
| LSTM sin atención | 1,712 | 2,176 | 0,719 |
| LSTM con atención (Bahdanau) | 1,712 | 2,175 | 0,719 |
| Transformer (2 bloques, 4 cabezas) | 1,716 | 2,196 | 0,714 |
| Transformer sin codificación posicional | 1,779 | 2,285 | 0,690 |

Lo que más vale la pena destacar:

- Todos los modelos superan a la línea base con claridad.
- La atención de Bahdanau aprendió a fijarse en los días más fríos de la ventana: la correlación promedio entre el peso de atención y la temperatura es de −0,88.
- En el Transformer, cada cabeza de atención se especializó en algo distinto. Una mira los días recientes, otra los días fríos y otra los días cálidos.
- Quitar la codificación posicional empeora el modelo, lo que confirma en la práctica por qué es una pieza clave.

## Estructura

```
├── notebooks/
│   ├── 01_atencion_series_tiempo.ipynb
│   ├── 02_transformer_basico.ipynb
│   ├── figuras/        # gráficas generadas por los notebooks
│   └── resultados/     # métricas en formato JSON
├── requirements.txt
└── README.md
```

## Cómo ejecutarlo

**En Google Colab:** abre cualquiera de los notebooks con el botón de la tabla y ejecuta *Entorno de ejecución → Ejecutar todas*. Basta con CPU; cada notebook tarda unos pocos minutos.

**En local:**

```bash
pip install -r requirements.txt
jupyter notebook notebooks/
```

Ejecuta primero el notebook 1 para que el notebook 2 pueda cargar sus métricas en la tabla comparativa.

## Referencias

- Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., Kaiser, Ł., & Polosukhin, I. (2017). *Attention Is All You Need*. arXiv:1706.03762.
- Bahdanau, D., Cho, K., & Bengio, Y. (2014). *Neural Machine Translation by Jointly Learning to Align and Translate*. arXiv:1409.0473.
- Doshi, K. (2021). *Transformers Explained Visually (Part 2): How It Works, Step-by-Step*. Towards Data Science.
