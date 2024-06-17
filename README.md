1.	El proceso de desarrollo del modelo relacional comenzó con la importación de datos desde un archivo, disponible en el siguiente enlace:
 A partir de estos datos, se crearon y estructuraron tablas individuales sin la necesidad de utilizar tablas puente en esta etapa inicial.
2.	La construcción del modelo relacional se inició con la importación de datos, los cuales se organizaron en tres tablas principales, cada una con sus respectivas columnas:
Invoice: Contiene información relacionada con las facturas, incluyendo las columnas Branch, City, COGS, Customer Type, Date, Gender, Gross Income y Gross Margin Percentage.
Customer: Almacena datos sobre los clientes, con columnas como Customer Type, Gender, Invoice ID, Payment y Rating.
Product: Esta tabla se enfoca en los productos vendidos, con las columnas Invoice ID y Product Line.
Estas tablas se relacionaron entre sí en una estructura uno a uno, estableciendo relaciones que reflejan la conexión de los datos según el contexto del negocio.



Para la creación de medidas calculadas, se optó por agregar una nueva tabla denominada "Medidas". Dentro de esta tabla se implementaron fórmulas específicas para calcular métricas clave. Estas medidas se aplicaron sobre los datos existentes en las tablas previamente creadas, permitiendo así obtener análisis más detallados y relevantes para la toma de decisiones.

•	La siguente formula tiene el propósito de calcular el promedio de la columna Rating para las ciudades específicas: Boston, New York y Washington. V
% RatingPorCiudad = 
CALCULATE(
    AVERAGE('Invoice'[Rating]),
    FILTER(
        'Invoice',
        'Invoice'[City] IN {"Boston", "New York", "Washintong"}
    )
)



Usamos la misma fórmula para calcular el promedio por genero.-
% RatingPorGenero = 
CALCULATE(
    AVERAGE('Invoice'[Rating]),
    FILTER(
        'Invoice',
        'Invoice'[Gender] IN {"Female", "Male"}
    )
)


•	Cuenta el número de filas en la columna Customer type de la tabla Invoice:  
TiposClienes = COUNT(Invoice[Customer type])
•	Cuenta el número total de filas en la columna Invoice ID de la tabla Custumer.
CantidadClienes = COUNT(Costumer[Invoice ID])
•	Cuenta el número de filas en la columna gross income de la tabla Invoice.
CantidadIngresos = COUNT(Invoice[gross income])
•	Cuenta el número total de registros en la columna Customer type de la tabla Invoice.
miembros cantidad = COUNT(Invoice[Customer type])


•	Calcula el número de clientes masculinos y femeninos contando las filas donde Gender es "Male" y ”Female”:
CantidadHombres = CALCULATE([CantidadClienes],
Costumer[Gender]="Male")

CantidadMujeres = CALCULATE([CantidadClienes],
Costumer[Gender]="Female")

•	Calcula el número de clientes normales y member contando las filas donde custumer type es "Member" y ”Normal”:
CantidadMember = CALCULATE([TiposClienes],
Invoice[Customer type]="Member")

CantidadNormal = CALCULATE([TiposClienes],
Invoice[Customer type]="Normal")

•	Las fórmulas en DAX cuentan el número de filas en la tabla Invoice donde la columna City es igual a "Boston", "New York" y "Washington", respectivamente. Estas medidas se utilizan para calcular cuántas facturas están asociadas con cada una de estas ciudades específicas, lo cual es útil para analizar y comparar el volumen de ventas o transacciones en diferentes ubicaciones geográficas.
CantidadProductBoston = COUNTROWS(FILTER(Invoice, Invoice[City] = "Boston")) 
CantidadProductNewYork = COUNTROWS(FILTER(Invoice, Invoice[City] = "New York")) 
CantidadProductWashintong = COUNTROWS(FILTER(Invoice, Invoice[City] = "Washintong")) 


•	Suma los valores de la columna Quantity en la tabla Invoice.  Para obtener la cantidad total de productos vendidos.

Cantidadtotal vendida = SUM('Invoice'[Quantity])

•	Calcula la cantidad total de productos vendidos agrupados por la línea de productos.
CantidadVendidoProductLine = CALCULATE(
    SUM('Invoice'[Quantity]),
    ALLEXCEPT('Invoice', 'Invoice'[Product line])
) 

•	Estas formulas cuentan el número de transacciones en la tabla invoice que se realizaron con "Cash", "Credit card" y "Ewallet" respectivamente. Estas medidas son útiles para analizar y comparar la popularidad o el volumen de uso de cada método de pago entre los clientes. Al contar las filas correspondientes a cada método de pago, estas medidas permiten identificar cuáles son las formas de pago más utilizadas y pueden ayudar en la toma de decisiones estratégicas relacionadas con las preferencias de pago de los clientes.
Cash = COUNTROWS(FILTER(Invoice, Invoice[Payment] = "Cash")) 
Creditcard = COUNTROWS(FILTER(Invoice, Invoice[Payment] = "Credit card")) 
Ewallet = COUNTROWS(FILTER(Invoice, Invoice[Payment] = "Ewallet")) 


