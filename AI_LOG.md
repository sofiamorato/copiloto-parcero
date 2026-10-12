# Registro de uso de IA

**Asistentes usados** (ChatGPT, Claude, Gemini, Copilot, otros) y para qué (ideas, redacción del prompt, análisis de fallos, casos de prueba):

- **Claude Code (Claude Opus y Sonnet, en VS Code):**
  - analizar los JSON de cada corrida caso por caso;
  - proponer y redactar los cambios del prompt (v1 a v8);
  - llevar `eval/registro.md`;
  - escribir los casos propios de `eval/mis_casos.jsonl`;
  - correr las pruebas `--dev`;
  - hacer los borradores de estos documentos.

  Los envíos al set oculto y los commits los hice yo: la IA tenía prohibido hacerlos (ver `CLAUDE.md`).

**Tres prompts que me sirvieron** (pégalos o resúmelos, y di qué salió de cada uno):

1. *"Analiza todo lo que tengo en esta carpeta… hazme un plan de trabajo para ganar, pero no me queda tanto tiempo."* La IA encontró que no había ningún envío oculto, que el encabezado de `system_prompt.md` tenía el comentario `<!--` sin cerrar (en GitHub ocultaba todo el prompt) y que dos "fallos por variancia" eran errores 429.
2. *Reescribir la regla de música palabra por palabra (v3 y v4).* Separar `play_music` de `set_music_volume` y no mandar el número de volumen como canción arregló dev-10 en 3 de 3 corridas.
3. *"Considera lo que ha sucedido… mejorar el texto pero mantener la voz."* De aquí salió el conteo de fallos por caso en 10 corridas, que mostró el ruido de ±3 puntos y los fallos intermitentes que había que atacar.

**Uno que me engañó** (qué pediste, qué te dio, por qué estaba mal, cómo lo detectaste):

En v4 le pedí analizar por qué dev-17 seguía fallando. Concluyó que era "un patrón de alineamiento del modelo más fuerte que cualquier instrucción" y recomendó dejar de intentarlo. En la misma revisión atribuyó a variancia dos fallos que eran `HTTP 429` del servidor. Ninguna de las dos conclusiones era cierta: dev-17 se arregló con un glosario de cuatro líneas que traduce las claves en inglés del estado (`fog_lights` = antiniebla). Lo detecté al pedirle que releyera el campo `why` completo y el formato real de `{{vehicle_context}}` en `cpbench/vehicle.py`.

**Porcentaje aproximado del texto final del system prompt que escribió la IA sin cambios:** 80%

La IA redactó gran parte de las instrucciones, pero yo revisé las propuestas, identifiqué qué cambios funcionaban, decidí cuáles conservar y realicé las pruebas y los envíos finales.

Opcional: exporta las conversaciones a `logs/`.
