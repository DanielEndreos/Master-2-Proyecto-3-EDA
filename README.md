# Master-2-Proyecto-3-EDA
Proyecto EDA con Python

## Analisis Visual sobre el enunciado del ejercicio
1. Puntos a Sanear

    1. Discrepancia entre las columnas reales / enunciado:
        
        - *Ref.:* **bank-additional.csv**

        En el enunciado del proyecto, menciona las columnas "contact_month" y "contact_year"
        En el documento **.csv** facilitado, no existen esas columnas y en su lugar están "latitude" y "longitude".

        Para el analisis de los datos, vamos a obviar que hay un error en la documentación y utilizaremos las coordenadas en nuestro beneficio.

        Para los datos "conctact_month" y "contact_year", aprovecharé "date" que es la unión de ambos, procederé a crear dos columnas nuevas, con los nombres propuestos y en cada una colocaremos el dato correspondiente.

## Analisis Durante el ejercicio

1. Puntos a Sanear

    2. Headers **.CSV** y **.XLSX**
        
        - *Ref.:* [bank-additional.csv](https://github.com/DanielEndreos/Master-2-Proyecto-3-EDA/blob/main/DatosProyecto/bank-additional.csv)
        - *Ref.:* **customer-details.xlsx**

        1. Columna "id"
            
            En ambos documentos se nombra de manera distinta, eso es un problema a la hora de realizar un `pd.merge()` entre ambos archivos. Ya que necesitamos un punto de unión entre archivos y el mejor punto de unión es la "id" ya que es la relación entre ambos documentos, no podemos utilizar la supuesta "PK" ya que podrían estar introducidos en otro orden.

            Es por ello, que modificaré el nombre de esta columna a "id" para mantener la relación de textos con las demás cabeceras.

        2. Columnas
            
            1. Lower Case:
                Para evitar problemas con el case sensitive, he decidido llevar todos los textos de los títulos a minusculas mediante `lower()`.

            2. Strip:
                Para evitar problemas en el que si hay alguna columna con espacios antes/después del texto, y con ello, me genere problemas en las consultas, antes de unir con `pd.merge()` he realizado un `.str.strip()` de las columnas.



        
    
    Se observa que ambos archivos, tanto el **.CSV** como el **.XLSX** tienen una columna principal sin nombre


