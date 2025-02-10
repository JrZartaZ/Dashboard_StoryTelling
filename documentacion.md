# Documentación del Dashboard Salarial

##  1. Introducción
Este proyecto consiste en la limpieza, modelado y análisis de datos salariales a nivel global, visualizados en un **dashboard interactivo en Looker**.

El objetivo principal es transformar los datos originales en una versión estructurada y estandarizada que facilite su análisis, permitiendo explorar tendencias salariales por país, industria, experiencia y género.

---

## 2. Variables en la Base de Datos Original
A continuación, se describen las variables originales del dataset. A Pesar que se le hace un tratamiento interno para la facilidad de manejar los campos, a continuación se agrega el nombre original de cada Campo.

| Variable | Tipo de Dato | Descripción |
|----------|-------------|-------------|
| `How old are you?` | Texto | Grupo de edad del encuestado. Ejemplo: "25-34". |
| `What industry do you work in?` | Texto | Industria en la que trabaja el encuestado. |
| `Job title` | Texto | Título del puesto del encuestado. |
| `If your job title needs additional context, please clarify here:` | Texto | Contexto del puesto del encuestado. |
| `What is your annual salary? (You'll indicate the currency in a later question. If you are part-time or hourly, please enter an annualized equivalent -- what you would earn if you worked the job 40 hours a week, 52 weeks a year.)` | Número | Salario anual reportado por el encuestado. |
| `How much additional monetary compensation do you get, if any (for example, bonuses or overtime in an average year)? Please only include monetary compensation here, not the value of benefits.` | Número | Compensación monetaria adicional (bonos, overtime, etc.). |
| `Please indicate the currency` | Texto | Moneda en la que se reporta el salario (ej., USD, EUR, COP, etc.). |
| `If "Other," please indicate the currency here:` | Texto | Condicionante si la moneda es otra a las de la lista. |
| `If your income needs additional context, please provide it here:` | Texto | Contexto de tenencia de Ingresos. |
| `What country do you work in?` | Texto | País donde trabaja el encuestado. |
| `If you're in the U.S., what state do you work in?` | Texto | Estado si el encuestado está en Estados Unidos. |
| `What city do you work in?` | Texto | Ciudad donde trabaja el encuestado. |
| `How many years of professional work experience do you have overall?` | Texto | Años de experiencia profesional en total. |
| `How many years of professional work experience do you have in your field?` | Texto | Años de experiencia en el campo de trabajo actual. |
| `What is your highest level of education completed?` | Texto | Nivel educativo más alto alcanzado. |
| `What is your gender?` | Texto | Género del encuestado. |
| `What is your race? (Choose all that apply.)` | Texto | Raza o Etnia del encuestado. |

---

##  3. Variables Luego del Modelado
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