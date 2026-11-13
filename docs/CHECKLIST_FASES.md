# Checklist de Fases del Pipeline MDA - SIGC

Seguimiento de entregables por cada fase del desarrollo dirigido por modelos (MDA) del Sistema Integral de Gestión Clínica (SIGC).

## Fase 0: Preparación
- [ ] JDK 17
- [ ] Eclipse Modeling Tools
- [ ] ATL (ATLAS Transformation Language)
- [ ] Acceleo
- [ ] Maven
- [ ] PostgreSQL / Docker
- [ ] Git
- [x] Estructura de carpetas

## Fase 1: CIM (Computation Independent Model)
- [ ] `CIM_SIGC.md`
- [ ] Diagramas PlantUML de `P-01` a `P-05`

## Fase 2: PIM / Ecore (Platform Independent Model)
- [ ] `sigc.ecore` validado en Eclipse

## Fase 3: OCL (Object Constraint Language)
- [ ] `sigc.ocl` con `RN-01` a `RN-12` + validación en Eclipse

## Fase 4: PSM / ATL (Platform Specific Model)
- [ ] `pim2psm.atl` o `psm.ecore` generado

## Fase 5: Acceleo (Generación M2T)
- [ ] Plantilla `entity.mtl`
- [ ] Plantilla `repository.mtl`
- [ ] Plantilla `service.mtl`
- [ ] Plantilla `controller.mtl`
- [ ] Plantilla `dto.mtl`
- [ ] Plantilla `mapper.mtl`
- [ ] Plantilla `main.mtl`

## Fase 6: Generación + Spring Boot
- [ ] Proyecto Spring Boot compilando
- [ ] Base de datos PostgreSQL migrada

## Fase 7: Pruebas y Cierre
- [ ] Tests JUnit
- [ ] Colección Postman
- [ ] `README` final
