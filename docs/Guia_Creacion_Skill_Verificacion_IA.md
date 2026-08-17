Guía Avanzada: Skill de Verificación
Socrática para IA

Esta guía detalla la arquitectura técnica y el diseño de prompts para crear una 'Skill' o agente
especializado en auditar respuestas de modelos de lenguaje utilizando el método socrático.

1. Arquitectura de la Skill

Para que la verificación sea efectiva, la skill debe operar de manera externa o en una capa de
ejecución aislada mediante tres componentes esenciales:

Componente

Función Técnica

Orquestador (Prompt de
Sistema)

Controla el flujo conversacional. Fuerza a la IA a adoptar el rol de auditor
crítico e imparcial, impidiendo que valide sus propias respuestas anteriores
de forma automática.

Herramienta de Búsqueda
(RAG/Web)

Conecta la skill a internet mediante APIs (Google Custom Search, Tavily o
Perplexity). Proporciona la base de hechos objetivos externos necesarios
para refutar al modelo.

Módulo de Memoria Corta

Almacena las premisas aceptadas y rechazadas durante el diálogo
socrático para identificar contradicciones lógicas en tiempo real.

2. Prompt de Sistema (System Prompt) para la Skill

Copia este prompt exacto en el configurador de tu agente (GPT, Custom Assistant o código
personalizado) para inicializar el entorno socrático de verificación:

System Prompt:
Eres un Auditor Socrático de Hechos. Tu único objetivo es verificar la veracidad de
las afirmaciones intercambiadas en el chat aplicando un examen crítico estricto.
Sigues estas reglas:
1. Cuando el usuario presente una respuesta o afirmación previa de una IA, NO la
aceptes como verdadera.
2. Formula una secuencia de preguntas breves que expongan posibles
contradicciones, lagunas o sesgos en la afirmación.
3. Paralelamente, utiliza tu herramienta de búsqueda web para contrastar las
respuestas con fuentes académicas u oficiales.
4. Si detectas una discrepancia empírica, no des la respuesta directamente; guía
al usuario con una pregunta socrática (ej. 'Si la fuente histórica X menciona que
los recursos eran escasos, ¿qué base sustenta que el caballo fuera de lujo?').
5. Mantén un tono neutral, analítico y estrictamente basado en evidencia.

3. Ejemplo de Script para Integración Técnica (Python API)

Si construyes la skill mediante código utilizando la API de un proveedor (como OpenAI o Anthropic),
debes forzar el uso de herramientas de búsqueda (*Function Calling*) para asegurar la triangulación
externa:

import openai

def verificar_afirmacion_socrática(texto_ia, respuesta_usuario):
    # Configuración del agente de auditoría
    response = openai.ChatCompletion.create(
        model='gpt-4o-mini',
        messages=[
            {'role': 'system', 'content': 'Eres un auditor socrático con acceso a
búsqueda web...'},
            {'role': 'user', 'content': f'IA dijo: {texto_ia}. Usuario respondió:
{respuesta_usuario}. Analiza lógicamente.'}
        ],
        tools=[{'type': 'web_search_tool'}] # Forzar triangulación externa
    )
    return response.choices[0].message.content

