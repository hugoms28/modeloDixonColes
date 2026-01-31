# Guía de Uso - Modelo Dixon-Coles xG

## Workflow por Jornada

### ANTES de la jornada (jueves/viernes)

1. **Abrir el notebook**
   ```
   jeke-dixon-coles.ipynb
   ```

2. **Cargar modelo guardado** (sin reentrenar)
   ```python
   model = load_model(latest=True, league="EPL")
   ```

3. **Ejecutar PASO 1** - Obtener partidos y odds
   - Descarga automática de Pinnacle
   - Si falla, usa los partidos manuales configurados

4. **Ejecutar PASO 2** - Generar predicciones
   - Muestra tabla con probabilidades, over/under, BTTS
   - Muestra value bets con edge >= 3%

5. **Registrar apuestas**
   ```python
   log_bets_from_predictions(df_predictions, min_edge=0.03)
   ```

6. **Hacer apuestas reales** en tu casa de apuestas

---

### DURANTE la jornada (fin de semana)

Nada que hacer. Esperar a que se jueguen los partidos.

---

### DESPUÉS de la jornada (lunes/martes)

1. **Actualizar resultados** de cada apuesta:
   ```python
   # Victoria local = "1", Empate = "X", Victoria visitante = "2"
   update_result("Leeds vs Arsenal", "1X2", "2")
   update_result("Liverpool vs Newcastle", "1X2", "1")
   update_result("Chelsea vs West Ham", "1X2", "X")
   # ... repetir para cada partido apostado
   ```

2. **Ver estadísticas**
   ```python
   show_roi_stats()
   ```

3. **Ver apuestas pendientes** (si quedaron partidos aplazados)
   ```python
   show_pending_bets()
   ```

---

## Cuándo Reentrenar el Modelo

Reentrenar **después de cada jornada completa** (10 partidos) o cada 2 semanas.

### Pasos para reentrenar:

1. **Limpiar cache** para obtener datos frescos
   ```python
   clear_cache("EPL")
   ```

2. **Forzar descarga** en la celda de datos
   ```python
   USE_CACHE = False
   ```

3. **Ejecutar las celdas** de descarga (PASO 1 de datos) y entrenamiento

4. **Guardar el nuevo modelo**
   ```python
   save_model(model, league="EPL")
   ```

---

## Comandos Útiles

| Acción | Comando |
|--------|---------|
| Cargar modelo | `model = load_model(latest=True, league="EPL")` |
| Guardar modelo | `save_model(model, league="EPL")` |
| Listar modelos | `list_models()` |
| Limpiar cache | `clear_cache("EPL")` |
| Registrar apuestas | `log_bets_from_predictions(df_predictions)` |
| Actualizar resultado | `update_result("Equipo1 vs Equipo2", "1X2", "1")` |
| Ver ROI | `show_roi_stats()` |
| Ver pendientes | `show_pending_bets()` |

---

## Configuración

### Cambiar liga
En la celda de configuración:
```python
LEAGUE = "EPL"  # Opciones: EPL, La_Liga, Bundesliga, Serie_A, Ligue_1
SEASON = ["2025"]
```

### Cambiar edge mínimo
```python
log_bets_from_predictions(df_predictions, min_edge=0.05)  # 5% en vez de 3%
```

### Cambiar stake
```python
# Flat betting (1 unidad por apuesta)
log_bets_from_predictions(df_predictions, stake_mode="flat", base_stake=1.0)

# Kelly fraccional (25% del Kelly óptimo)
log_bets_from_predictions(df_predictions, stake_mode="kelly", base_stake=100)  # 100 = bankroll
```

---

## Archivos del Proyecto

| Archivo | Descripción |
|---------|-------------|
| `jeke-dixon-coles.ipynb` | Notebook principal |
| `models/` | Modelos guardados (.pkl) |
| `cache/` | Cache de datos de Understat |
| `bets_history.csv` | Historial de apuestas |
| `predicciones_*.csv` | Predicciones exportadas |

---

## Troubleshooting

### "No hay modelos guardados"
→ Entrena el modelo primero y guárdalo con `save_model(model)`

### "Equipo X no encontrado"
→ El nombre no coincide. Usa `show_available_teams(model)` para ver los nombres exactos.

### Push a GitHub falla
→ Probablemente tienes cambios en remoto. Ejecuta:
```bash
git pull --rebase origin main
git push
```

### API de odds no funciona
→ El modelo usa fallback manual. Puedes editar `JORNADA_24_MANUAL` en PASO 1 con los partidos correctos.
