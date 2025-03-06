# Sistema de Balanceo para PTL (Put-to-Light) 🧩

[![Python 3.8+](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Métodos constructivos avanzados para optimización de sistemas logísticos PTL mediante heurísticas y metaheurísticas.

## 📖 Contenidos
- [Visión General](#-visión-general)
- [Características Clave](#-características-clave)
- [Instalación](#-instalación)
- [Uso](#-uso)
- [Algoritmos Implementados](#-algoritmos-implementados)
- [Parámetros Clave](#-parámetros-clave)
- [Resultados](#-resultados)
- [Contribución](#-contribución)
- [Licencia](#-licencia)
- [Contacto](#-contacto)

## 🌐 Visión General
Sistema de optimización para balancear cargas de trabajo en sistemas PTL (Put-to-Light) mediante:
- **Heurística Greedy** Clásica con estrategias adaptativas
- **Heurística Layered** Basada en clasificación previa de importancia
- **Metaheurística GRASP** (Greedy Randomized Adaptive Search Procedure)
- Múltiples objetivos de optimización: tiempo promedio, diferencia máxima-minima, y tiempo máximo

## 🚀 Características Clave
- ✅ Soporte para múltiples funciones objetivo
- ✅ Configuración flexible de parámetros
- ✅ Sistema adaptativo de asignación de posiciones
- ✅ Visualización de resultados comparativos
- ✅ Manejo eficiente de restricciones operacionales
- ✅ Implementación paralelizable

## 📦 Instalación
1. Clonar repositorio:
```bash
git clone https://github.com/GatosLoco1990/PTL_Entrega1.git
cd PTL_Entrega1
```

2. Instalar dependencias:
```bash
pip install -r requirements.txt
```

## 🖥️ Uso
### Formato de Entrada

Estructura detallada del archivo Excel de entrada (`*.xlsx`):

| Hoja Excel                | Descripción                                                                 | Estructura Ejemplo                                                                 |
|---------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| **Pedidos**               | Listado de todos los pedidos                                                | `Pedido_1`, `Pedido_2`, ..., `Pedido_N` (como filas)                             |
| **Zonas**                 | Identificadores de zonas disponibles                                       | `Z1`, `Z2`, ..., `ZM` (como filas)                                               |
| **Salidas**               | Puntos de salida del sistema                                               | `S001`, `S002`, ..., `SKP` (como filas)                                          |
| **SKU**                   | Unidades de mantenimiento de stock                                         | `SKU_1`, `SKU_2`, ..., `SKU_L` (como filas)                                      |
| **Salidas_en_cada_zona**  | Capacidad de salidas por zona                                               | `Zonas` \| `Num_Salidas`<br>`Z1` \| `20`<br>`Z2` \| `20`                         |
| **Salidas_pertenece_zona**| Matriz de pertenencia salidas-zonas                                        | Matriz con zonas como filas y salidas como columnas (`1` = pertenece, `0` = no)  |
| **Tiempo_salida**         | Tiempos de transporte por salida y zona                                     | Matriz con zonas como filas y salidas como columnas (valores en minutos)         |
| **SKU_pertenece_pedido**  | Relación SKU-Pedidos                                                       | Matriz con pedidos como filas y SKUs como columnas (`1` = pertenece, `0` = no)   |
| **Tiempo_SKU**            | Tiempos de procesamiento por SKU y pedido                                  | Matriz con pedidos como filas y SKUs como columnas (valores en minutos)           |
| **Parametros**            | Parámetros operativos del sistema                                          | `Parametros` \| `Valor`<br>`v` \| `61.66`<br>`zn` \| `2`                         |


### Ejecución Básica

Ejecutar el notebook **PTL Constructivo**

## 🧠 Algoritmos Implementados
### 1. Heurística Greedy Constructiva
```python
procedure GREEDY:
    Inicializar cargas W_j = 0 ∀j ∈ Z
    Ordenar pedidos (si es min_max)
    for each pedido i ∈ P:
        Generar candidatos válidos C_i
        Evaluar impacto en cada zona
        Seleccionar mejor opción según objetivo
        Actualizar cargas y posiciones
    return asignaciones y métricas
```

### 2. Metaheurística Layered 
```python
procedure LAYERED_ASSIGNMENT:
    # Fase 1: Clasificación estratificada
    Clasificar pedidos en 3 capas:
        - Alta: Mayor tiempo procesamiento + más SKUs
        - Media: Balance entre SKUs y tiempo
        - Baja: Resto de pedidos
    
    # Fase 2: Procesamiento jerárquico
    for capa in [Alta, Media, Baja]:
        for pedido in capa:
            Generar candidatos válidos:
                - Posiciones disponibles más cercanas
                - Calcular impacto temporal Y_ij
                - Simular carga temporal
            
            # Selección adaptativa
            if objetivo = min_avg:
                seleccionar mínima contribución
            elif objetivo = min_diff:
                seleccionar mínima diferencia
            elif objetivo = min_max:
                seleccionar mínimo máximo
            
            Actualizar estructuras:
                - Cargas de trabajo
                - Posiciones ocupadas
                - Asignaciones registradas
    
    return solución estratificada
```

### 3. Metaheurística GRASP
```python
procedure GRASP:
    for iter = 1 to max_iterations:
        Construir solución greedy aleatorizada
            - Crear RCL con factor α
            - Selección aleatoria controlada
        Búsqueda local (opcional)
        Actualizar mejor solución
    return mejor solución encontrada
```

## 🎛️ Parámetros Clave
| Parámetro | Valores | Descripción |
|-----------|---------|-------------|
| `--alpha` | 0.0-1.0 | Factor de aleatorización en GRASP |
| `--iterations` | 1-1000 | Número de iteraciones GRASP |
| `--objective` | min_avg, min_diff, min_max | Función objetivo |

## 📊 Resultados
Métricas principales:
- Tiempo de ejecución
- Valor de función objetivo
- Balance de carga por zona
- Distribución de asignaciones

Ejemplo de visualización:
![Resultados Comparativos](Comparación min_diff.png)

## 🤝 Contribución
1. Hacer fork del proyecto
2. Crear branch (`git checkout -b feature/nueva-funcionalidad`)
3. Commit cambios (`git commit -m 'Add amazing feature'`)
4. Push al branch (`git push origin feature/nueva-funcionalidad`)
5. Abrir Pull Request

## 📄 Licencia
Distribuido bajo licencia MIT. Ver `LICENSE` para más detalles.

## 📧 Contacto
Jesús Ricardo Gandica Velasco  
📧 jrgandicav@eafit.edu.co
🔗 [LinkedIn](https://www.linkedin.com/in/ricardo-gandica-gg/)