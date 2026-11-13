# Sistema Integral de Gestión Clínica (SIGC)
## Documento consolidado de contexto, dominio y requisitos para el proceso MDA

**Versión:** 1.0  
**Fecha:** 2026-11-13  
**Enfoque:** Desarrollo de Software Dirigido por Modelos (MDA)

---

## 1. Información general

| Campo | Valor |
|---|---|
| Nombre provisional | Sistema Integral de Gestión Clínica (SIGC) |
| Tipo de sistema | Aplicación de gestión clínica ambulatoria |
| Alcance | Clínica con varias especialidades médicas |
| Usuarios | Exclusivamente personal interno de la clínica |
| Enfoque de desarrollo | Desarrollo de Software Dirigido por Modelos (MDA) |
| Stack objetivo | Java + Spring Boot + PostgreSQL |
| Artefactos MDA | CIM → PIM (Ecore) → validación OCL → PSM (JPA, repositorios, servicios, controladores) → transformación ATL + generación Acceleo |

---

## 2. Descripción general

El **SIGC** será una aplicación informática destinada a gestionar los procesos administrativos y médicos de una clínica. Permitirá registrar y administrar pacientes, profesionales de salud, especialidades médicas, citas y consultas; mantener historiales clínicos; registrar diagnósticos, tratamientos, prescripciones y alergias; controlar el inventario de medicamentos e insumos; y generar reportes administrativos y estadísticos.

El sistema será utilizado por administradores, recepcionistas y médicos, cada uno con permisos según sus responsabilidades. Mantendrá registros de auditoría para garantizar la trazabilidad de las operaciones sobre la información clínica y administrativa.

---

## 3. Problema que busca resolver

La información de pacientes, citas, consultas y medicamentos suele estar dispersa en documentos físicos, hojas de cálculo o sistemas independientes, lo que ocasiona:

- Duplicación o pérdida de información de pacientes.
- Dificultades para consultar historiales médicos.
- Conflictos en la programación de citas.
- Falta de seguimiento de diagnósticos y tratamientos.
- Errores en el control de medicamentos e insumos.
- Dificultades para conocer quién modificó información médica.
- Demoras en la elaboración de reportes administrativos.

El SIGC busca **centralizar estos procesos en una plataforma**, mejorar la organización de la información y facilitar el trabajo del personal de la clínica.

---

## 4. Objetivos

### Objetivo general
Desarrollar un sistema integral de gestión clínica que permita administrar de manera centralizada la información de pacientes, profesionales médicos, citas, consultas, historiales clínicos e inventario, aplicando un enfoque MDA.

### Objetivos específicos
1. Gestionar el registro, consulta, actualización y administración de pacientes.
2. Administrar médicos, especialidades y horarios de atención.
3. Permitir la programación, modificación y cancelación de citas médicas.
4. Mantener historiales clínicos con diagnósticos, alergias, tratamientos y prescripciones.
5. Controlar las existencias y movimientos del inventario de medicamentos e insumos.
6. Generar reportes que faciliten el seguimiento de las actividades de la clínica.
7. Implementar mecanismos de seguridad, control de acceso y auditoría.
8. Aplicar MDA para modelar el dominio, validar restricciones, transformar modelos y generar código fuente.

---

## 5. Alcance funcional (módulos)

### Módulo 1. Gestión de pacientes
- Registrar pacientes con información personal y de contacto.
- Consultar, editar y actualizar sus datos.
- Buscar pacientes por identificación, nombre o código.
- Consultar el historial clínico asociado.

### Módulo 2. Gestión de personal médico
- Registrar médicos y sus datos profesionales.
- Administrar especialidades médicas.
- Asociar médicos con una o varias especialidades.
- Definir horarios y disponibilidad de atención.

### Módulo 3. Gestión de citas
- Agendar citas para pacientes.
- Seleccionar especialidad y médico.
- Consultar disponibilidad.
- Reprogramar y cancelar citas.
- Gestionar estados de las citas.

