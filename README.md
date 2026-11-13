# Sistema Integral de Gestión Clínica (SIGC) - Proyecto MDA

Proyecto de desarrollo del **Sistema Integral de Gestión Clínica (SIGC)** bajo la metodología **Model-Driven Architecture (MDA)**, abarcando desde el modelado independiente de computación (CIM) hasta la transformación de modelos y generación automática de código hacia Java / Spring Boot.

## Stack Tecnológico y MDA

- **Metodología:** MDA (`CIM` → `PIM` → `PSM` → `Código`)
- **Modelado y Metamodelado:** Eclipse Modeling Framework (EMF / Ecore / GenModel)
- **Restricciones Formales:** Object Constraint Language (OCL 2.4)
- **Transformación Modelo a Modelo (M2M):** ATLAS Transformation Language (ATL)
- **Transformación Modelo a Texto (M2T):** Eclipse Acceleo (`.mtl`)
- **Backend Objetivo:** Java 17 + Spring Boot 3 (Spring Data JPA, Spring Web, Validation)
- **Base de Datos:** PostgreSQL

## Estructura del Repositorio

```text
SIGC-MDA/
├── CIM/
│   ├── CIM_SIGC.md                  # Especificación del CIM y matriz de trazabilidad
│   └── diagramas/                   # 8 diagramas PlantUML (Casos de uso, P-01..P-05, Estados, Clases)
├── model/
│   ├── sigc.ecore                   # Metamodelo PIM (17 clases y 7 enumeraciones)
│   ├── sigc.genmodel                # Modelo generador EMF validado
│   ├── sigc.ocl                     # Restricciones formales de negocio (RN-01 a RN-12)
│   └── psm.ecore                    # Metamodelo PSM relacional (Database, Table, Column, ForeignKey)
├── transformations/
│   └── pim2psm.atl                  # Reglas ATL de transformación PIM -> PSM
├── generation/
│   └── generate.mtl                 # Plantilla Acceleo (M2T) para generación de clases Java
├── generated/                       # Salida de código generado por Acceleo
├── app/                             # Aplicación objetivo en Spring Boot 3
└── docs/
    ├── CHECKLIST_FASES.md           # Checklist detallado de entregables por fase
    ├── Estado-SIGC_MDA.md           # Documento maestro de avance y handoff para el equipo
    └── SIGC_contexto_MDA.md         # Documento consolidado de contexto, requisitos y decisiones
```

## Estado de las Fases MDA

| Fase | Nombre | Estado | Fecha |
|---|---|---|---|
| **Fase 0** | Preparación del entorno y estructura | Completada | 2026-11-13 |
| **Fase 1** | CIM (Modelo Independiente de Computación) | Completada | 2026-11-13 |
| **Fase 2** | PIM / Ecore (Modelo Independiente de Plataforma) | Completada | 2026-11-13 |
| **Fase 3** | Validación y restricciones OCL | Completada | 2026-11-13 |
| **Fase 4** | PSM / Transformaciones ATL | Completada | 2026-11-13 |
| **Fase 5** | Plantillas de generación Acceleo | En curso | 2026-11-13 |
| **Fase 6** | Generación de código + Integración Spring Boot | Pendiente | - |
| **Fase 7** | Pruebas, validación y documentación final | Pendiente | - |
