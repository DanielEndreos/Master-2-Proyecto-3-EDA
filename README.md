# Master 2º: Proyecto EDA con Python

## Estructura NoteBook
1. **Dependencias y Configuración Entorno**  
    1.1. Librerías Principales  
    1.2. Librerías Gráficos  
    1.3. Otras Librerías  
    1.4. Customización del NoteBook  
    1.5. Direccionamiento Paths  

2. **Definición de Funciones**  
    2.1. Función `extraerElementoFecha(fecha, elemento)`  

* 3. **Procesamiento de Archivos**  
    * 3.1. Preparación Inicial `.XLSX`  
        * 3.1.1. Identificación Hojas  
        * 3.1.2. Comprobación Columnas entre Hojas  
        * 3.1.3. Concatenación Hojas en DataFrame `df1`  

    * 3.2. Preparación Inicial `.CSV`  
        * 3.2.1. Transformación en DataFrame `df2`  

    * 3.3. Preparación para Unificar DataFrames con `.Merge()`  
        * 3.3.1. Identificación Columnas en DataFrames  
        * 3.3.2. Comprobación Duplicados en DataFrames  
        * 3.3.3. Corrección Columnas en DataFrames  
        * 3.3.4. Creación Columnas `contact_month` y `contact_year`  
        * 3.3.5. Comprobación Dimensiones Tablas  
        * 3.3.6. Unión de DataFrames  

    * 3.4. Limpieza de Datos  
        * 3.4.1. Comprobación Cantidad NaN por Columna
        * 3.4.2. 
        * 3.4.3. Limpieza Job

* 4. **Analisis de Datos**
    * 4.1. Clientes Suscritos  
        * 4.1.1. Clasificación por Edad  
        * 4.1.2. Clientes Contactados Vs Suscritos   
            * 4.1.2.1. Gráfica Clientes Contactados   
            * 4.1.2.2. Gráfica Clientes Suscritos   
            * 4.1.2.3. Cálculo % de Contratación por Edad   
            * 4.1.2.4. Cálculo % de Contratación por Fecha   

    5.1. 

        4.1.2.  


## Analisis Visual sobre el enunciado del ejercicio
1. Puntos a Sanear

    1. Discrepancia entre las columnas reales / enunciado:
        
        - *Ref.:* [bank-additional.csv](https://github.com/DanielEndreos/Master-2-Proyecto-3-EDA/blob/main/DatosProyecto/bank-additional.csv)

        En el enunciado del proyecto, menciona las columnas "contact_month" y "contact_year"
        En el documento **.csv** facilitado, no existen esas columnas y en su lugar están "latitude" y "longitude".

        Para el analisis de los datos, vamos a obviar que hay un error en la documentación y utilizaremos las coordenadas en nuestro beneficio.

        Para los datos "contact_month" y "contact_year", aprovecharé "date" que es la unión de ambos, procederé a crear dos columnas nuevas, con los nombres propuestos y en cada una colocaremos el dato correspondiente.

## Limpieza y Preparación

1. Puntos a Sanear

    1. Headers **.CSV** y **.XLSX**
        
        - *Ref.:* [bank-additional.csv](https://github.com/DanielEndreos/Master-2-Proyecto-3-EDA/blob/main/DatosProyecto/bank-additional.csv)
        - *Ref.:* [customer-details.xlsx](https://github.com/DanielEndreos/Master-2-Proyecto-3-EDA/blob/main/DatosProyecto/customer-details.xlsx)

        1. Columna "id"
            
            En ambos documentos se nombra de manera distinta, eso es un problema a la hora de realizar un `pd.merge()` entre ambos archivos. Ya que necesitamos un punto de unión entre archivos y el mejor punto de unión es la "id" ya que es la relación entre ambos documentos, no podemos utilizar la supuesta "PK" ya que podrían estar introducidos en otro orden.

            Es por ello, que modificaré el nombre de esta columna a "id" para mantener la relación de textos con las demás cabeceras.

        2. Columnas
            
            1. Lower Case:
                Para evitar problemas con el case sensitive, he decidido llevar todos los textos de los títulos a minusculas mediante `.str.lower()`.

            2. Strip:
                Para evitar problemas en el que si hay alguna columna con espacios antes/después del texto, y con ello, me genere problemas en las consultas, antes de unir con `pd.merge()` he realizado un `.str.strip()` de las columnas.


## Conclusiones