### Módulo 4. Historial clínico y consultas
- Crear y consultar historiales clínicos.
- Registrar alergias y antecedentes médicos.
- Registrar consultas y diagnósticos.
- Documentar tratamientos e indicaciones.
- Crear prescripciones médicas.
- Consultar la evolución clínica del paciente.

### Módulo 5. Inventario
- Registrar medicamentos e insumos.
- Controlar existencias.
- Registrar entradas y salidas.
- Consultar productos con stock bajo.
- Registrar fechas de vencimiento cuando corresponda.

### Módulo 6. Reportes
- Reportar citas por período, médico y especialidad.
- Consultar pacientes registrados.
- Generar estadísticas de consultas y diagnósticos.
- Consultar niveles de inventario.

### Módulo 7. Seguridad y auditoría
- Gestionar usuarios y roles.
- Restringir operaciones según permisos.
- Registrar accesos y modificaciones relevantes.
- Identificar usuario, fecha, operación y registro afectado.

---

## 6. Actores del sistema

| Actor | Responsabilidades |
|---|---|
| **Administrador** | Gestionar usuarios, roles, médicos, especialidades y configuración. |
| **Recepcionista** | Registrar pacientes, gestionar citas y consultar información administrativa autorizada. |
| **Médico** | Consultar historiales, registrar consultas, diagnósticos, tratamientos y prescripciones. |

> **Nota de seguridad:** el administrador **no** tendrá acceso automático a datos clínicos. Los permisos se definen por función y necesidad de acceso.

---

## 7. Requisitos funcionales (RF)

| Código | Requisito funcional |
|---|---|
| RF-01 | El sistema permitirá registrar pacientes. |
| RF-02 | El sistema permitirá consultar, editar y actualizar pacientes. |
| RF-03 | El sistema permitirá registrar médicos y sus especialidades. |
| RF-04 | El sistema permitirá gestionar horarios de atención médica. |
| RF-05 | El sistema permitirá registrar citas médicas. |
| RF-06 | El sistema permitirá consultar la disponibilidad de médicos. |
| RF-07 | El sistema permitirá reprogramar y cancelar citas. |
| RF-08 | El sistema permitirá gestionar el historial clínico de cada paciente. |
| RF-09 | El sistema permitirá registrar alergias y antecedentes médicos. |
| RF-10 | El sistema permitirá registrar consultas médicas. |
| RF-11 | El sistema permitirá registrar diagnósticos asociados a consultas. |
| RF-12 | El sistema permitirá registrar tratamientos e indicaciones médicas. |
| RF-13 | El sistema permitirá generar prescripciones médicas. |
| RF-14 | El sistema permitirá consultar el historial de consultas de un paciente. |
| RF-15 | El sistema permitirá registrar medicamentos e insumos. |
| RF-16 | El sistema permitirá controlar entradas, salidas y existencias del inventario. |
| RF-17 | El sistema permitirá consultar productos con stock bajo o próximos a vencer. |
| RF-18 | El sistema permitirá generar reportes administrativos y estadísticos. |
| RF-19 | El sistema permitirá administrar usuarios y roles. |
| RF-20 | El sistema registrará operaciones relevantes para fines de auditoría. |
| RF-21 | El sistema permitirá autenticar usuarios (inicio y cierre de sesión). |
| RF-22 | El sistema permitirá consultar la auditoría por usuario, fecha, operación o registro afectado. |
| RF-23 | El sistema permitirá registrar correcciones o enmiendas al historial clínico sin sobrescribir entradas previas. |
| RF-24 | El sistema permitirá inactivar y reactivar pacientes, médicos, usuarios y productos (baja lógica). |
| RF-25 | El sistema permitirá consultar y actualizar el catálogo de medicamentos e insumos. |
| RF-26 | El sistema permitirá gestionar estados de cita, incluida la marcación de no asistencia. |
| RF-27 | El sistema permitirá consultar la agenda de un médico por período. |

---

## 8. Requisitos no funcionales (RNF)

