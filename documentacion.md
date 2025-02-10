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
| `Timestamp` | Fecha | Fecha en que se registra la Encuesta. |

---

## 3. Variables Para el fácil uso.
A continuación, se parte de las variables originales, y luego colocamos en esta documentación las variables equivalentes, ya que em el procedimiento de la creación del tablero en Looker se cambiaron los nombres de los campos:

| Campo Antiguo | Campo Nuevo | 
|----------|-------------|
| `How old are you?` | Edad | 
| `What industry do you work in?` | Industria |
| `Job title` | Profesión |
| `If your job title needs additional context, please clarify here:` | Contexto Profesión |
| `What is your annual salary? (You'll indicate the currency in a later question. If you are part-time or hourly, please enter an annualized equivalent -- what you would earn if you worked the job 40 hours a week, 52 weeks a year.)` | Salario Anual |
| `How much additional monetary compensation do you get, if any (for example, bonuses or overtime in an average year)? Please only include monetary compensation here, not the value of benefits.` | Otras Compensaciones |
| `Please indicate the currency` | Moneda |
| `If "Other," please indicate the currency here:` | Moneda - Otra |
| `If your income needs additional context, please provide it here:` | Contexto Ingresos |
| `What country do you work in?` | País - incorrecto |
| `If you're in the U.S., what state do you work in?` | Estado |
| `What city do you work in?` | Ciudad Incorrecto |
| `How many years of professional work experience do you have overall?` | Años Experiencia - Total |
| `How many years of professional work experience do you have in your field?` | Años Experiencia - Específicos |
| `What is your highest level of education completed?` | Nivel Educativo |
| `What is your gender?` | Género |
| `What is your race? (Choose all that apply.)` | Raza |
| `Timestamp` | Timestamp |

---

### **Nota**: Es importante tener en cuenta que se realiza un análisis cuidadoso de la información bruta, esto para entender como está el reporte de los datos, lo que se puede definir inicialmente, es que la estructura de la encuesta se va a prestar para demasiados inconvenientes en el futuro, ya que muchas de las preguntas no tienen una estandarización clara que permita, dar valores únicos y al mismo tiempo resultados solidos, es por esto que como primera recomendación, se debería manejar la estandarización de todas las preguntas, y clarificar como deben entrar los valores numéricos, en miles, en unidades y demás.

##  4. Variables Luego del Modelado
Después del procesamiento y limpieza de los datos, se generaron nuevas variables para facilitar el análisis y la visualización en Looker.

| Variable Nueva | Tipo de Dato | Descripción |
|----------------|-------------|-------------|
| `País_Correcto` | Texto | Homologación del campo `country`, estandarizando nombres de países. |
| `Ciudad_Correcta` | Texto | Homologación del campo `city`, corrigiendo errores tipográficos y nombres repetidos. |
| `Salario_Anual_COP` | Número | Conversión del `annual_salary` a Pesos Colombianos (COP) usando la TRM actualizada. |
| `Compensaciones_COP` | Número | Conversión de `additional_compensation` a Pesos Colombianos (COP) con la TRM más reciente. |
| `Salario_Total_COP` | Número | Suma de `Salario_Anual_COP` y `Compensaciones_COP` para un valor total en pesos colombianos. |
| `Industria_act` | Texto | Nos enfocamos en las Industrias señaladas en la encuesta y las demás se agrupan en "Otras". |
| `Anio` | Número | Año del campo Timestamp o año de la toma de la encuesta.  |

---

##  5. Paso a Paso para Actualizar Datos y Aplicar el Modelado
A continuación se evidencia cómo actualizar los datos y aplicar el modelado en Looker para futuras versiones del análisis.

### **Paso 1: Cargar Nueva Base de Datos**
1. Subir la nueva base de datos a Looker manteniendo la misma estructura.
2. Verificar que los nombres de las columnas no hayan cambiado.

### **Paso 2: Homologación de Países, Ciudades**

#### A continuación el código que corresponde a la manualidad realizada para País

