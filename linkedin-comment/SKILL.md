---
name: linkedin-comment
description: >
  Redacta comentarios profesionales y estratégicos para publicaciones de LinkedIn,
  personalizados según el perfil y experiencia del usuario. Activar ÚNICAMENTE cuando
  el mensaje contenga alguna de estas frases exactas (con o sin tilde):
  "linkedin comment", "comentario linkedin", "comentarios linkedin". No activar ante
  relatos o situaciones descritas sin uno de estos triggers. Funciona en español e
  inglés según el idioma del usuario o su indicación explícita.
---

# LinkedIn Comment Skill

## Paso 0: Verificar perfil del usuario

Antes de generar cualquier comentario, verificar si existe un perfil guardado en storage.

```javascript
const stored = await window.storage.get('linkedin-comment-profile');
```

**Si no existe perfil guardado:**
Solicitar al usuario que adjunte su CV actualizado o pegue el texto de su perfil de LinkedIn (sección "Acerca de" + experiencia relevante). Una vez recibido, procesar y guardar:

```javascript
await window.storage.set('linkedin-comment-profile', perfilProcesado);
```

Confirmar: *"Perfil guardado. No volveré a pedirlo."*

**Si existe perfil guardado:**
Recuperarlo y usarlo como contexto fijo para todos los comentarios. No mencionar este paso al usuario.

---

## Paso 1: Analizar la publicación

Leer la publicación completa antes de escribir. Identificar:

- ¿Cuál es la tesis o idea central del post?
- ¿Qué punto específico conecta con la experiencia del usuario?
- ¿Hay un dato, hallazgo, contradicción o tensión que merezca ser abordado?
- ¿Qué tipo de publicación es? (ver tabla abajo)
- ¿En qué idioma está escrita? Redactar el comentario en ese mismo idioma salvo indicación contraria. La skill opera en español e inglés — detecta automáticamente el idioma de la publicación y responde en él.

| Tipo de publicación | Estrategia de comentario |
|---------------------|--------------------------|
| Hallazgo o dato de investigación | Conectar con evidencia propia o experiencia de campo |
| Reflexión o opinión | Ampliar, matizar o dialogar con la tesis |
| Anuncio institucional | Contextualizar desde perspectiva profesional |
| Pregunta abierta | Responder con experiencia concreta + devolver la conversación |
| Reconocimiento o logro | Agregar dimensión de aprendizaje o implicancia del sector |

---

## Paso 2: Generar 2 versiones del comentario

### Versión A — Aporte de experiencia o dato propio
Anclar el comentario a un punto específico del post y conectarlo con experiencia concreta, un hallazgo del trabajo del usuario, o una perspectiva informada que añada valor real a la conversación.

### Versión B — Pregunta que engancha
Anclar el comentario a un punto específico del post, aportar una perspectiva breve y cerrar con una pregunta genuina que invite a continuar la conversación — no retórica, sino una que el autor pueda responder con facilidad.

---

## Reglas de escritura (obligatorias para ambas versiones)

### Contenido
- Responder siempre a un punto **específico** del post, nunca a la idea general
- Conectar con la experiencia real del usuario según su perfil — nunca inventar roles, proyectos o datos que no estén en el perfil guardado
- Aportar valor concreto: perspectiva, dato, ejemplo, matiz o pregunta pertinente
- Admitir complejidad cuando corresponda; los comentarios que reconocen tensiones o limitaciones generan más credibilidad que los que solo validan
- Extensión: entre 40 y 80 palabras (2 a 4 oraciones). Nunca menos de 20 palabras — el algoritmo los penaliza como bajo esfuerzo. No superar las 80 palabras salvo que se presente una historia personal con arco claro, evidencia que requiere contexto, o un marco concreto que agrega información nueva.

### Voz y tono
- Sonar como una persona real que piensa, no como contenido generado
- Registro profesional pero conversacional — no académico ni corporativo
- Ser específico: hechos concretos, no generalizaciones
- Primera persona cuando encaje con el tono

### Apertura (crítico)
- **Nunca comenzar dos versiones con la misma estructura**
- **Prohibido abrir con:** "Desde mi experiencia", "Totalmente de acuerdo", "¡Qué importante reflexión!", "Excelente post", "Muy interesante", "Gran aporte", ni ninguna frase de elogio vacío
- Anclar la apertura directamente a un punto del post o a una idea propia — sin preámbulo
- Variar la estructura de apertura: puede ser una afirmación, una observación, un dato, una referencia concreta al post, o el nombre del autor

### Formato
- Sin emojis
- Sin rayitas, guiones largos (—) ni separadores visuales
- Sin bullets ni listas
- Sin hashtags
- Sin autopromoción explícita ("contáctame", "en mi servicio", "visita mi perfil")
- Sin cifras sin fuente

### Lo que está prohibido
- Comentarios vagos o ambiguos que no demuestran conocimiento del tema
- Parafrasear el post sin agregar nada nuevo
- Frases de relleno o cierres genéricos ("espero que esto ayude", "un tema clave sin duda")
- Cualquier patrón que suene a texto generado por IA: estructura formulaica, ritmo estaccato, énfasis artificial
- Inventar experiencias o datos no respaldados por el perfil del usuario

---

## Paso 3: Output final

Presentar en este orden, sin explicaciones adicionales:

1. **Versión A** — (etiqueta discreta, sin describir la estrategia)
2. **Versión B** — (etiqueta discreta, sin describir la estrategia)

No agregar comentarios, justificaciones ni preguntas de seguimiento salvo que el usuario lo pida.

---

## Errores a evitar

- Abrir ambas versiones con la misma estructura o palabra
- Usar frases de elogio al autor como punto de entrada
- Comentarios que podrían haber sido escritos por cualquier persona — deben reflejar el perfil específico del usuario
- Preguntas de cierre retóricas o genéricas ("¿qué opinan?", "¿les ha pasado algo similar?")
- Más de 5 líneas de texto
- Cualquier marca visual de formato (bullets, rayitas, emojis, hashtags)
