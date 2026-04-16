+++
title = "EcoBici Collector: MLOps en GCP y Supabase"
date = "2026-04-15"
slug = "ecobici-collector"
description = "Pipeline automatizado para recolectar datos de EcoBici y predecir disponibilidad."
tags = ["Machine Learning", "MLOps", "GCP", "Supabase", "GitHub Actions"]
categories = ["Proyectos"]
readingtime = 4
+++

He construido y publicado **[EcoBici Collector](https://github.com/jmtoral/ecobici-collector)**, un pipeline automatizado para recolectar datos de disponibilidad de bicicletas en EcoBici (CDMX) y entrenar un modelo de clasificación. El objetivo es simple: estimar la probabilidad de encontrar al menos una bicicleta funcional en una estación dada, a cualquier hora del día y día de la semana.

### ¿El problema con la programación convencional?

El proyecto inició usando los *cron jobs* de **GitHub Actions** como único programador (*scheduler*). Aunque funcionaba, rápidamente me topé con la realidad: las colas compartidas en GitHub provocan retrasos inconsistentes de entre 5 y 30 minutos. Para un pipeline de recolección de datos que necesita coherencia temporal (cada 15 minutos exactos), esta variabilidad destruía la integridad del dataset.

### La solución: Arquitectura de "Dual Trigger"

Para garantizar precisión sin sacrificar redundancia, rediseñé la arquitectura hacia un enfoque híbrido:

- **Google Cloud Scheduler (Principal)**: Ejecuta una solicitud HTTP POST cada 15 minutos con precisión de segundos.
- **Supabase (Backend)**: Sirve como base de datos PostgreSQL y Edge Function para recibir la invocación y persistir los datos (snapshots).
- **GitHub Actions (Respaldo y Mantenimiento)**: Se encarga de reentrenar semanalmente el modelo usando los datos de Supabase, generar nuevas versiones (`.pkl`) y funge como disparador de respaldo por si el Scheduler de GCP llega a fallar.

### El Modelo

Recolectar millones de filas de GBFS no sirve de mucho si no se traducen en un producto analítico. Cada domingo a las 3:00 am, el cluster se despierta, extrae la historia completa y entrena un modelo que utiliza características temporales (*sine/cosine embeddings* de horas y días) y geográficas para responder la gran pregunta: *"Si voy a la estación X un martes a las 8am, ¿qué tan probable es que logre llevarme una bici?"*

El stack demuestra cómo diseñar sistemas escalables casi gratuitos: combinando el tier libre de GCP, Supabase y Streamlit Community Cloud para el dashboard final.

[**Puedes ver el código completo y la arquitectura en mi repositorio de GitHub.**](https://github.com/jmtoral/ecobici-collector)
