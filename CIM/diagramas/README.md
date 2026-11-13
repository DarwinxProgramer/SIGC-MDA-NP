# Diagramas PlantUML del CIM - SIGC

Catálogo de diagramas del **Modelo Independiente de Computación (CIM)** para el Sistema Integral de Gestión Clínica (SIGC), compatibles con PlantUML 1.2024+.

## Lista de Diagramas

1. `01_casos_uso.puml`: Diagrama general de casos de uso por módulos para Administrador, Recepcionista y Médico, incluyendo autenticación (`<<include>>`), auditoría (`<<extend>>`) y restricción de acceso clínico del Administrador.
2. `02_actividad_P01_registrar_paciente.puml`: Flujo de actividad (Recepcionista | Sistema) para búsqueda previa, validación de duplicados, registro de paciente, creación automática 1:1 del historial clínico y auditoría.
3. `03_actividad_P02_agendar_cita.puml`: Flujo de actividad (Recepcionista | Sistema) para agendamiento de citas en estado `PENDIENTE`, validando especialidad del médico y no solapamiento con margen de 30 minutos.
4. `04_actividad_P03_consulta_medica.puml`: Flujo de actividad (Médico | Sistema) para atención médica, transición `PENDIENTE` → `EN_PROCESO` → `FINALIZADA`, registro de consulta, diagnóstico, tratamiento, prescripción y auditoría clínica.
5. `05_actividad_P04_inventario.puml`: Flujo de actividad (Administrador | Sistema) para registro de entradas y salidas de inventario con validación de existencias disponibles (`RN-10`) y alertas de stock bajo.
6. `06_actividad_P05_autenticacion.puml`: Flujo de actividad de inicio de sesión, validación de usuario activo, carga de múltiples roles (`Usuario * — * Rol`), restricción clínica para rol `ADMIN` y auditoría de acceso.
7. `07_estado_cita.puml`: Máquina de estados del ciclo de vida de la entidad `Cita` (`PENDIENTE`, `EN_PROCESO`, `FINALIZADA`, `CANCELADA`) y documentación de transiciones prohibidas (`RN-12`).
8. `08_clases_conceptual.puml`: Modelo conceptual de dominio con las entidades principales, enumeraciones (`<<enumeration>>`), multiplicidades y decisiones clave (`Paciente 1—1 HistorialClinico`, `Usuario *—* Rol`, `ProductoInventario` unificado).

## Cómo renderizar todos los diagramas

Desde esta carpeta (`SIGC-MDA/CIM/diagramas/`), ejecuta:

```bash
plantuml *.puml
```

Para generarlos en formato SVG de alta resolución:

```bash
plantuml -tsvg *.puml
```