| Código | Requisito |
|---|---|
| RNF-01 | **Seguridad:** el acceso requerirá autenticación. |
| RNF-02 | **Autorización:** cada usuario podrá ejecutar únicamente las operaciones permitidas por su rol. |
| RNF-03 | **Privacidad:** la información clínica deberá protegerse contra accesos no autorizados. |
| RNF-04 | **Integridad:** el sistema deberá mantener consistencia entre pacientes, citas, consultas e historiales. |
| RNF-05 | **Disponibilidad:** la aplicación deberá estar disponible durante los horarios de operación de la clínica. |
| RNF-06 | **Usabilidad:** la interfaz deberá permitir que el personal realice sus tareas de forma clara. |
| RNF-07 | **Mantenibilidad:** el código deberá estar organizado en componentes y capas independientes. |
| RNF-08 | **Trazabilidad:** las operaciones críticas deberán conservar información de auditoría. |
| RNF-09 | **Rendimiento:** las consultas y operaciones habituales deberán responder en tiempos aceptables. |
| RNF-10 | **Persistencia:** los datos deberán almacenarse de forma permanente en una base de datos relacional. |
| RNF-11 | **Cumplimiento legal:** se revisarán las obligaciones aplicables sobre protección de datos personales y datos de salud (Ley Orgánica de Protección de Datos Personales del Ecuador). |

---

## 9. Reglas de negocio (RN)

| Código | Regla de negocio |
|---|---|
| RN-01 | Cada paciente deberá tener un identificador único. |
| RN-02 | Un paciente tendrá **exactamente un** historial clínico. |
| RN-03 | Una cita deberá estar asociada a un paciente y a un médico. |
| RN-04 | Un médico no podrá tener dos citas confirmadas que se superpongan. |
| RN-05 | Un paciente no podrá tener dos citas confirmadas que se superpongan. |
| RN-06 | La cita deberá corresponder a una especialidad que el médico esté autorizado a atender. |
| RN-07 | Una consulta deberá estar asociada a un paciente y a un médico responsable. |
| RN-08 | Un diagnóstico deberá estar asociado a una consulta. |
| RN-09 | Una prescripción deberá ser emitida por un médico autorizado. |
| RN-10 | El inventario no podrá registrar salidas superiores a las existencias disponibles. |
| RN-11 | Toda modificación relevante del historial clínico deberá quedar auditada. |
| RN-12 | Una cita cancelada no podrá registrarse como atendida sin un procedimiento válido de corrección o reactivación. |

> **Criterio adicional de diseño:** el historial clínico no se sobrescribe. Las correcciones se registran como nuevas entradas auditadas, preservando siempre el registro anterior.

---

## 10. Decisiones de modelado confirmadas

| # | Decisión | Impacto en el modelo |
|---|---|---|
| 1 | **Un paciente tiene exactamente un HistorialClinico.** | `Paciente 1 —— 1 HistorialClinico` (composición). OCL: `context Paciente inv unHistorial: self.historial->size() = 1`. |
| 2 | **Cita con `fechaHoraInicio` y `fechaHoraFin`, con margen de 30 minutos entre citas del mismo médico.** | Atributos en `Cita`; restricción OCL de no solapamiento incluyendo el margen de 30 min. |
| 3 | **Estados de Cita: `PENDIENTE`, `EN_PROCESO`, `FINALIZADA` (+ `CANCELADA` por RN-12).** | Enumeración `EstadoCita` con transiciones válidas modeladas como OCL. |
| 4 | **Usuario puede tener varios roles.** | `Usuario * —— * Rol` (relación muchos a muchos). |
| 5 | **Prescripción es un registro simple (no tiene líneas).** | `Prescripcion` como entidad plana, sin `LineaPrescripcion`. |
| 6 | **Alergia es texto libre.** | `Alergia.descripcion : String` (no hay catálogo). |
| 7 | **`Medicamento` y `ProductoInventario` son la misma entidad.** | Se unifica como `ProductoInventario` (con atributo `tipo: MEDICAMENTO | INSUMO`). |
| 8 | **Auditoría guarda solo metadatos.** | `Auditoria`: usuario, fecha/hora, operación, entidad afectada, id del registro. Sin valores anteriores/posteriores. |
| 9 | **Solo baja lógica.** | Todas las entidades principales tendrán atributo `activo : Boolean`. No hay DELETE físico. |
| 10 | **El Administrador no tiene acceso clínico por defecto.** | Restricción OCL sobre permisos por rol; el rol ADMIN solo accede a funciones administrativas. |

