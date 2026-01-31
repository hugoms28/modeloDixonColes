# Modelo Dixon-Coles con xG

Modelo de predicción de fútbol basado en **Dixon-Coles** usando **Expected Goals (xG)** para estimar probabilidades de resultados y detectar value bets.

## Características

- **Time Decay**: Partidos recientes pesan más (half_life configurable)
- **Múltiples mercados**: 1X2, Over/Under 1.5/2.5/3.5, BTTS, Doble Oportunidad
- **Comparación con odds**: Integración con Pinnacle vía the-odds-api
- **Value betting**: Detección automática de apuestas con edge positivo
- **ROI Tracking**: Sistema de seguimiento de apuestas y resultados
- **Cache inteligente**: Evita descargas repetidas de Understat

## Instalación

```bash
git clone https://github.com/hugoms28/modeloDixonColes.git
cd modeloDixonColes
pip install -r requirements.txt
```

## Uso rápido

```python
# Cargar modelo entrenado
model = load_model(latest=True, league="EPL")

# Predecir un partido
pred = predict_match("Liverpool", "Arsenal", model)
print(f"Victoria Liverpool: {pred['p_home']*100:.1f}%")
print(f"Over 2.5: {pred['p_over_25']*100:.1f}%")
```

## Documentación

Ver [GUIA_USO.md](GUIA_USO.md) para el workflow completo por jornada.

## Estructura del proyecto

| Archivo | Descripción |
|---------|-------------|
| `jeke-dixon-coles.ipynb` | Notebook principal con el modelo |
| `GUIA_USO.md` | Guía de uso paso a paso |
| `models/` | Modelos guardados |
| `cache/` | Cache de datos de Understat |
| `bets_history.csv` | Historial de apuestas |

## Ligas disponibles

- EPL (Premier League)
- La_Liga
- Bundesliga
- Serie_A
- Ligue_1

## Créditos

Proyecto desarrollado a partir de los conceptos y código de [Jeke Deportivas](https://github.com/jeke-deportivas), gran referente en el análisis de datos deportivos con xG.

## Licencia

MIT
