# Sistema Huellas Seguras - Especificación de Diseño

## 1. Diccionario de Datos

### Tabla ANIMALES
| Columna | Tipo | Restricciones |
|---|---|---|
| ID_ANIMAL | NUMBER | PK |
| NOMBRE | VARCHAR2(100) | NOT NULL |
| ESPECIE | VARCHAR2(50) | NOT NULL |
| RAZA | VARCHAR2(100) | |
| FECHA_ENTRADA | DATE | NOT NULL |
| ESTADO | VARCHAR2(20) | NOT NULL (Disponible / En tratamiento / Adoptado) |

### Tabla ADOPTANTES
| Columna | Tipo | Restricciones |
|---|---|---|
| ID_ADOPTANTE | NUMBER | PK |
| NOMBRE | VARCHAR2(150) | NOT NULL |
| DNI | VARCHAR2(20) | NOT NULL, UNIQUE |
| EMAIL | VARCHAR2(200) | NOT NULL, UNIQUE |
| TELEFONO | VARCHAR2(20) | |

### Tabla ADOPCIONES
| Columna | Tipo | Restricciones |
|---|---|---|
| ID_ADOPCION | NUMBER | PK |
| ID_ANIMAL | NUMBER | FK → ANIMALES |
| ID_ADOPTANTE | NUMBER | FK → ADOPTANTES |
| FECHA | DATE | NOT NULL |
| OBSERVACIONES | VARCHAR2(500) | |

### Tabla HISTORIAL_MEDICO
| Columna | Tipo | Restricciones |
|---|---|---|
| ID_HISTORIAL | NUMBER | PK |
| ID_ANIMAL | NUMBER | FK → ANIMALES |
| FECHA | DATE | NOT NULL |
| DESCRIPCION | VARCHAR2(1000) | NOT NULL |
| COSTE | NUMBER(10,2) | |

### Tabla LOG_CAMBIOS_ANIMAL (auditoría)
| Columna | Tipo | Restricciones |
|---|---|---|
| ID_LOG | NUMBER | PK |
| ID_ANIMAL | NUMBER | NOT NULL |
| ESTADO_ANTERIOR | VARCHAR2(20) | |
| ESTADO_NUEVO | VARCHAR2(20) | NOT NULL |
| FECHA_CAMBIO | DATE | NOT NULL |
| USUARIO_BD | VARCHAR2(100) | NOT NULL |

---

## 2. Lógica de Negocio

- Un animal solo se puede adoptar si su estado es "Disponible". Si está "En tratamiento" o ya "Adoptado", el sistema da error.
- Cuando se hace una adopción, se inserta el registro y se cambia el estado del animal a "Adoptado" en la misma transacción. Si algo falla, se hace ROLLBACK.
- Cada vez que cambia el estado de un animal, un trigger guarda en el log quién lo cambió, cuándo y de qué estado a qué estado.

---

## 3. Mapa de Navegación (APEX)

- Inicio
- Gestión de Animales: listado + formulario de alta/edición
- Gestión de Adoptantes: listado + formulario de alta/edición
- Página de Adopción: formulario que llama al procedimiento del paquete
- Historial Médico: listado y formulario de registro
- Informes: animales por especie, adopciones por mes, log de cambios
