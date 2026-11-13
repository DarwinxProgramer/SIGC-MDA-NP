# Modelo Independiente de Computación (CIM) — SIGC

**Proyecto:** Sistema Integral de Gestión Clínica (SIGC)  
**Fase MDA:** Fase 1 — Computation Independent Model (CIM)  
**Fecha:** 2026-11-13  

---

## 1. Propósito del CIM en la Arquitectura MDA

El **Modelo Independiente de Computación (CIM)** describe el dominio del problema médico-administrativo de la clínica, su vocabulario de negocio, actores, procesos operativos y reglas de negocio **sin ninguna referencia a tecnologías de implementación** (sin mencionar Java, Spring Boot, tablas SQL ni tipos de datos Ecore).

Su objetivo es garantizar la trazabilidad directa entre las necesidades clínicas/administrativas y el **Modelo Independiente de Plataforma (PIM)** definido en `model/sigc.ecore` y `model/sigc.ocl`.

---

## 2. Actores del Negocio y Frontera de Seguridad

| Actor | Rol en la Clínica | Frontera de Acceso |
|---|---|---|
| **Administrador** | Personal de dirección y soporte operativo | Gestiona personal médico, especialidades, horarios, inventario, usuarios/roles y auditoría de metadatos. **No tiene acceso al historial clínico ni a consultas médicas (Decisión 10 / RNF-03).** |
| **Recepcionista** | Personal de admisión y atención al paciente | Registra y actualiza pacientes, verifica disponibilidad de agenda y programa/reprograma/cancela citas médicas. |
| **Médico** | Profesional de la salud | Consulta el historial clínico del paciente, atiende citas programadas y registra consultas, diagnósticos, tratamientos, prescripciones, alergias y antecedentes. |

---

## 3. Catálogo de Procesos de Negocio (P-01 a P-05)

| Código | Proceso | Actores | Reglas de Negocio Aplicadas | Diagrama PlantUML |
|---|---|---|---|---|
| **P-01** | Registro de Paciente e Inicialización de Historial | Recepcionista, Sistema | `RN-01` (ID único), `RN-02` (1 Historial por Paciente), `RF-20` (Auditoría) | [02_actividad_P01_registrar_paciente.puml](diagramas/02_actividad_P01_registrar_paciente.puml) |
| **P-02** | Agendamiento de Cita Médica | Recepcionista, Sistema | `RN-03`, `RN-04` y `RN-05` (No solapamiento + margen 30 min), `RN-06` (Especialidad autorizada) | [03_actividad_P02_agendar_cita.puml](diagramas/03_actividad_P02_agendar_cita.puml) |
| **P-03** | Atención de Consulta Médica | Médico, Sistema | `RN-07`, `RN-08`, `RN-09`, `RN-11` (Auditoría clínica), `RN-12`, `RF-23` (Inmutabilidad previa) | [04_actividad_P03_consulta_medica.puml](diagramas/04_actividad_P03_consulta_medica.puml) |
| **P-04** | Gestión de Inventario y Movimientos | Administrador, Sistema | `RN-10` (Salidas no superan existencias), `RF-17` (Alertas de stock mínimo) | [05_actividad_P04_inventario.puml](diagramas/05_actividad_P04_inventario.puml) |
| **P-05** | Autenticación y Control de Acceso Multirrol | Usuario Interno, Sistema | `RNF-01`, `RNF-02`, Decisión 4 (`Usuario *—* Rol`), Decisión 10 (Bloqueo clínico a `ADMIN`) | [06_actividad_P05_autenticacion.puml](diagramas/06_actividad_P05_autenticacion.puml) |

---

## 4. Artefactos Gráficos del CIM (`CIM/diagramas/`)

