# Checklist de Fases del Pipeline MDA - SIGC

Seguimiento de entregables por cada fase del desarrollo dirigido por modelos (MDA) del Sistema Integral de Gestión Clínica (SIGC).

## Fase 0: Preparación (Completada)
- [x] JDK 17 / JDK 21 configurado en entorno Eclipse
- [x] Eclipse Modeling Tools (EMF / Ecore / GenModel)
- [x] ATL (ATLAS Transformation Language)
- [x] Acceleo (M2T Generator)
- [x] Maven
- [ ] PostgreSQL / Docker (pendiente de levantar para Fase 6/7)
- [x] Git inicializado
- [x] Estructura de carpetas (`SIGC-MDA/`)

## Fase 1: CIM (Computation Independent Model) (Completada)
- [x] `CIM/CIM_SIGC.md` (especificación formal del CIM y matriz de trazabilidad)
- [x] Diagramas PlantUML en `CIM/diagramas/`:
  - [x] `01_casos_uso.puml`
  - [x] `02_actividad_P01_registrar_paciente.puml`
  - [x] `03_actividad_P02_agendar_cita.puml`
  - [x] `04_actividad_P03_consulta_medica.puml`
  - [x] `05_actividad_P04_inventario.puml`
  - [x] `06_actividad_P05_autenticacion.puml`
  - [x] `07_estado_cita.puml`
  - [x] `08_clases_conceptual.puml`

## Fase 2: PIM / Ecore (Platform Independent Model) (Completada)
- [x] `model/sigc.ecore` validado en Eclipse (7 enumeraciones y 17 clases de dominio)
- [x] `model/sigc.genmodel` generado y validado sin errores

## Fase 3: OCL (Object Constraint Language) (Completada)
- [x] `model/sigc.ocl` con las reglas de negocio formales (`RN-01` a `RN-12`) en sintaxis OCL 2.4

## Fase 4: PSM / ATL (Platform Specific Model) (Completada)
- [x] `model/psm.ecore` (metamodelo relacional destino: `Database`, `Table`, `Column`, `ForeignKey`)
- [x] `transformations/pim2psm.atl` (reglas de transformación PIM → PSM)

## Fase 5: Acceleo (Generación M2T) (En curso)
- [x] Proyecto Eclipse `SIGC-Acceleo` configurado (`MANIFEST.MF`)
- [x] Plantilla base `generation/generate.mtl` (generación de clases Java desde `sigc.ecore`)
- [ ] Ejecución de `generate.mtl` hacia `generated/`
- [ ] Plantillas adicionales / capas (`entity`, `repository`, `service`, `controller`, `dto`, `mapper`, `main`)

## Fase 6: Generación + Spring Boot (Pendiente)
- [ ] Proyecto Spring Boot 3 compilando en `app/`
- [ ] Entidades JPA anotadas e integradas
- [ ] Base de datos PostgreSQL migrada

## Fase 7: Pruebas y Cierre (Pendiente)
- [ ] Tests JUnit
- [ ] Colección Postman / Swagger UI
- [ ] `README` final de despliegue
