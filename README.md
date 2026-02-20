# Advanced Price Elasticity Framework (GBM)
**Author:** SuaS</br>
**Creation Date:** 2026-02-17</br>

# Project Structure
|- Code: Code Notebooks Container </br>
|- inputs: Input data files </br>
|- models: Trained Model Repository </br>
|- outputs: All output files </br>


### La brecha entre Sell-in y Sell-out 

**Visión de Negocio:** Explicar que la empresa no controla el precio final. El retailer es un "filtro" que puede amplificar o absorber nuestras decisiones de precio.</br>

**Technical View:** Definir el problema como una cadena de causalidad: $P_{in} \to P_{out} \to Volume$.  Si no modelamos el primer paso, la elasticidad final está sesgada.</br>

### Fase 1: Modelando el Traspaso de Precio (Pass-through)
**Concepto:** ¿Cuánto de nuestro aumento llega al anaquel?</br>
**Detalle Técnico:** Uso de un Distributed Lag Model (DLM). Explicar por qué usamos rezagos ($t-1, t-2$) para capturar la velocidad de reacción del retailer.</br>
**Impacto de Negocio:** Identificar qué retailers son "aliados" (pasan el precio gradualmente) y cuáles son "agresivos" (sobre-reaccionan).</br>

### Fase 2: El Modelo de Demanda (Gradient Boosting)
**Concepto:** Capturar el comportamiento no lineal del consumidor.</br>
**Detalle Técnico:** Por qué elegimos HistGradientBoosting sobre una regresión lineal (captura de interacciones complejas, estacionalidad y efectos no lineales).</br>
**Impacto de Negocio:** Entender que la sensibilidad al precio cambia según la temporada y la actividad promocional.</br>

### Fase 3: Inyectando "Sentido Común" (Monotonic Constraints)
**Concepto:** Asegurar que el modelo nunca contradiga la lógica económica.</br>
**Detalle Técnico:** Explicación de las Monotonic Constraints ($[-1, -1, -1, 1, 1, 0]$). Cómo forzamos al modelo a asignar la causalidad correcta en casos ruidosos (como el ejemplo de Semana Santa).</br>
**Impacto de Negocio:** Eliminar resultados absurdos (como elasticidades positivas) que quitan credibilidad a la herramienta frente a los tomadores de decisiones.</br>

### Resultados Clave: El "Golden Number"
**La Métrica:** Diferencia entre Elasticidad Sell-out (consumidor) y Elasticidad Sell-in (efectiva para la empresa).</br>
**Simulación:** Mostrar un ejemplo de "What-if": "Si subimos el 5% de Sell-in, el volumen caerá X% debido al efecto combinado de la reacción del retailer y la sensibilidad del cliente".</br>

### Validación y Confianza (MAPE)
**Rigor Técnico:** Análisis de la brecha Train vs. Test. Explicar la Regularización aplicada para evitar el sobreajuste (Overfitting).</br>
**Confianza de Negocio:** Establecer niveles de confianza por SKU. "En este 80% del portafolio, el modelo es altamente accionable; en este 20%, la demanda es ruidosa y requiere criterio experto".</br>

