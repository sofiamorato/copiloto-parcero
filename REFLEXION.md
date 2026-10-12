# Reflexión (300 a 500 palabras)

**Equipo:** sofia_morato  
**Integrantes:** Sofía Morato  
**Puntaje final en el set oculto:** [COMPLETAR con el último envío]

## Qué quería lograr

En el taller el asistente sonaba neutro, "de manual", y fallaba en las secuencias de las políticas: prendía las exploradoras sin las bajas o decía que había desempañado sin llamar las herramientas. Quería dos cosas: que sonara colombiano sin caricatura y que convirtiera cada política en pasos concretos. [COMPLETAR: algo que te haya llamado la atención en el taller]

## Qué cambié y qué pasó

1. **Reglas sin contradicciones (v4).** dev-05 ("sube la ventana… del todo") falló en v2 y v3 aunque reforzamos la regla. La causa era una contradicción dentro del prompt: "del todo es 100" chocaba con "subir cierra a 0". Al definir "del todo" según la dirección pedida, el caso pasó 3 de 3 veces.
2. **Glosario del estado (v5).** dev-17 (pedir las altas con las antiniebla prendidas) falló en todas las versiones hasta v4. El estado llega en JSON con claves en inglés (`fog_lights`) y el prompt solo decía "antiniebla". Bastaron cuatro líneas que traducen las claves para que pasara 2 de 2. Antes creíamos que era un "patrón de alineamiento" del modelo, y no lo era.
3. **Principio general de preguntar (v7).** El primer envío oculto (v6) sacó 62,6 frente a ~88 en el público, y Ambiguas quedó en 25 %. Habíamos arreglado frases concretas, no la regla. Con una regla general (si falta cuál, qué valor o a quién, pregunta, aunque el estado parezca resolverlo), Ambiguas subió a 50 % en el oculto.

## Dónde la IA me ayudó y dónde me engañó

Me ayudó a leer los JSON caso por caso y a encontrar el glosario faltante. Me engañó cuando atribuyó a la "variancia del modelo" dos fallos que en realidad eran `HTTP 429` del servidor: el error estaba en el campo `why` y nadie lo leyó completo. Desde entonces separo los fallos de infraestructura antes de comparar versiones.

## Qué haría con un segundo día

Correr cada versión 5 veces o más y comparar fallos por caso, no totales: con el mismo prompt el modelo varía entre 17 y 20 de 20. También montaría un modelo local para correr mis 12 casos propios (`eval/mis_casos.jsonl`), que hoy solo sirven como especificación. [COMPLETAR]

## Qué aprendí sobre los asistentes en el carro, o sobre cómo trabajo yo

Un puntaje alto en el set público no garantizaba nada: ajustar el prompt a 20 casos me costó 25 puntos en el oculto. Lo que sí se transfiere son las reglas generales y que el modelo sepa leer el estado. [COMPLETAR: una frase tuya]
