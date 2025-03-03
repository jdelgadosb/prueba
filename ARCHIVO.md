![image](https://github.com/user-attachments/assets/d68bd6ec-4ca4-402a-9fde-ab29c8bfee8b)

# Regla de Calidad de Datos: <span style="color: #35CACB;">IDMC-00004</span>

<div align="center">
  <img src="https://img.shields.io/badge/Calidad_De_Datos-4CAF50?style=for-the-badge&logo=checkmarx&logoColor=white" alt="Data Quality" />
  <img src="https://img.shields.io/badge/Consistencia-4CAF50?style=for-the-badge&logo=sync&logoColor=white" alt="Consistency" />
</div>


# Índice

- [Regla de Calidad de Datos: IDMC-00004](#regla-de-calidad-de-datos-idmc-00001)
- [Índice](#índice)
  - [1. Objetivo de la Regla](#1-objetivo-de-la-regla)
  - [2. Descripción de la Regla](#2-descripción-de-la-regla)
    - [Tabla de Equivalencia con el MRI](#tabla-de-equivalencia-con-el-mri)
  - [3. Lógica de Validación](#3-lógica-de-validación)
    - [Procedimiento de Validación:](#procedimiento-de-validación)
  - [4. Glosario - Resultados de Discrepancias](#4-glosario---resultados-de-discrepancias)
  - [5. Ejemplo de Discrepancias](#5-ejemplo-de-discrepancias)
  - [6. Confirmación de Discrepancias](#6-confirmación-de-discrepancias)
    - [Procedimiento de Validación Interna:](#procedimiento-de-validación-interna)
    - [Ejemplo de Validación Interna:](#ejemplo-de-validación-interna)

---

## 1. Objetivo de la Regla

El objetivo de esta regla es garantizar la **consistencia** del campo `MONTODESEMBOLSADO` en el reporte `OPERACIONES CREDITICIAS DE DEUDORES`. Se considera inconsistencia cuando este campo es `0`, `nulo`, contiene espacios en blanco o está vacío, y existe un registro cruzado en el reporte `DETALLE DE OPERACIONES CREDITICIAS DE DEUDORES – CUENTAS CONTABLES` con una cuenta contable que no inicia con `6` o `252`.

---

## 2. Descripción de la Regla

| Campo | Contenido |
|--------------------------------|-----------|
| **Regla_ID** | IDMC-00004 |
| **Plataforma** | IDMC |
| **Fecha_Creacion** | 2025-03-03 |
| **Regla_Familia** | 1 |
| **Regla_Tipo** | InterReporte |
| **Dimensión de Calidad** | Consistencia |
| **Descripción de la Discrepancia** | Se detecta inconsistencia cuando el campo `MONTODESEMBOLSADO` en el reporte `OPERACIONES CREDITICIAS DE DEUDORES` es `0`, nulo, contiene espacios en blanco o está vacío, y existe un registro cruzado en el reporte `DETALLE DE OPERACIONES CREDITICIAS DE DEUDORES – CUENTAS CONTABLES` cuya cuenta contable no inicia con `6` o `252`. |
| **Mensaje de Discrepancia** | Monto desembolsado inválido en relación con la cuenta contable del reporte cruzado. |
| **Reportes Afectados** | OPERACIONES CREDITICIAS DE DEUDORES, DETALLE DE OPERACIONES CREDITICIAS DE DEUDORES – CUENTAS CONTABLES |
| **Campos Afectados** | `RC01_MONTODESEMBOLSADO`, `RC02_CUENTACONTABLE` |

### Tabla de Equivalencia con el MRI
> Los nombres de los campos mencionados corresponden a una propuesta recomendada para fines de validación y posible incorporación en las bases de datos.
> Se entiende que estos nombres pueden variar según la entidad, por lo que se proporciona la siguiente tabla de equivalencias con los nombres utilizados en el documento oficial MRI.  


| **Reporte** | **Nombre_Campo_MRI** | **Nombre_Campo_BD** |
|------------|-----------------------|----------------------|
| RC01 - OPERACIONES CREDITICIAS DE DEUDORES | MONTO DESEMBOLSADO | RC01_MONTODESEMBOLSADO |
| RC02 - DETALLE DE OPERACIONES CREDITICIAS DE DEUDORES – CUENTAS CONTABLES | CUENTA CONTABLE | RC02_CUENTACONTABLE |

---

## 3. Lógica de Validación

### Procedimiento de Validación:
1. Identificar los registros en `RC01` donde `RC01_MONTODESEMBOLSADO` sea `0`, nulo, vacío o tenga espacios en blanco.
2. Cruzar con `RC02` usando la clave común entre ambos reportes.
3. Verificar si el valor de `RC02_CUENTACONTABLE` no inicia con `6` o `252`.
4. Si se cumple la condición anterior, marcar el registro como inconsistente.

**Excepciones:** No aplica cuando `RC02_CUENTACONTABLE` inicia con `6` o `252`.

---

## 4. Glosario - Resultados de Discrepancias

| Campo | Descripción |
|------------------------------|-------------|
| **DISCREPANCIA_ID** | Identificador único de la discrepancia detectada. |
| **DISCREPANCIA_ELEMENTO** | Identificador del elemento afectado. |
| **REGLA_ID** | Identificador de la regla asociada a la discrepancia. |
| **PERIODO** | Periodo asociado a la discrepancia (formato: AAAAMMDD). |
| **REPORTES_AFECTADOS** | Reportes asociados a la discrepancia. |
| **CAMPOS_AFECTADOS** | Lista de los campos afectados dentro de los reportes. |
| **CUENTAS_AFECTADAS** | Lista de cuentas impactadas por la discrepancia. |
| **DISCREPANCIA_MENSAJE** | Mensaje de la discrepancia. |
| **DISCREPANCIA_ESTATUS** | Estado actual de la discrepancia (por ejemplo, ACTIVA, CORREGIDA). |
| **ACEPTACION** | Indica si la entidad está de acuerdo o no con la discrepancia. |

---

## 5. Ejemplo de Discrepancias

| DISCREPANCIA_ID | DISCREPANCIA_ELEMENTO | PERIODO | REPORTES_AFECTADOS | CAMPOS_AFECTADOS | CUENTAS_AFECTADAS | DISCREPANCIA_MENSAJE | DISCREPANCIA_ESTATUS | ACEPTACION |
|-----------------|-----------------|---------|-----------------|-----------------|-----------------|-----------------|-----------------|------------|
| 2D51A87B55249C18E0630D0310AC99A2 | 020-0016696-3 | 20241201 | RC01, RC02 | RC01_MONTODESEMBOLSADO, RC02_CUENTACONTABLE | | Monto desembolsado inválido en relación con la cuenta contable del reporte cruzado. | ACTIVA | 1 |

---

## 6. Confirmación de Discrepancias

Si la entidad desea confirmar que la discrepancia detectada por la regla `IDMC-00004` es válida en su base de datos interna, se debe analizar la agrupación de los registros y las cuentas asociadas.

### Procedimiento de Validación Interna:
1. **Filtrar registros**: Identificar todos los registros de `RC01_MONTODESEMBOLSADO` nulos, vacíos o con espacios en blanco.
2. **Cruzar con RC02**: Vincular con `RC02_CUENTACONTABLE`.
3. **Verificar Cuentas Contables**: Confirmar si el prefijo es distinto de `6` o `252`.
4. **Registrar discrepancias**: Si la condición se cumple, registrar la discrepancia.

---