1. **[01_casos_uso.puml](diagramas/01_casos_uso.puml):** Mapa funcional de los 7 módulos del sistema y sus relaciones `<<include>>` (autenticación) y `<<extend>>` (auditoría).
2. **[02_actividad_P01_registrar_paciente.puml](diagramas/02_actividad_P01_registrar_paciente.puml):** Flujo de admisión con control de duplicados y composición obligatoria `Paciente 1—1 HistorialClinico`.
3. **[03_actividad_P02_agendar_cita.puml](diagramas/03_actividad_P02_agendar_cita.puml):** Validación triple en agendamiento (especialidad del médico, agenda del médico con 30 min de margen y agenda del paciente).
4. **[04_actividad_P03_consulta_medica.puml](diagramas/04_actividad_P03_consulta_medica.puml):** Flujo clínico completo desde `PENDIENTE` → `EN_PROCESO` → `FINALIZADA` con trazabilidad de auditoría.
5. **[05_actividad_P04_inventario.puml](diagramas/05_actividad_P04_inventario.puml):** Control de entradas y salidas de medicamentos e insumos unificados.
6. **[06_actividad_P05_autenticacion.puml](diagramas/06_actividad_P05_autenticacion.puml):** Flujo de autenticación, consolidación de roles y segregación de funciones.
7. **[07_estado_cita.puml](diagramas/07_estado_cita.puml):** Ciclo de vida formal de una `Cita` (`PENDIENTE`, `EN_PROCESO`, `FINALIZADA`, `CANCELADA`) y transiciones inválidas (`RN-12`).
8. **[08_clases_conceptual.puml](diagramas/08_clases_conceptual.puml):** Modelo conceptual de dominio con las entidades y enumeraciones del negocio previo al metamodelado Ecore.

---

## 5. Matriz de Trazabilidad CIM → PIM / OCL

| Regla / Decisión CIM | Concepto de Dominio | Representación en PIM (`sigc.ecore`) | Restricción Formal (`sigc.ocl`) |
|---|---|---|---|
| **RN-01** | Identificación única de paciente | `Paciente.identificacion : EString` | `RN01_IdentificadorUnico` |
| **RN-02 / Dec. 1** | Un historial clínico por paciente | `Paciente.historial` (`containment=true`, `1..1`) | `RN02_UnHistorialPorPaciente` |
| **RN-03** | Cita vinculada a paciente y médico | `Cita.paciente` y `Cita.medico` | `RN03_CitaConPacienteYMedico` |
| **RN-04 / Dec. 2** | No solapamiento en agenda médica | `Cita.fechaHoraInicio` / `fechaHoraFin` | `RN04_NoSolapamientoMedico` |
| **RN-05** | No solapamiento en agenda de paciente | `Paciente.citas` (`0..*`) | `RN05_NoSolapamientoPaciente` |
| **RN-06** | Médico autorizado en especialidad | `Medico.especialidades` (`*..*`) | `RN06_EspecialidadAutorizadaMedico` |
| **RN-07** | Consulta con paciente y médico | `Consulta.paciente` y `Consulta.medico` | `RN07_ConsultaConPacienteYMedico` |
| **RN-08** | Diagnóstico vinculado a consulta | `Consulta.diagnosticos` (`containment=true`) | `RN08_DiagnosticoAsociadoAConsulta` |
| **RN-09** | Prescripción por médico activo | `Prescripcion.medico` y `Medico.activo` | `RN09_PrescripcionPorMedicoAutorizado` |
| **RN-10** | Salida no supera stock disponible | `ProductoInventario.stockActual` y `MovimientoInventario.cantidad` | `RN10_SalidaNoExcedeStockActual` / `RN10_StockNoNegativo` |
| **RN-12 / Dec. 3** | Cita cancelada no puede ser atendida | `Cita.estado : EstadoCita` y `Cita.consulta` | `RN12_CitaCanceladaNoAtendida` |
| **Dec. 7** | Unificación medicamento/insumo | `ProductoInventario.tipo : TipoProducto` | Enumeración `TipoProducto` |
| **Dec. 9** | Baja lógica universal | Atributo `activo : EBoolean = true` en todas las clases | Filtrado lógico en invariantes |
