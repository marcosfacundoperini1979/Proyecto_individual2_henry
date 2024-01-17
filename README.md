Proyecto Individual Numero 2 --- Siniestros Viales

***


Los siniestros viales, también conocidos como accidentes de tráfico o accidentes de tránsito, son eventos que involucran vehículos en las vías públicas y que pueden

tener diversas causas, como colisiones entre automóviles, motocicletas, bicicletas o peatones, atropellos, choques con objetos fijos o caídas de vehículos. 

Estos incidentes pueden tener consecuencias que van desde daños materiales hasta lesiones graves o fatales para los involucrados.

En el contexto de una ciudad como Buenos Aires, los siniestros viales pueden ser una preocupación importante debido al alto volumen de tráfico y la densidad poblacional.

Estos incidentes pueden tener un impacto significativo en la seguridad de los residentes y visitantes de la ciudad, así como en la infraestructura vial y

los servicios de emergencia.

Las tasas de mortalidad relacionadas con siniestros viales suelen ser un indicador crítico de la seguridad vial en una región.

Estas tasas se calculan, generalmente, como el número de muertes por cada cierto número de habitantes o por cada cierta cantidad de vehículos registrados.

Reducir estas tasas es un objetivo clave para mejorar la seguridad vial y proteger la vida de las personas en la ciudad.

Es importante destacar que la prevención de siniestros viales involucra medidas como la educación vial, el cumplimiento de las normas de tráfico,

la infraestructura segura de carreteras y calles, así como la promoción de vehículos más seguros. El seguimiento de las estadísticas y

la implementación de políticas efectivas son esenciales para abordar este problema de manera adecuada.

***
Datos Utilizados 🚀

Los datos utilizados para este análisis fueron extraídos de la siguiente fuente:
https://data.buenosaires.gob.ar/dataset/victimas-siniestros-viales.

Comunas:
https://www.argentina.gob.ar/caba/comunas

Poblacion de la Ciudad de Buenos Aires Datos Obtenidos de :
https://es.wikipedia.org/wiki/Anexo:Comunas_de_la_ciudad_de_Buenos_Aires_por_poblaci%C3%B3n

***

Comenzando 🚀

Realizamos un primer acercamiento a los datos haciendo un ETL Correspondiente no tenemos demasiado que trabajar pero asi igual corroboramos.
Para Datos Especificos del ETL adjunto link del Notebook [ETL](https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/blob/main/etl.ipynb)
![image](https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/assets/125610561/e68c7cdf-3a01-420d-9ee6-069fff5efa93)

Como Pocedimiento mas importante en los datos hemos eliminado la Columna Altura Ya que la Misma Tenia Mas de un 80% de datos faltantes y 
no resulto al cabo de todo el proyecto reelevante la misma.

Fui Adpatando eldataset a un formato  que fuese consistente con nuestros objetivos, modificando tipo de datos por ejemplo.
Trabajamos sobre la columna de Latitud y Longitud para dejarlos acorde.

Aplicamos la Funcion Map para incorporar la columna barrios correspondientes a cada comuna, se agrego columna Dias para poder tener mas herramientas de filtrado.

guardamos el archivo como siniestros.to_csv("siniestros_limpio.csv", index=False), el cual es adjuntado en la carpeta DATA:
https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/blob/main/data/siniestros_limpio.csv

***
Pasamos al EDA

Adjunto link del Notebook para mas detalles [EDA](https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/blob/main/eda.ipynb)


se Buscaron los primeros Outliers para evitar Datos erroneos
![image](https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/assets/125610561/8be0b34b-dddf-43bf-aaac-f424b428e069)

Se  vieron caracteristicas particulares Como por ejemplo una cierta estabilidad pre pandemia en cuanto a las victimas, fatales.... y post pandemia un incremento 
Brusco de las misma 

![image](https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/assets/125610561/14a18b12-767d-47eb-a17c-de7febb8131f)

podemos visualizar el incrmento de Fatalidades  cercano a las Fiestas de Finde Años se Incrementa notablemente 
![image](https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/assets/125610561/e5541b35-bc23-43eb-a6cb-a811a617b966)



en los histogramas de victimas podemos individualizar  el mayor indice de decesos   en el Point Of Control de la campana Gaussiana representada por el grafico
![image](https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/assets/125610561/7704ec23-9cc7-4922-866f-cf683a8e309c)

visualizamos  Datos  usando Geopandas  de victimas 

![image](https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/assets/125610561/bd425fc6-99b6-4c4c-b562-fd68b3ce41d4)



podemos ver los barrios con mayor indice de accidentes.....  

![image](https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/assets/125610561/51a736f9-dcb8-4ad7-893f-506eae41acda)


