# 📌 Sistema de Gestión - Firma Jurídica
Este proyecto presenta el diseño y la modelación de una base de datos relacional orientada a la gestión de expedientes, clientes y procuradores para una firma jurídica. El objetivo principal es garantizar la integridad de la información y la trazabilidad de los asuntos legales.

# 🧾 Interpretación del problema
La firma jurídica enfrenta el reto de centralizar la información dispersa de sus operaciones. Se requiere una solución que permita:

Identificar unívocamente a los clientes y asociarlos con sus respectivos procesos legales.

Gestionar expedientes (asuntos) de manera cronológica, permitiendo conocer su estado actual.

Vincular a los procuradores con los casos que representan, considerando que la naturaleza de los litigios modernos a menudo requiere la colaboración de múltiples especialistas en un mismo asunto.

El sistema busca resolver la redundancia de datos y facilitar la generación de reportes sobre el rendimiento de los procuradores (casos ganados) y el volumen de asuntos por cliente.

# 🧩 Entidades y atributos
Basado en el análisis de dominio, se han definido las siguientes entidades fundamentales:

Cliente: Representa a la persona física que contrata los servicios de la firma.

DNI (Identificador): Clave primaria para garantizar la unicidad.

Nombre: Identificación nominal del sujeto.

Dirección: Ubicación de contacto legal.

Fecha de Nacimiento: Dato necesario para validaciones legales de mayoría de edad.

Asunto: Unidad mínima de gestión legal (expediente).

Número de Expediente (Identificador): Código alfanumérico único para cada proceso.

Fecha de Inicio y Fecha de Finalización: Atributos para el control de plazos y términos.

Estado: Indica la etapa actual (ej. En trámite, Archivado, Sentenciado).

Procurador: Representante legal encargado de la tramitación de los asuntos.

DNI (Identificador): Clave primaria.

Nombre y Apellidos: Identificación del profesional.

Número de Colegiado: Atributo que valida la facultad legal para ejercer.

Casos Ganados: Métrica de desempeño profesional.

# 🔗 Relaciones y cardinalidades
El modelo se rige por las siguientes reglas de negocio:

Cliente → Asunto (1:N): * Un cliente puede tener registrados múltiples asuntos legales en la firma (por ejemplo, un caso civil y uno laboral simultáneamente).

Sin embargo, cada asunto pertenece estrictamente a un único cliente responsable.

Procurador ↔ Asunto (N:M):

Un asunto legal puede ser gestionado por un equipo de varios procuradores para garantizar una defensa integral.

A su vez, un procurador puede estar a cargo de múltiples asuntos de manera simultánea. Esta relación requiere una tabla intermedia para su correcta implementación en el modelo lógico.

# 🗂 Modelo conceptual (DER)
El diagrama entidad-relación (ubicado en la carpeta /diagramas) visualiza la estructura semántica del sistema:

Utiliza la notación de Patas de Gallo (Crow's Foot) para representar la cardinalidad.

Muestra las entidades como rectángulos y las relaciones como rombos o líneas con conectores específicos.

Se observa claramente el flujo de datos: desde el cliente hacia sus asuntos, y la interconexión bidireccional entre asuntos y procuradores.

# 🧱 Modelo lógico relacional
La implementación lógica (disponible en la carpeta /modelo_logico) traduce los conceptos en tablas normalizadas:

Tabla Cliente: PK(dni_cliente).

Tabla Asunto: PK(num_expediente), FK(dni_cliente) que referencia a Cliente.

Tabla Procurador: PK(dni_procurador).

Tabla Asunto_Procurador (Tabla Intermedia): PK(num_expediente, dni_procurador). Contiene dos FKs que apuntan a sus respectivas tablas maestras, resolviendo la relación muchos a muchos.

# ⚙️ Transformación del modelo conceptual al lógico
El proceso de ingeniería se realizó siguiendo estos pasos:

Paso 1: Mapeo de Entidades: Cada entidad del DER se convirtió en una tabla independiente con sus respectivos tipos de datos (VARCHAR para identificadores, DATE para fechas, INT para contadores).

Paso 2: Resolución de la relación 1:N: Para conectar al Cliente con el Asunto, se migró la clave primaria del Cliente (dni_cliente) a la tabla Asunto como clave foránea.

Paso 3: Descomposición de la relación N:M: La relación entre Procurador y Asunto no puede implementarse directamente con una FK simple. Se creó la tabla puente Asunto_Procurador que hereda las PKs de ambos para formar una clave primaria compuesta, asegurando que un mismo procurador no sea asignado dos veces al mismo asunto por error.

# 🧠 Decisiones de diseño
Normalización: Se aplicó hasta la Tercera Forma Normal (3FN) para evitar anomalías de inserción y borrado.

Tabla Intermedia: Se decidió crear una tabla física para la relación Procurador-Asunto para permitir, en futuras versiones, añadir atributos descriptivos a la relación (como "rol del procurador" o "horas asignadas").

Nomenclatura: Se utilizó el estándar snake_case para los nombres de campos y tablas, facilitando la compatibilidad con distintos motores de bases de datos SQL.

Integridad Referencial: Se establecieron restricciones de clave foránea con acción ON DELETE RESTRICT para evitar la eliminación de clientes que aún tengan procesos judiciales activos.

##### Nota de documentación: Este diseño cumple con los estándares académicos requeridos para la asignatura de Bases de Datos. Los scripts SQL y diagramas detallados pueden consultarse en las carpetas correspondientes del repositorio.