### Estados de Cita y transiciones válidas

| Estado Origen | Estado Destino | Condición / Regla |
|---|---|---|
| `[*]` (Creación) | `PENDIENTE` | Estado inicial al agendar una cita (`RF-05`, Decisión 3). |
| `PENDIENTE` | `PENDIENTE` | Reprogramación de horario validando margen de 30 minutos (`RF-07`, Decisión 2). |
| `PENDIENTE` | `EN_PROCESO` | El médico inicia la consulta médica (`RF-10`). |
| `PENDIENTE` | `CANCELADA` | Cancelación administrativa o por no asistencia (`RF-07`, `RF-26`). |
| `EN_PROCESO` | `FINALIZADA` | Cierre de la consulta médica tras registrar evolución y auditoría (`RN-11`). |
| `CANCELADA` | `EN_PROCESO` / `FINALIZADA` | **Transición prohibida (`RN-12`)**: requiere reactivación formal auditada o nueva cita. |

---

## 11. Procesos principales del negocio (CIM)

- **P-01. Registro de paciente:** Búsqueda previa por identificación (`RN-01`), validación de duplicados, alta de `Paciente`, creación automática `1:1` de `HistorialClinico` (`RN-02`) y registro en `Auditoria`.
- **P-02. Agendamiento de cita:** Selección de paciente, especialidad y médico; validación de especialidad autorizada (`RN-06`) y no solapamiento con margen de 30 minutos (`RN-04`, `RN-05`, Decisión 2); creación en estado `PENDIENTE`.
- **P-03. Atención de consulta médica:** Transición `PENDIENTE` → `EN_PROCESO`, registro de `Consulta`, `Diagnostico`, `Tratamiento`, `Prescripcion`, alergias/antecedentes sin sobrescritura (`RF-23`), auditoría (`RN-11`) y cierre en `FINALIZADA`.
- **P-04. Movimiento de inventario:** Registro de entradas y salidas sobre `ProductoInventario` validando existencias suficientes en salidas (`RN-10`) y alertas de stock mínimo (`RF-17`).
- **P-05. Autenticación y control de acceso:** Validación de credenciales y estado activo, carga de múltiples roles (`Usuario * — * Rol`, Decisión 4), restricción de acceso clínico al rol `ADMIN` (Decisión 10) y registro en `Auditoria`.

---

## 12. Entidades del dominio (PIM / Ecore)

### Clases principales (`EClass`)
1. `Paciente`
2. `HistorialClinico`
3. `Medico`
4. `Especialidad`
5. `HorarioMedico`
6. `Cita`
7. `Consulta`
8. `Diagnostico`
9. `Tratamiento`
10. `Prescripcion`
11. `Alergia`
12. `Antecedente`
13. `ProductoInventario`
14. `MovimientoInventario`
15. `Usuario`
16. `Rol`
17. `Auditoria`

### Enumeraciones (`EEnum`)
1. `Sexo` (`MASCULINO`, `FEMENINO`, `OTRO`)
2. `EstadoCita` (`PENDIENTE`, `EN_PROCESO`, `FINALIZADA`, `CANCELADA`)
3. `TipoProducto` (`MEDICAMENTO`, `INSUMO`)
4. `TipoMovimiento` (`ENTRADA`, `SALIDA`)
5. `TipoOperacion` (`CREAR`, `ACTUALIZAR`, `CANCELAR`, `CONSULTAR`, `ELIMINAR_LOGICO`, `AUTENTICAR`)
6. `TipoAntecedente` (`FAMILIAR`, `PERSONAL`, `QUIRURGICO`, `OTRO`)
7. `DiaSemana` (`LUNES`, `MARTES`, `MIERCOLES`, `JUEVES`, `VIERNES`, `SABADO`, `DOMINGO`)

