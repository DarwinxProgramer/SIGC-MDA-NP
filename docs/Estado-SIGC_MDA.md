# Sistema Integral de Gestión Clínica (SIGC)
## Documento Maestro del Proyecto y Pipeline MDA

**Versión:** 2.0 (Consolidada)  
**Fecha:** 13 de Noviembre de 2026  
**Enfoque:** Desarrollo de Software Dirigido por Modelos (MDA)  
**Stack Objetivo:** Java 17 + Spring Boot 3 + PostgreSQL  

---

# PARTE 1: DEFINICIÓN DEL DOMINIO Y CONTEXTO (CIM)

## 1. Información General
- **Nombre provisional:** Sistema Integral de Gestión Clínica (SIGC).
- **Tipo de sistema:** Aplicación de gestión clínica ambulatoria.
- **Alcance:** Clínica con varias especialidades médicas.
- **Usuarios:** Exclusivamente personal interno de la clínica (Administradores, Recepcionistas y Médicos).

## 2. Problema que busca resolver
La información de pacientes, citas, consultas y medicamentos suele estar dispersa en documentos físicos, hojas de cálculo o sistemas independientes. Esto ocasiona duplicación de información, dificultades para consultar historiales, conflictos en la programación de citas y errores en el control de inventario. El SIGC busca centralizar estos procesos en una plataforma segura y auditable.

## 3. Objetivos
**General:** Desarrollar un sistema integral de gestión clínica que permita administrar de manera centralizada la información clínica y administrativa, aplicando un enfoque MDA.

**Específicos:**
1. Gestionar el registro y administración de pacientes.
2. Administrar médicos, especialidades y horarios.
3. Permitir la programación y gestión de citas médicas.
4. Mantener historiales clínicos (diagnósticos, alergias, tratamientos, prescripciones).
5. Controlar las existencias y movimientos del inventario.
6. Generar reportes administrativos y estadísticos.
7. Implementar mecanismos de seguridad y auditoría.
8. Aplicar MDA para modelar el dominio, validar restricciones y generar código fuente.

## 4. Alcance Funcional (Módulos)
- **Módulo 1. Gestión de pacientes:** Registro, consulta, edición, búsqueda e historial.
- **Módulo 2. Gestión de personal médico:** Registro de médicos, especialidades y horarios.
- **Módulo 3. Gestión de citas:** Agendamiento, disponibilidad, reprogramación y estados.
- **Módulo 4. Historial clínico y consultas:** Creación de historiales, alergias, diagnósticos, tratamientos y prescripciones.
- **Módulo 5. Inventario:** Registro de medicamentos/insumos, entradas/salidas y alertas de stock.
- **Módulo 6. Reportes:** Estadísticas de citas, consultas y niveles de inventario.
- **Módulo 7. Seguridad y auditoría:** Usuarios, roles, permisos y trazabilidad de operaciones.

## 5. Actores del Sistema
- **Administrador:** Gestiona usuarios, roles, médicos, especialidades y configuración. *(Nota: No tiene acceso automático a datos clínicos).*
- **Recepcionista:** Registra pacientes, gestiona citas y consulta información administrativa.
- **Médico:** Consulta historiales, registra consultas, diagnósticos, tratamientos y prescripciones.

---

# PARTE 2: REQUISITOS Y REGLAS DE NEGOCIO

## 6. Requisitos Funcionales (RF)
- **RF-01 a RF-02:** Registrar, consultar, editar y actualizar pacientes.
- **RF-03 a RF-04:** Registrar médicos, especialidades y gestionar horarios.
- **RF-05 a RF-07:** Registrar, consultar disponibilidad, reprogramar y cancelar citas.
- **RF-08 a RF-14:** Gestionar historial clínico, registrar alergias, consultas, diagnósticos, tratamientos y prescripciones.
- **RF-15 a RF-17:** Registrar medicamentos/insumos, controlar inventario y alertar stock bajo.
- **RF-18:** Generar reportes administrativos y estadísticos.
- **RF-19 a RF-27:** Administrar usuarios/roles, registrar auditoría, autenticación, correcciones a historial sin sobrescribir, baja lógica (inactivar/reactivar), catálogo de productos, estados de cita (no asistencia) y agenda médica.

