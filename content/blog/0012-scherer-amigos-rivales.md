+++
date = "2025-04-14"
title = "Amigos, rivales y todo lo demás: mapeando el universo político de Julio Scherer Ibarra con IA"
slug = "scherer-amigos-rivales-ia"
tags = ["Ciencia de datos", "Inteligencia Artificial", "Política", "NLP", "R", "Python"]
categories = ["Proyectos"]
[params]
  metadescription = "Cómo usé 15 agentes de IA para analizar las memorias de Julio Scherer Ibarra y construir un mapa interactivo de sus relaciones políticas."
  metakeywords = "Scherer, política mexicana, NLP, LLM, análisis de texto, ciencia de datos"
+++

Cuando Julio Scherer Ibarra publicó *Ni venganza ni perdón* en 2025, nadie esperaba que sus memorias se convirtieran en un documento político de primer orden. El libro —denso, cargado de nombres, fechas y afectos— describe casi tres décadas de una vida pública intensa: consejero jurídico de la Presidencia, operador político, testigo privilegiado de la Cuarta Transformación.

La pregunta que me hice al leerlo fue sencilla: **¿a quién quiere Scherer y a quién no?** Y si eso era respondible, ¿podía hacerse de forma sistemática?

La respuesta fue sí. Y el proceso es lo que quiero compartir aquí.

## El problema: 300 páginas, cientos de personajes

El libro tiene cerca de 300 páginas y menciona a decenas de figuras públicas: presidentes, ministros, gobernadores, periodistas, empresarios. Hacer un análisis manual sería posible pero lento y subjetivo. Usar un LLM en una sola pasada era inviable por los límites de contexto.

La solución fue diseñar una **arquitectura de procesamiento paralelo**: dividir el libro en bloques de ~20 páginas y asignar cada fragmento a un "agente" —una instancia específica de análisis— con instrucciones precisas sobre qué buscar y cómo clasificarlo.

## La metodología: 15 agentes, una escala, un resultado

Cada agente recibía la misma instrucción base (guardada en `prompt.txt`):

1. Identificar **personas reales** mencionadas en el fragmento.
2. Extraer la **cita textual** que mejor captura el sentimiento del autor hacia esa persona.
3. Clasificar la relación en una escala de **-5 (enemigo acérrimo) a +5 (aliado incondicional)**.
4. No inferir: basarse únicamente en lo que el texto dice de forma explícita.

Una vez procesados los 15 bloques, un proceso central consolidó los resultados:
- **Deduplicación** de nombres ("AMLO", "Andrés Manuel", "el Presidente" → mismo nodo).
- **Resolución de conflictos** cuando un personaje aparecía en múltiples fragmentos con puntajes distintos.
- Generación de un Excel con código de colores y el dataset final.

## El resultado: un beeswarm interactivo

El producto final es una visualización tipo *beeswarm plot* donde cada círculo es una persona. El tamaño refleja el **peso narrativo** (páginas dedicadas), la posición horizontal indica el sentimiento, y los antagonistas centrales llevan un contorno punteado para distinguirlos.

Puedes explorarlo directamente en la [GitHub Page del proyecto](https://jmtoral.github.io/scherer-amigos-rivales/) o revisar el código en [el repositorio](https://github.com/jmtoral/scherer-amigos-rivales).

## Lo que aprendí

Algunos hallazgos del análisis me sorprendieron. Ramírez Cuevas y Gertz Manero emergieron como los antagonistas más marcados —no por su frecuencia de mención sino por la intensidad del lenguaje utilizado. AMLO, en cambio, aparece con una relación ambivalente: admiración genuina combinada con decepción en momentos clave.

Lo que más me llamó la atención fue que el libro no es un ajuste de cuentas. Es, en realidad, un intento de ordenar la experiencia. Scherer escribe como quien hace un inventario emocional antes de cerrar un capítulo.

## Sobre el uso de IA

Este proyecto, incluyendo el código de extracción y la visualización, fue asistido por agentes de IA —específicamente Google DeepMind y Claude Code. Los uso como herramientas para acelerar el proceso analítico, no como sustitutos del criterio o la interpretación. La pregunta, el diseño metodológico y las conclusiones son mías.

El código está disponible en GitHub bajo licencia MIT. Si tienes preguntas o quieres replicar el análisis con otro texto, me puedes encontrar en [LinkedIn](https://www.linkedin.com/in/josetoral/).
