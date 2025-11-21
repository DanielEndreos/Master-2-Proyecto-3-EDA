# Master 2º: Proyecto EDA con Python

## Estructura NoteBook
* 1 - **Dependencias y Configuración Entorno**  
    * 1.1  Librerías Principales  
    * 1.2  Librerías Gráficos  
    * 1.3  Otras Librerías  
    * 1.4  Customización del NoteBook  
    * 1.5  Direccionamiento Paths  

* 2 - **Definición de Funciones**  
    * 2.1  Función `extraerElementoFecha(fecha, elemento)`  

* 3 - **Procesamiento de Archivos**  
    * 3.1 - Preparación Inicial `.XLSX`  
        * 3.1.1  Identificación Hojas  
        * 3.1.2  Comprobación Columnas entre Hojas  
        * 3.1.3  Concatenación Hojas en DataFrame `df1`  

    * 3.2 - Preparación Inicial `.CSV`  
        * 3.2.1  Transformación en DataFrame `df2`  

    * 3.3 Preparación para Unificar DataFrames con `.Merge()`  
        * 3.3.1  Identificación Columnas en DataFrames  
        * 3.3.2  Comprobación Duplicados en DataFrames  
        * 3.3.3  Corrección       Columnas en DataFrames  
        * 3.3.4  Creación Columnas `contact_month` y `contact_year`  
        * 3.3.5  Comprobación Dimensiones Tablas  
        * 3.3.6  Unión de DataFrames  

    * 3.4 Limpieza de Datos  
        * 3.4.1  Comprobación Columnas (NaN, Tipo Dato)  
        * 3.4.2  Creación Copia antes de Limpieza Datos  
        * 3.4.3  Corrección Tipos de Dato  
        * 3.4.4  Busqueda correlación entre `euribor3m` y `emp.var.rate`  

    * 3.5 Exportación Datos a Excel

* 4 - **Analisis de Datos**
    * 4.1 Descripción Datos

    * 4.2 Éxito Por Campaña
        * 4.2.1  Tabla Datos
        * 4.2.2  Gráfica
          
    * 4.3 Clientes Suscritos  
        * 4.3.1  Clientes Contactados Vs Suscritos   
            * 4.3.1.1  Gráfica Clientes Contactados   
            * 4.3.1.2  Gráfica Clientes Suscritos   
            * 4.3.1.3  Cálculo % de Contratación por Edad   

        * 4.3.2  Análisis Clientes Sucritos   
            * 4.3.2.1  Criterio Por Trabajo   
            * 4.3.2.2  Criterio Por Estado Civil   
            * 4.3.2.3  Criterio Por Estudios  
            * 4.3.2.4  Criterio Por Estar Hipotecado   
            * 4.3.2.5  Criterio Por Estar Endeudado   
            * 4.3.2.6  Criterio Por Número Visitas Web

## Analisis Visual sobre el enunciado del ejercicio
1. Puntos a Sanear

    1. Discrepancia entre las columnas reales / enunciado:
        
        - *Ref.:* [bank-additional.csv](https://github.com/DanielEndreos/Master-2-Proyecto-3-EDA/blob/main/DatosProyecto/bank-additional.csv)

        En el enunciado del proyecto, menciona las columnas "contact_month" y "contact_year"
        En el documento **.csv** facilitado, no existen esas columnas y en su lugar están "latitude" y "longitude".

        Para el analisis de los datos, vamos a obviar que hay un error en la documentación y utilizaremos las coordenadas en nuestro beneficio.

        Para los datos "contact_month" y "contact_year", aprovecharé "date" que es la unión de ambos, procederé a crear esas dos columnas nuevas, con los nombres propuestos y en cada una colocaremos el dato correspondiente. También aprovechó para sacar el día. Tres columnas en total.

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

        3. Datos

            Tras realizar la correcta unión entre las tablas, procedo a realizar lo siguiente:

            1. Corrigo los tipos de datos:
                1. `Object -> Float`:  En las String que deberían ser Float, cambio la `,` por el `.`.
                2. `Object -> Int`: En los datos que deberían ser INT, los transformo.
                3. `Object -> Fechas`: Aquí me he complicado un poco la existencia, pero las dos fechas que existen `date` y `dt_customer`, las he dejado en formato date.

            2. Relleno Huecos Vacíos
                
                Este apartado ha sido complicado, muchas horas pensando cual es el mejor proposito. Al final he decidido hacer lo siguiente:

                Para los valores `age`, `default` y `loan` he decidido dejarlo como NaN, ya que me afectaban a la hora de utilizar la funcion `.describe().T`  
                  
                Para las columnas `job`, `marital` y `education`, he asignado un valor de `unknown`.  

                En el caso del `euribor3m`, he visto una gran relación entre la posición del indice junto con los valores `emp.var.rate`, `cons.price.idx` y `cons.conf.idx`.
                
                Cómo son muchos los datos faltantas (9256), he decidido usar esa relación que he visto para rellenar la mayoría de ellos. Para ello he realizado un grupo de las tres condiciones que pienso son relacionales, y he rellenado anterior y posteriormente el euribor segun los datos previos y posteriores al mismo.

                Esto ha reducido los campos vacíos a 471, ya que `cons.price.idx` tenía el mismo gap.

                Tanto para los valores de `euribor3m` y `cons.price.idx` restantes, he procedido a rellenar los campos según los datos anteriores y posteriores.

                Pienso que en el fondo, no se debería nunca rellenar un dato que no sabemos a ciencia cierta que es certero, pero la relación es tan fuerte que creo que el dato debe ser el correspondiente, o muy proximo a el. Y estos datos son interesantes para el resultado del informe.


## Conclusiones

En el punto `4.2 Éxito Por Campaña` observamos claramente que la variación del `euribor3m` ha repercutido en la cantidad de suscripciones, según subía el valor del `euribor3m` disminuian las suscripciones, y observamos como hay dos momentos en el que el `euribor3m` desciende un poco y vuelven a generarse más suscripciones.

En el punto `4.3 Clientes Suscritos`, observamos que:   

    - La mayor cantidad de contrataciones está en una edad comprendida entre los 20 y los 60 años.  
    - Los clientes que más se suscriben son `students` y `retired`  
    - El estado `marital`, `education`, `housing`, `loan` no afecta a la decisión de la contratación.  

En conclusión, los clientes son más atraídos al producto cuando el `euribor3m` desciende.
    

