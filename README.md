# Proyecto Heuristica y Optimización

Incluye un solucionador de asignación de estacionamientos mediante satisfacción de restricciones y un planeador de rutas para ambulancias con búsqueda A*.

## Estructura del proyecto

- `Part1`: Script que formula el problema de asignación de vehículos a plazas de estacionamiento como un CSP utilizando la librería `python-constraint`. 【F:Part1†L1-L140】
- `Part2`: Versión inicial del planeador de rutas para ambulancias basado en búsqueda A*, con funciones heurísticas exploratorias. 【F:Part2†L1-L236】
- `definitivo`: Implementación pulida del planeador A* empleada como entrega final; mantiene la misma lógica general que `Part2` con mejoras en documentación y limpieza. 【F:definitivo†L1-L234】
- `Heuristics`: Borrador de funciones heurísticas experimentales (no utilizado directamente en los scripts principales). 【F:Heuristics†L1-L48】

## Requisitos previos

1. Python 3.8 o superior.
2. Dependencias:
   - [`python-constraint`](https://pypi.org/project/python-constraint/) para el solucionador de parking.

Instalación rápida de dependencias:

```bash
python -m pip install python-constraint
```

## Uso

### 1. Asignación de estacionamientos (`Part1`)

El script resuelve una instancia de estacionamiento donde cada vehículo debe ubicarse en una plaza cumpliendo restricciones de tipo, congelador y maniobrabilidad.

**Formato de entrada**

1. Primera línea: número de filas y columnas del estacionamiento, por ejemplo `Filas: 4 Columnas: 5`.
2. Segunda línea: plazas conectadas a la red eléctrica con formato `Electricas: (1,1) (2,3) ...`.
3. Líneas siguientes: vehículos en formato `<id>-<tipo>-<freezer>`, donde `tipo` puede ser `TSU` o `TNU`, y `freezer` indica si requiere conexión (`C`) o no (`X`).

**Ejecución**

```bash
python Part1 instancia.txt
```

La primera solución encontrada se exporta a `parking.csv`, con el número total de soluciones en la primera línea seguido de la matriz formateada. 【F:Part1†L120-L183】

### 2. Planeador A* para ambulancias (`Part2` y `definitivo`)

Ambos scripts reciben un mapa en CSV y un número de heurística (`1` o `2`) para guiar la búsqueda A*. El archivo CSV debe estar separado por `;` y emplear los siguientes símbolos:

- `P`: posición de la base/parqueadero.
- `C` / `N`: pacientes contiguos / no contiguos.
- `CC` / `CN`: zonas de entrega de pacientes contiguos / no contiguos.
- `X`: celda bloqueada.
- Enteros: coste temporal de desplazamiento por la celda.

**Ejecución**

```bash
python definitivo mapa.csv 1
```

El programa imprime por consola el nombre del archivo base y genera dos archivos:

- `<mapa>-<h>.output`: secuencia de celdas visitadas con carga de energía restante. 【F:definitivo†L205-L221】
- `<mapa>-<h>.stat`: métricas de tiempo total, coste, longitud del plan y nodos expandidos. 【F:definitivo†L189-L204】

> `Part2` comparte la misma interfaz pero contiene mensajes de depuración adicionales; se recomienda utilizar `definitivo` para ejecuciones formales. 【F:Part2†L227-L248】

## Notas adicionales

- La energía inicial de la ambulancia se controla con la constante `initial_energy` (50 unidades por defecto). 【F:definitivo†L1-L12】
- Ambos scripts A* limitan el número de recargas a 5 (`timesinp < 5`) para acotar el espacio de búsqueda. 【F:definitivo†L160-L188】
- Los resultados pueden variar según el tamaño del mapa y la heurística seleccionada; pruebe ambas para comparar el rendimiento.

## Contribución

1. Crear una rama a partir de `main`.
2. Implementar los cambios y añadir pruebas o ejemplos si aplica.
3. Ejecutar `git commit` con un mensaje descriptivo.
4. Abrir un Pull Request describiendo los cambios realizados.