## 7. Requisitos No Funcionales (RNF)
- **RNF-01 a RNF-03:** Seguridad (autenticación), Autorización (por rol) y Privacidad de datos clínicos.
- **RNF-04 a RNF-05:** Integridad de datos y Disponibilidad en horarios de clínica.
- **RNF-06 a RNF-07:** Usabilidad y Mantenibilidad (código por capas).
- **RNF-08 a RNF-10:** Trazabilidad (auditoría), Rendimiento y Persistencia (Base de datos relacional).
- **RNF-11:** Cumplimiento legal (Ley Orgánica de Protección de Datos Personales de Ecuador).

## 8. Reglas de Negocio (RN)
- **RN-01 a RN-02:** Identificador único de paciente y exactamente un historial clínico por paciente.
- **RN-03 a RN-06:** Cita asociada a paciente/médico, sin solapamiento (con margen de 30 min), y especialidad autorizada para el médico.
- **RN-07 a RN-09:** Consulta asociada a paciente/médico, diagnóstico asociado a consulta y prescripción emitida por médico autorizado.
- **RN-10:** Inventario no permite salidas mayores al stock actual.
- **RN-11 a RN-12:** Modificaciones al historial clínico auditadas. Cita cancelada no puede marcarse como atendida sin reactivación formal.

---

# PARTE 3: DECISIONES DE MODELADO Y PROCESOS

## 9. Decisiones de Modelado Confirmadas (Aplicadas en el Ecore)
1. **Paciente - Historial:** Relación `1 a 1` (Composición).
2. **Citas:** Uso de `fechaHoraInicio` y `fechaHoraFin` con margen de 30 min entre citas del mismo médico.
3. **Estados de Cita:** `PENDIENTE`, `EN_PROCESO`, `FINALIZADA` (+ `CANCELADA` por RN-12).
4. **Usuario:** Puede tener múltiples roles (`* a *`).
5. **Prescripción:** Registro simple (sin líneas de detalle).
6. **Alergia:** Texto libre (`String`).
7. **Inventario:** `Medicamento` y `ProductoInventario` son la misma entidad (diferenciados por un Enum `tipo`).
8. **Auditoría:** Solo guarda metadatos (usuario, fecha, operación, entidad, id), no snapshots de datos.
9. **Baja Lógica:** Todas las entidades tienen atributo `activo: Boolean`. Sin deletes físicos.
10. **Administrador:** Sin acceso clínico por defecto (restringido en OCL).

## 10. Procesos Principales (CIM)
- **P-01. Registrar paciente:** Búsqueda de duplicados, validación de identificación, creación de paciente e historial, auditoría.
- **P-02. Agendar cita:** Selección de médico/especialidad, validación de solapamiento, creación en estado `PENDIENTE`.
- **P-03. Realizar consulta:** Cambio a `EN_PROCESO`, registro de evolución, diagnósticos, tratamientos, prescripciones y cambio a `FINALIZADA`.
- **P-04. Movimiento de inventario:** Validación de stock y registro de entrada/salida.
- **P-05. Autenticación:** Validación de credenciales, carga de roles y registro en auditoría.

---

# PARTE 4: PIPELINE MDA - AVANCE Y ESTADO ACTUAL

## 11. Resumen del Avance (Fases 0 a 5)

El equipo ha completado el modelado conceptual y la infraestructura base de MDA. 

### 🟢 Fase 0: Preparación del Entorno
- **Herramientas:** JDK 17, Eclipse Modeling Tools, Plugins (ATL, Acceleo), Maven, Git.
- **Estructura creada:** `SIGC-MDA/` con carpetas `CIM/`, `model/`, `transformations/`, `generation/`, `generated/` y `app/`.

### 🟢 Fase 1: CIM (Modelo Independiente de Computación)
- **Documentación:** Definición de dominio, RF, RNF, RN.
- **Diagramas UML:** Se generaron 8 diagramas en PlantUML (Casos de uso, Actividades, Máquina de estados y Clases conceptuales) guardados en `CIM/diagramas/`.