sacando Como conclusiones y determinando graficamente  Puntos muy interesantes a tener en cuenta a la hora de interpretar  los KPI





***





Procedemos a usar Mysql  📋

procedemos a abrir el archivo CSV , primero  creando las tablas pertinentes 
Facu Elmes Mmm, [17/1/2024 03:28]
-- Selecciona la base de datos
USE homicidios_sql;

-- Crea la tabla
CREATE TABLE Accidentes (
    ID VARCHAR(255),
    N_VICTIMAS INT,
    FECHA DATE,
    AÑO INT,
    MES INT,
    DIA INT,
    MM INT,
    HORA TIME,
    LUGAR_DEL_HECHO VARCHAR(255),
    TIPO_DE_CALLE VARCHAR(255),
    COMUNA INT,
    LONGITUD DECIMAL(10,8),
    LATITUD DECIMAL(10,8),
    VICTIMA VARCHAR(255),
    ACUSADO VARCHAR(255),
    ROL_VICTIMA VARCHAR(255),
    SEXO VARCHAR(255),
    EDAD INT,
    FECHA_FALLECIMIENTO VARCHAR(255)
);


-- Importa los datos desde el archivo CSV
LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/siniestros_limpios.csv'
INTO TABLE Accidentes
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

![image](https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/assets/125610561/f6547e63-9017-417d-b418-4d70f730c2f3)



una vez teninedo acceso a nuestra Base de datos Procedi a conectar via Power Bi- Lo cual fue todo una aventura, ya que necesite instalar los conectores.net y
como ejecute varias veces tuve que limpiar el registro  luego de arduas horas de trabajo  ya que no podia conectar.
para ser mas especifico  hay que limpiar las entradas duplicadas en C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config

Luego fue simplemente cargar datos  desde base de datos mysql   localhost:3306  agregar las credenciales root y contraseña y listo

Una vez Obtenidos los Datos extraje de las paginas publicadas anteriormente tablas para la confeccion de los KPI's

KPI 1
 *Reducir en un 10% la tasa de homicidios en siniestros viales de los últimos seis meses, en CABA, en comparación con la tasa de homicidios en 
 siniestros viales del semestre anterior*
 
> Definimos a la **tasa de homicidios en siniestros viales** como el número de víctimas fatales en accidentes de tránsito por cada 100,000 habitantes en un área geográfica durante un período de tiempo específico.
  Su fórmula es: (Número de homicidios en siniestros viales / Población total) * 100,000
>
> ![image](https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/assets/125610561/583b6ba0-c145-4738-8758-b4ec618c9aca)




KPI 2



> - *Reducir en un 7% la cantidad de accidentes mortales de motociclistas en el último año, en CABA, respecto al año anterior*
>
> Definimos a la **cantidad de accidentes mortales de motociclistas en siniestros viales** como el número absoluto de accidentes fatales en los que estuvieron involucradas víctimas que viajaban en moto en un determinado periodo temporal.
  Su fórmula para medir la evolución de los accidentes mortales con víctimas en moto es:
>  (Número de accidentes mortales con víctimas en moto en el año anterior - Número de accidentes mortales con víctimas en moto en el año actual) /
> (Número de accidentes mortales con víctimas en moto en el año anterior) * 100
> 
![image](https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/assets/125610561/645e4eac-866d-4929-b2ba-a13c21d14d30)

Lo cual Podemos observar un incremento, pero tambien podemos vincular que  ese semestre estamos ante la salida post-pandemia la cual no arroja con exacitud una fuente
fiable de  datos..


KPI 3

Accidentes Fatales Múltiples en Fines de Semana



Este KPI mide el número de accidentes que resultan en más de una muerte durante los fines de semana.
Es útil para entender la gravedad de los accidentes que ocurren durante estos días y puede ayudar a identificar situaciones de alto riesgo.

![image](https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/assets/125610561/6ce79758-eabc-48b5-99a2-4241a2953a97)

en el cual podemos identificar que los accidentes mas resonantes ocurren los finde semanas




Dashboard principal


![image](https://github.com/marcosfacundoperini1979/Proyecto_individual2_henry/assets/125610561/186d2a17-e56d-4c84-bf7b-09c61629d52a)

***

Formato del Repositoorio

📂 Data: Aca se encuentran los conjuntos de datos que fueron empleados en el análisis.

CSV: Incluye los archivos tras la ejecución del proceso ETL.
Originales: Almacena los archivos que fueron descargados inicialmente.

📂 DataSQL: Contiene codigo dumpeado de la base de datos de MYsql

EDA.ipynb  codigo fuente donde se realiza EDA en profundidad y explicado paso a paso

ETL.ipynb codigo fuente donde se realiza el ETL 


***
Autor ✒️
Marcos Facundo Perini