1. Crear un **campo calculado** en Looker con un `CASE WHEN` para estandarizar los países:
   ```sql
   CASE 
	WHEN TRIM(País - incorrecto) IN ('For the United States government, but posted overseas','Worldwide (based in US but short term trips aroudn the world)','contracts', 'global',"I work for an US based company but I'm from Argentina.","I am located in Canada but I work for a company in the US", 'currently finance', 'bonus based on meeting yearly goals set w/ my supervisor','international', 'policy', 'n/a (remote from wherever I want)', 'remote', 'remote (philippines)', 'i am located in canada but i work for a company in the us', 'i work for a uae-based organization, though i am personally in the us.',"i work for an us based company but i'm from argentina.", 'i was brought in on this salary to help with the ehr and very quickly was promoted to current position but compensation was not altered.','company in germany. i work from pakistan.', 'from new zealand but on projects across apac', 'from romania, but for an us based company','for the united states government, but posted overseas', 'usat', 'uxz', 'ss', 'dbfemf', 'ibdia', 'loutreland', 'ff') THEN 'Incorrecto'
	WHEN TRIM(País - incorrecto) IN ('united states','U.s.a.','The US','United  States','Unites states','UNited States','The United States','United STates','uS','U.s.a','uSA','UsA','United states of america', 'us','usA', 'usa', 'united states of america', 'america', 'u.s.', 'u.s', 'united state','united stated', 'united statss', 'united statesp', 'untied states', 'united stattes', 'united statea', 'united states of american', 'uniter statez', 'u. s', 'united statues', 'unitef stated', 'united states- puerto rico','us','Unted States','Uniyes States','Uniyed States','Unites States','Uniteed States','UnitedStates','United y','United Statew','United Sttes','United Statws','United Status','United States of Americas','United States is America','United Statees','United Stateds', 'United State of America','United Stares','United Sates of America','United Sates','United States','Uniited States','USaa','USS','USD','US of A','USA','United States','US',	'USA',	'U.S.',	'United States of America',	'U.S>',	'United State',	'U.S.A',	'U.S.A.',	'United State of America',	'United Stated',	'USA-- Virgin Islands',	'United Statws',	'U.S',	'Unites States',	'U. S.',	'United Sates',	'United States of American',	'Uniited States',	'United Sates of America',	'United States (I work from home and my clients are all over the US/Canada/PR',	'Unted States',	'United Statesp',	'United Stattes',	'United Statea',	'United Statees',	'Uniyed states',	'Uniyes States',	'United States of Americas',	'US of A',	'U.SA',	'United Status',	'USS',	'Uniteed States',	'United Stares',	'Unite States',	'UnitedStates',	'United statew',	'United Statues',	'Untied States',	'USA (company is based in a US territory, I work remote)',	'USAB',	'Unitied States',	'United Sttes',	'Uniter Statez',	'U. S',	'USA tomorrow',	'United Stateds',	'US govt employee overseas, country withheld',	'Usat',	'🇺🇸',	'Unitef Stated',	'USaa',	'United States- Puerto Rico',	'United y',	'USD',	"USA, but for foreign gov't",	'United Statss',	'United States is America') THEN 'USA'
	WHEN TRIM(LOWER(País - incorrecto)) IN ('united states','U.s.a.','United  States','UNited States','United STates','uS','U.s.a','uSA','UsA','United states of america', 'us','usA', 'usa', 'united states of america', 'america', 'u.s.', 'u.s', 'united state','united stated', 'united statss', 'united statesp', 'untied states', 'united stattes', 'united statea', 'united states of american', 'uniter statez', 'u. s', 'united statues', 'unitef stated', 'united states- puerto rico','us','Unted States','Uniyes States','Uniyed States','Unites States','Uniteed States','UnitedStates','United y','United Statew','United Sttes','United Statws','United Status','United States of Americas','United States is America','United Statees','United Stateds', 'United State of America','United Stares','United Sates of America','United Sates','United States','Uniited States','USaa','USS','USD','US of A','USA','United States','US',	'USA',	'U.S.',	'United States of America',	'U.S>',	'United State',	'U.S.A',	'U.S.A.',	'United State of America',	'United Stated',	'USA-- Virgin Islands',	'United Statws',	'U.S',	'Unites States',	'U. S.',	'United Sates',	'United States of American',	'Uniited States',	'United Sates of America',	'United States (I work from home and my clients are all over the US/Canada/PR',	'Unted States',	'United Statesp',	'United Stattes',	'United Statea',	'United Statees',	'Uniyed states',	'Uniyes States',	'United States of Americas',	'US of A',	'U.SA',	'United Status',	'USS',	'Uniteed States',	'United Stares',	'Unite States',	'UnitedStates',	'United statew',	'United Statues',	'Untied States',	'USA (company is based in a US territory, I work remote)',	'USAB',	'Unitied States',	'United Sttes',	'Uniter Statez',	'U. S',	'USA tomorrow',	'United Stateds',	'US govt employee overseas, country withheld',	'Usat',	'🇺🇸',	'Unitef Stated',	'USaa',	'United States- Puerto Rico',	'United y',	'USD',	"USA, but for foreign gov't",	'United Statss',	'United States is America') THEN 'USA'
	WHEN TRIM(País - incorrecto) IN ('united kingdom', 'uk', 'great britain', 'england', 'scotland', 'northern ireland', 'wales',  'britain', 'england/uk', 'england, uk.', 'england, gb', 'uk (england)', 'scotland, uk', 'uk, remote', 'northern ireland, united kingdom',  'uk, but for globally fully remote company', 'u.k.', 'u.k',"'United Kingdom",	'UK',	'England',	'England/UK',	'England, UK.',	'United Kingdom (England)',	'United Kingdom.',	'U.K.',	'United Kindom',	'England, UK',	'UK (Northern Ireland)',	'UK for U.S. company',	'United Kingdomk',	'Wales (United Kingdom)',	'England, Gb',	'U.K. (northern England)',	'U.K',	'England, United Kingdom',	'Englang',	'Wales',	'UK (England)',	'UK, remote',	'Wales, UK',	'Wales (UK)',	'UK, but for globally fully remote company') THEN 'United Kingdom'
	WHEN TRIM(País - incorrecto) IN ('canada','CANADA', 'can', 'canda', 'canad', 'canadw', 'csnada', 'canadá', 'canada and usa', 'Canada',	'Canada, Ottawa, ontario',	'Canadw',	'Can',	'Canda',	'Canada and USA',	'Csnada',	'Canad',	'Canadá') THEN 'Canada'
	WHEN TRIM(País - incorrecto) IN ('australia', 'aus', 'australi', 'australian') THEN 'Australia'
	WHEN TRIM(País - incorrecto) IN ('mexico', 'méxico') THEN 'Mexico'
	WHEN TRIM(País - incorrecto) IN ('germany', 'deutschland') THEN 'Germany'
	WHEN TRIM(País - incorrecto) IN ('france', 'fr') THEN 'France'
	WHEN TRIM(País - incorrecto) IN ('spain', 'españa') THEN 'Spain'
	WHEN TRIM(País - incorrecto) IN ('brazil', 'brasil') THEN 'Brazil'
	WHEN TRIM(País - incorrecto) IN ('india') THEN 'India'
	WHEN TRIM(País - incorrecto) IN ('china', 'mainland china') THEN 'China'
	ELSE TRIM(País - incorrecto)
   END

### **Paso 2.1: Homologación de Ciudades**

#### A continuación el código que corresponde a la manualidad realizada para Ciudad:
   ```sql
   CASE 

  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('boston') THEN 'Boston'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('boston, ma') THEN 'Boston'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('nyc', 'new york city', 'new york', 'nyc (remotely)') THEN 'New York'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('washington, dc', 'washington dc', 'dc', 'district of columbia') THEN 'Washington DC'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('st. paul', 'saint paul') THEN 'Saint Paul'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('philadelphia (suburbs)') THEN 'Philadelphia'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('toronto, on', 'toronto') THEN 'Toronto'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('chicago area mostly, but also the us and canada', 'chicago (remote)', 'greater chicago') THEN 'Chicago'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('greater boston area', 'metro boston') THEN 'Boston'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('houston area') THEN 'Houston'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('dfw area', 'dfw') THEN 'Dallas-Fort Worth'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('los angeles', 'la') THEN 'Los Angeles'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('san francisco bay area', 'san francisco') THEN 'San Francisco'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('sacramento, ca') THEN 'Sacramento'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('denver metro') THEN 'Denver'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('charlotte, nc') THEN 'Charlotte'
  	WHEN TRIM(LOWER(Ciudad Incorrecto)) IN ('seattle, wa') THEN 'Seattle'
	ELSE TRIM(LOWER(Ciudad Incorrecto))
   END
---
### Paso 2.2: Homologación de Industria

#### A continuación el código que corresponde a la manualidad realizada para Industria:

