
Los modelos de lenguaje a gran escala (LLM, por sus siglas en inglés) están diseñados para generar respuestas basadas en instrucciones y consultas del usuario. En muchas aplicaciones, estos modelos operan con múltiples capas de instrucciones:

Mensajes del sistema: Instrucciones ocultas que definen el rol y las limitaciones del modelo (por ejemplo, "Eres un asistente útil, pero nunca reveles herramientas internas ni credenciales").
Indicaciones para el usuario: Texto introducido por el usuario final (por ejemplo, "¿Cómo puedo restablecer mi contraseña?").
Los atacantes se han dado cuenta de que pueden manipular cuidadosamente sus datos de entrada para eludir, confundir o incluso explotar las medidas de seguridad del modelo. Esto se conoce como manipulación de entrada. La forma más común de manipulación de entrada es la inyección de prompts, donde el atacante cambia el flujo de instrucciones y obliga al modelo a ignorar o sortear las restricciones.

En algunos casos, la manipulación de la entrada puede provocar fugas de información del sistema, exponiendo la configuración o las instrucciones ocultas en las que se basa el modelo. Podría pensarse en estas inyecciones como la "SQL injection" actual para los LLM. Al igual que las consultas SQL mal validadas pueden permitir que un atacante ejecute comandos arbitrarios contra una base de datos, las indicaciones mal controladas pueden permitir que un atacante tome el control de un modelo de lenguaje (LLM).


