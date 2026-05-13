# Diario de IA - Sistema Huellas Seguras

## Herramienta utilizada
Claude (Anthropic) - claude.ai

---

## Prompts principales utilizados

**1. Configuración del entorno**
Le pedí ayuda para instalar y configurar Oracle APEX en un Mac con Apple Silicon, ya que Oracle Database no tiene versión nativa para macOS.

**2. Creación de las tablas**
Le pedí que me generara el script SQL para crear las tablas del sistema. El primer resultado usaba secuencias separadas, pero le pedí que lo simplificara usando GENERATED ALWAYS AS IDENTITY, que es más moderno y limpio.

**3. Lógica PL/SQL**
Le pedí el trigger de auditoría y el package PKG_PROTECTORA con la función de validación y el procedimiento de adopción.

**4. Aplicación APEX**
Me fue guiando paso a paso para crear la app, añadir las páginas y configurarlas con las tablas correspondientes.

---

## Cambios realizados al código sugerido

- **VARCHAR por VARCHAR2**: la IA inicialmente usó VARCHAR en algunos tipos de datos. Lo corregí a VARCHAR2, que es el tipo correcto en Oracle.
- **Secuencias**: el primer script usaba secuencias separadas (CREATE SEQUENCE). Lo simplifiqué usando GENERATED ALWAYS AS IDENTITY directamente en la definición de la tabla.
- **Foreign Keys**: la IA usó REFERENCES directamente en la columna. Lo cambié a CONSTRAINT ... FOREIGN KEY ... REFERENCES para que fuera más claro y correcto.
- **Ejecución en APEX**: SQL Commands solo permite ejecutar una sentencia a la vez, así que tuve que dividir el script y ejecutar cada tabla por separado en lugar de todo de golpe.

---

## Reflexión

La IA fue útil para generar la estructura base del código, pero requirió revisión y corrección en varios puntos. El más importante fue entender que Oracle tiene particularidades propias (como VARCHAR2 en vez de VARCHAR) que la IA no siempre respeta. También fue necesario adaptar los scripts al entorno real de Oracle APEX, que tiene limitaciones a la hora de ejecutar múltiples sentencias a la vez.
