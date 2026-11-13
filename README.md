# Sistema Integral de Gestión Clínica (SIGC) - Proyecto MDA

Proyecto de desarrollo del **Sistema Integral de Gestión Clínica (SIGC)** bajo la metodología **Model-Driven Architecture (MDA)**, abarcando desde el modelado independiente de computación (CIM) hasta la generación automática de código y despliegue sobre Spring Boot.

## Stack Tecnológico y MDA

- **Metodología:** MDA (`CIM` → `PIM` → `PSM` → `Código`)
- **Modelado y Metamodelado:** Eclipse Modeling Framework (EMF / Ecore)
- **Restricciones Formales:** Object Constraint Language (OCL)
- **Transformación Modelo a Modelo (M2M):** ATLAS Transformation Language (ATL)
- **Transformación Modelo a Texto (M2T):** Eclipse Acceleo (`.mtl`)
- **Backend Objetivo:** Java 17 + Spring Boot 3 (Spring Data JPA, Spring Web, Validation)
- **Base de Datos:** PostgreSQL

## Estructura del Proyecto

- `CIM/`: Modelo Independiente de Computación (contexto de negocio, requisitos, reglas de negocio y diagramas).
- `model/`: Modelo Independiente de Plataforma (`sigc.ecore` y restricciones `sigc.ocl`).
- `transformations/`: Transformaciones M2M en ATL (`pim2psm.atl`).
- `generation/`: Plantillas de generación de código en Acceleo (`.mtl`).
- `generated/`: Salida cruda del código generado por Acceleo (ignorado en control de versiones).
- `app/`: Aplicación final en Spring Boot 3 lista para compilar y ejecutar.
- `docs/`: Documentación de seguimiento y listas de verificación del pipeline MDA.

## Estado de las Fases MDA

| Fase | Nombre | Estado | Fecha |
|---|---|---|---|
| **Fase 0** | Preparación del entorno y estructura | En curso | 2026-11-13 |
| **Fase 1** | CIM (Modelo Independiente de Computación) | Pendiente | - |
| **Fase 2** | PIM / Ecore (Modelo Independiente de Plataforma) | Pendiente | - |
| **Fase 3** | Validación y restricciones OCL | Pendiente | - |
| **Fase 4** | PSM / Transformaciones ATL | Pendiente | - |
| **Fase 5** | Plantillas de generación Acceleo | Pendiente | - |
| **Fase 6** | Generación de código + Integración Spring Boot | Pendiente | - |
| **Fase 7** | Pruebas, validación y documentación final | Pendiente | - |