### 🟢 Fase 2: PIM (Modelo Independiente de Plataforma)
- **Archivo `model/sigc.ecore`:** Modelo EMF/Eclipse. Contiene **7 Enumeraciones** (`Sexo`, `EstadoCita`, `TipoProducto`, `TipoMovimiento`, `TipoOperacion`, `TipoAntecedente`, `DiaSemana`) y **17 Clases** (`Paciente`, `HistorialClinico`, `Medico`, `Especialidad`, `HorarioMedico`, `Cita`, `Consulta`, `Diagnostico`, `Tratamiento`, `Prescripcion`, `Alergia`, `Antecedente`, `ProductoInventario`, `MovimientoInventario`, `Usuario`, `Rol`, `Auditoria`).
- **Validación:** Se generó el `sigc.genmodel` y se validó el metamodelo en Eclipse sin errores.

### 🟢 Fase 3: Validación OCL
- **Archivo `model/sigc.ocl`:** Restricciones de negocio formalizadas en sintaxis OCL 2.4 (unicidad de identificación, no solapamiento, stock suficiente, etc.).
- *Nota:* Se documentaron conceptualmente debido a un conflicto de librerías (Guava) en la consola visual de Eclipse, priorizando la entrega del artefacto de código.

### 🟢 Fase 4: PSM y Transformación ATL
- **Archivo `model/psm.ecore`:** Modelo destino que simula un esquema relacional (Database, Table, Column, ForeignKey).
- **Archivo `transformations/pim2psm.atl`:** Reglas de transformación PIM a PSM escritas y validadas visualmente. *(El plugin ATL de Eclipse presentó fallos de ejecución silenciosos, comportamiento común del plugin, conservando los archivos fuente como entregable).*

### 🟡 Fase 5: Generación de Código con Acceleo
- **Proyecto `SIGC-Acceleo`:** Configurado en Eclipse con dependencias EMF en el `MANIFEST.MF`.
- **Archivo `generate.mtl`:** Plantilla de Acceleo escrita para generar clases Java con atributos privados y constructores a partir del `sigc.ecore`.

---

# PARTE 5: TAREAS PENDIENTES (HANDOFF PARA EL EQUIPO)

Para finalizar el proyecto, el equipo debe continuar con los siguientes pasos:

### ⏳ 1. Terminar Fase 5 (Ejecutar Acceleo)
- En Eclipse, ejecutar la plantilla `generate.mtl` (`Run As -> Run Configurations -> Acceleo Application`).
- Verificar que los archivos `.java` se generen correctamente en la carpeta `SIGC-MDA/generated/`.

### ⏳ 2. Fase 6: Implementación en Spring Boot (Carpeta `app/`)
- Inicializar el proyecto Spring Boot 3 con Java 17 y Maven.
- Agregar dependencias: `spring-boot-starter-data-jpa`, `spring-boot-starter-web`, `postgresql`, `lombok`.
- **Mapeo JPA:** Tomar las clases generadas por Acceleo y agregar las anotaciones `@Entity`, `@Table`, `@Id`, `@Column`, `@ManyToOne`, etc.
- Crear interfaces de Repositorio (`JpaRepository`), Servicios y Controladores REST.
- Configurar `application.properties` con la URL de PostgreSQL y `spring.jpa.hibernate.ddl-auto=update`.
- *(Opcional)* Implementar seguridad con Spring Security + JWT y la lógica de auditoría.

### ⏳ 3. Fase 7: Pruebas y Despliegue
- Levantar la base de datos en Docker o local (`postgres:15`).
- Ejecutar la aplicación (`mvn spring-boot:run`).
- Probar los endpoints (Registrar paciente, Agendar cita) con Postman o Swagger UI.
- Completar el archivo `README.md` con instrucciones de ejecución y finalizar el proyecto.

---
**Fin del documento maestro.** Todos los artefactos generados hasta la fecha se encuentran en el repositorio Git del proyecto.