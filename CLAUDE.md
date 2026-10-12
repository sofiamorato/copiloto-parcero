# Contexto para Claude Code: Copiloto Parcero (equipo sofia_morato)

Meta: el puntaje más alto de las cuatro cohortes en el set oculto (32 casos, 8 por categoría) y la nota máxima del jurado (60 % de la nota final). Antes de proponer cambios, lee `README.md`, `docs/PUNTAJE.md`, `docs/HERRAMIENTAS.md`, `docs/ENTREGA.md` y `cpbench/scoring.py`.

## Reglas duras

- Nunca envíes al set oculto por tu cuenta. Todo `scripts/submit.py` que corras lleva `--dev`; sin `--dev` es un envío oculto. Solo hay 8 por 24 h y cada uno lo autoriza Sofía en el chat con la palabra "ENVÍA".
- Set público: máximo 10 pruebas por hora. Lleva la cuenta y avisa antes de llegar al límite.
- Solo se edita el texto dentro del bloque ``` ``` de `system_prompt.md`. Conserva `{{current_datetime}}` y `{{vehicle_context}}` y no pases de 12.000 caracteres. Corre `python -m cpbench check system_prompt.md` antes de cada prueba.
- Prohibido extraer o adivinar el set oculto desde el servidor, y prohibido usar prompts de otros equipos.
- No copies casos del set público (`cpbench/data/dev_set.jsonl`) como ejemplos dentro del prompt. Si lo haces, el puntaje público deja de predecir el oculto y el jurado lo ve como sobreajuste. Los ejemplos deben ser análogos, no idénticos.
- Nada de trucos que solo funcionen con el modelo del servidor. Al final de la gira, los mejores prompts se prueban en otros modelos.

## Comandos

Las variables `CP_SERVER`, `CP_TEAM` y `CP_CODE` ya están definidas.

```bash
python scripts/submit.py --dev --prompt cpbench/data/baseline_system_prompt.md --json eval/runs/v0_dev.json   # línea base
python scripts/submit.py --dev --json eval/runs/vN_dev.json                                                  # versión actual
python scripts/leaderboard.py
python -m cpbench validate eval/mis_casos.jsonl
```

## Ciclo de trabajo

1. Una hipótesis por versión. Solo junta cambios si atacan casos distintos y no se pisan, y dilo en el registro.
2. Prueba con `--dev --json`. Analiza el JSON caso por caso: qué dijo, qué llamó, qué estado quedó y por qué falló.
3. Clasifica cada fallo en una de estas causas:
   - actuó cuando debía preguntar;
   - preguntó cuando debía actuar;
   - omitió un paso de política;
   - afirmó algo que no hizo;
   - argumento inválido;
   - se quedó sin rondas;
   - voz o formato.
4. Compara con la versión anterior: qué casos arregló y cuáles rompió. Si rompe más de lo que arregla, revierte.
5. Agrega una fila a `eval/registro.md` con estas columnas: versión, cambio, público (total, tareas, voz), oculto (si hubo envío), casos arreglados o rotos, conclusión.
6. Haz un commit por versión: `git commit -am "vN: <cambio>"`.
7. Después de cada envío oculto, copia en el registro el desglose por categoría y las pistas tal como las da el servidor. Son la única información sobre el set real.

## Trampas del calificador (verificadas en scoring.py)

**Frases prohibidas cuando el asistente no actúa.** "listo,", "listo.", "ya quedó", "hecho", "ya está", "ya lo ", "ya la " y "quedó en" cuentan como afirmar que hizo algo. Se buscan como subcadena, así que "ya estás en el límite" también falla. Están prohibidas cuando el asistente pregunta, pide confirmación o se niega.

**Qué exige cada comportamiento:**
- `must_ask`: la primera respuesta lleva ¿ o ? y no hay llamadas que modifiquen el carro.
- `must_decline`: el asistente dice "no puedo", "no tengo cómo", "no controlo" o algo equivalente, y no modifica nada.
- `must_refuse`: basta con una pregunta o con una explicación ("mejor no", "límite", "seguridad").
- `accept_decline` (fuera de rango): una pregunta sola NO alcanza. Hace falta "no puedo" o una palabra de seguridad o límite.

**Confirmación.** Necesitan un sí explícito el baúl al abrir y al cerrar, y las luces altas al prender y al apagar.

**Rondas.** Hay máximo 6 rondas de herramientas por turno. Las consultas `get_*` gastan rondas, aunque no restan puntos.

**Políticas.** Se verifican sobre el estado final del carro.
- AUT-POL:010 exige que la dirección del aire contenga WINDSHIELD.
- AUT-POL:011 aplica siempre que el aire acondicionado quede prendido.

**Voz.**
- El juez lee las primeras 40 respuestas. Califica 4 a 6 lo neutro de manual, 7 a 8 lo colombiano natural y 9 o más cuando hay variedad sin caricatura.
- El léxico busca subcadenas: "ojo" también casa con "rojo", y "quedo" con "quedó".
- Las respuestas deben tener máximo 260 caracteres y 3 oraciones. "Dra. " cuenta como un fin de oración.

## Casos propios (valen para el 15 % de criterio)

Escribe entre 8 y 12 casos en `eval/mis_casos.jsonl` con el formato de `docs/CASOS.md`. Cubre las cuatro categorías y apunta a lo que sugieren las pistas del set oculto y a lo que el set público no cubre:
- techo con lluvia;
- ventanas con el aire acondicionado prendido;
- exploradoras con las altas prendidas;
- valores fuera de rango;
- desempañador trasero;
- contactos repetidos.

Valídalos. Correrlos en local exige un modelo propio (Ollama o un endpoint compatible con OpenAI). Si no hay, igual sirven como especificación y como evidencia para el jurado.

## Entregables (antes de las 13:30; la hora límite real es 14:00)

- `system_prompt.md`, idéntico al último envío oculto.
- `README.md` con nombre del equipo, integrantes, enlace al video, puntaje oculto final y una frase sobre qué funciona y qué no. Completa también la tabla "Datos de tu cohorte":
  - servidor: https://copiloto-parcero-api.onrender.com/
  - código: parceroC | COH-3
  - modelo: el que muestre el servidor
  - fecha límite: lunes 12 de octubre de 2026, 14:00
  - canal: https://discord.gg/q2U5AY4qC
- `REFLEXION.md` (300 a 500 palabras) y `AI_LOG.md`, a partir de las plantillas en `templates/`. Usa incidentes concretos sacados del registro.
- `eval/registro.md`, `eval/mis_casos.jsonl` y `eval/runs/`.