•	Cuentan el número de transacciones o registros en la tabla Producto para cada una de las líneas de productos especificadas. Estas medidas permiten analizar y comparar el volumen de ventas o la cantidad de transacciones por cada línea de producto, proporcionando una visión clara de qué categorías de productos son más populares o tienen mayores ventas. Esto es útil para identificar tendencias, ajustar estrategias de inventario y marketing, y tomar decisiones informadas basadas en la popularidad de cada línea de producto.
Electronic accessories = COUNTROWS(FILTER(Porduct, Porduct[Product line] = "Electronic accessories"))

Fashion accessories = COUNTROWS(FILTER(Porduct, Porduct[Product line] = "Fashion accessories")) 

Sports and travel = COUNTROWS(FILTER(Porduct, Porduct[Product line] = "Sports and travel")) 

Food and beverages = COUNTROWS(FILTER(Porduct, Porduct[Product line] = "Food and beverages")) 

Health and beauty = COUNTROWS(FILTER(Porduct, Porduct[Product line] = "Health and beauty"))

Home and lifestyle = COUNTROWS(FILTER(Porduct, Porduct[Product line] = "Home and lifestyle"))


•	Calcula el promedio de los valores en la columna "Rating" de la tabla o conjunto de datos llamado "Invoice".
Promedio de Rating = AVERAGE('Invoice'[Rating])

•	Calcula el promedio del ingreso bruto para cada valor único en la columna 'Payment' de la tabla 'Invoice', lo que proporciona una visión más detallada del rendimiento financiero según los diferentes métodos de pago utilizados. Esta medida es útil para analizar el comportamiento de los ingresos en relación con los métodos de pago específicos y puede ayudar en la toma de decisiones relacionadas con la gestión financiera y estrategias comerciales.

PromedioGrossIncomePorPago = 
CALCULATE(
    AVERAGE('Invoice'[gross income]),
    ALLEXCEPT('Invoice', 'Invoice'[Payment])
)

•	Calcula el promedio de la cantidad de productos vendidos para cada tipo de pago único en la tabla 'Invoice'. Es útil para entender el comportamiento de compra de los clientes según el método de pago utilizado y puede ayudar en la optimización de estrategias de precios y promociones.
PromedioQuantityPorPago = 
CALCULATE(
    AVERAGE('Invoice'[Quantity]),
    ALLEXCEPT('Invoice', 'Invoice'[Payment])
)


•	Calcula la suma de los ingresos brutos para cada ciudad única en la tabla 'Invoice', lo que proporciona una vista detallada de las ganancias por ubicación. Es útil para analizar el rendimiento financiero en diferentes ciudades y puede ayudar en la toma de decisiones relacionadas con la expansión del negocio, asignación de recursos y estrategias de marketing específicas para ciertas áreas geográficas.
GananciasPorCiudad = 
CALCULATE(
    SUM('Invoice'[gross income]),
    ALLEXCEPT('Invoice', 'Invoice'[City])
)


•	Calcula el promedio del rating de productos vendidos en las categorías de productos seleccionadas. Es útil para comprender la satisfacción del cliente en diferentes categorías de productos y puede ayudar en la toma de decisiones relacionadas con la gestión de inventario, marketing y desarrollo de productos.
Rating = 
CALCULATE(
    AVERAGE('Invoice'[Rating]),
    FILTER(
        'Invoice',
        'Invoice'[Product line] IN {"Electronic accessories", "Fashion accessories", "Health and beauty", "Home and lifestyle", "Food and beverages", "Sports and travel"}
    )
)

•	Calcula el promedio de calificación de compras realizadas por hombres.
RatingPromedioHombres = 
CALCULATE(
    AVERAGE(Invoice[Rating]),
    Invoice[Gender] = "Male"
)

•	Calcula el promedio de calificación de compras realizadas por mujeres.
RatingPromedioMujeres = 
CALCULATE(
    AVERAGE(Invoice[Rating]),
    Invoice[Gender] = "Female"
)


3.	Tabla Calendario: 
Antes de crear la tabla, determinamos las fechas mínima y máxima presentes en el conjunto de datos.   Y luego creamos nueva tabla utilizando la siguiente formula Dax: 

 TablaCalendario = CALENDAR(MIN(Invoice[Date]),MAX(Invoice[Date]))

Generar las Fechas en el Rango Especificado: Utilizando las funciones o herramientas disponibles, puedes generar un conjunto de fechas que abarque desde la fecha mínima hasta la fecha máxima. Esto puede hacerse mediante la generación de una secuencia de fechas o mediante la especificación de un intervalo de tiempo (por día, semana, mes, etc.).
Una vez creada la tabla de calendario, la relacionamos con la tabla Invoice ID  

