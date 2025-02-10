# 📊 Documentación del Dashboard Salarial

## 📌 1. Introducción
Este proyecto consiste en la limpieza, modelado y análisis de datos salariales a nivel global, visualizados en un **dashboard interactivo en Looker**.

El objetivo principal es transformar los datos originales en una versión estructurada y estandarizada que facilite su análisis, permitiendo explorar tendencias salariales por país, industria, experiencia y género.

---

## 📌 2. Variables en la Base de Datos Original
A continuación, se describen las variables originales del dataset. Los nombres de las variables se mantienen en **inglés**, pero la descripción está en **español**.

| Variable | Tipo de Dato | Descripción |
|----------|-------------|-------------|
| `age` | Texto | Grupo de edad del encuestado. Ejemplo: "25-34". |
| `industry` | Texto | Industria en la que trabaja el encuestado. |
| `job_title` | Texto | Título del puesto del encuestado. |
| `annual_salary` | Número | Salario anual reportado por el encuestado. |
| `additional_compensation` | Número | Compensación monetaria adicional (bonos, overtime, etc.). |
| `currency` | Texto | Moneda en la que se reporta el salario (ej., USD, EUR, COP, etc.). |
| `country` | Texto | País donde trabaja el encuestado. |
| `city` | Texto | Ciudad donde trabaja el encuestado. |
| `years_experience` | Texto | Años de experiencia profesional en total. |
| `years_experience_field` | Texto | Años de experiencia en el campo de trabajo actual. |
| `education` | Texto | Nivel educativo más alto alcanzado. |
| `gender` | Texto | Género del encuestado. |

---

## 📌 3. Variables Luego del Modelado
Después del procesamiento y limpieza de los datos, se generaron nuevas variables para facilitar el análisis y la visualización en Looker.

| Variable Nueva | Tipo de Dato | Descripción |
|----------------|-------------|-------------|
| `País_Correcto` | Texto | Homologación del campo `country`, estandarizando nombres de países. |
| `Ciudad_Correcta` | Texto | Homologación del campo `city`, corrigiendo errores tipográficos y nombres repetidos. |
| `Salario_Anual_COP` | Número | Conversión del `annual_salary` a Pesos Colombianos (COP) usando la TRM actualizada. |
| `Compensaciones_COP` | Número | Conversión de `additional_compensation` a Pesos Colombianos (COP) con la TRM más reciente. |
| `Salario_Total_COP` | Número | Suma de `Salario_Anual_COP` y `Compensaciones_COP` para un valor total en pesos colombianos. |

---

## 📌 4. Paso a Paso para Actualizar Datos y Aplicar el Modelado
Esta sección explica cómo actualizar los datos y aplicar el modelado en Looker para futuras versiones del análisis.

### **🔹 Paso 1: Cargar Nueva Base de Datos**
1. Subir la nueva base de datos a Looker manteniendo la misma estructura.
2. Verificar que los nombres de las columnas no hayan cambiado.

### **🔹 Paso 2: Homologación de Países y Ciudades**
1. Crear un **campo calculado** en Looker con un `CASE WHEN` para estandarizar los países:
   ```sql
   CASE 
     WHEN TRIM(LOWER(`country`)) IN ('us', 'usa', 'united states') THEN 'USA'
     WHEN TRIM(LOWER(`country`)) IN ('uk', 'united kingdom') THEN 'UK'
     ELSE TRIM(`country`)
   END