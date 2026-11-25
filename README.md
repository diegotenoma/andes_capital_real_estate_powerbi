# 🏙️ Andes Capital Real Estate – Dashboard de Ventas Inmobiliarias

Reporte en Power BI que analiza las ventas de propiedades de Andes Capital en Ciudad de México y Bogotá durante 2023–2024: ingresos, mix de propiedades, canales de venta, comisiones y recompra de clientes por cohorte.

Archivo principal: PBI_Andes_Capital_Real_Estate.pbix

📌 Resumen de resultados
Métrica	Valor
Ingreso total	$6,012.5 M
Ventas	8,500
Ticket promedio	$707,353
Comisión total	$200.6 M (3.34 % del ingreso)
Crecimiento YoY 2024 vs 2023	+11.1 %
Clientes compradores	3,139 (77.1 % con recompra)
Precio de venta / precio publicado	93.0 %

Hallazgos clave

Casa es el tipo con mayor revenue (37.3 %); Comercial tiene el mayor ticket (~$1.8 M) y el mayor crecimiento (+18.4 % YoY).
Ciudad de México concentra el 53.9 % del ingreso; Bogotá el mayor número de ventas.
El canal Corredor aporta el 72.8 % del ingreso y el 87.8 % de las comisiones (4.0 % vs 1.5 % del canal Directo).
El segmento Primera vez genera el 62.9 % del ingreso y tiene la mayor recompra (82 %).
T4 es el trimestre más fuerte; T3 el más débil.
🧱 Modelo de datos

Esquema estrella con una tabla de hechos y tres dimensiones.

dim_clientes (1) ──< hecho_ventas_propiedades >── (1) dim_propiedades
                              │
                         dim_fecha
Tabla	Tipo	Descripción
hecho_ventas_propiedades	Hechos	Una fila por venta: fecha, cliente, propiedad, precio, canal, % y monto de comisión
dim_propiedades	Dimensión	Tipo, ciudad, barrio, habitaciones, m², precio publicado, categoría
dim_clientes	Dimensión	Segmento comprador, país, ciudad, primera compra, cohorte y banderas de recompra (3M/6M/12M)
dim_fecha	Dimensión	Calendario 2023–2024 (año, mes, año-mes)
Medidas	Tabla de medidas	Todas las medidas DAX del reporte

Relaciones: hecho_ventas_propiedades[id_propiedad] → dim_propiedades y hecho_ventas_propiedades[id_cliente] → dim_clientes (muchos a uno, filtro simple).

Medidas DAX principales
Medida	Definición
Ingreso Total	SUM(precio_venta)
Ventas	DISTINCTCOUNT(id_venta)
Ticket Promedio	Ingreso Total / Ventas
Comisiones Totales	SUM(monto_comision)
Comisión Promedio %	AVERAGE(porcentaje_comision)
Crecimiento YoY %	Ingreso del año vs año anterior
Clientes Compradores / Clientes con Recompra / % Clientes con Recompra	Base y tasa de recompra
Ventas de Recompra / % Ventas de Recompra	Ventas posteriores a la primera compra del cliente
Retención 3M / 6M / 12M	% de clientes que recompran dentro de la ventana desde su primera compra
% del Publicado	Precio de venta / precio publicado
Columnas calculadas (dim_clientes)

Primera Compra, Cohorte (año + trimestre), Recompró 3M, Recompró 6M, Recompró 12M.

📂 Fuentes de datos

Tres archivos CSV cargados con Power Query:

hecho_ventas_propiedades.csv
dim_propiedades.csv
dim_clientes.csv

▶️ Cómo usarlo
Instala Power BI Desktop (Windows).
Clona este repositorio y abre PBI_Andes_Capital_Real_Estate.pbix.
Si Power BI no encuentra los CSV, actualiza las rutas del origen de datos y pulsa Actualizar.
🔍 Notas de calidad de datos / mejoras pendientes
dim_clientes incluye una fila de encabezado sin promover (segmento_comprador aparece como un cliente más). Falta aplicar Promover encabezados.
dim_fecha no tiene relación activa con la tabla de hechos; las medidas YoY y de retención dependen de ella y conviene validarla.
La consulta de hecho_ventas_propiedades define pasos de deduplicación y eliminación de columnas que el paso final no utiliza.
El modelo no especifica moneda; si CDMX y Bogotá están en MXN y COP, los totales combinados mezclan monedas.
Las rutas de los CSV están fijas a una máquina local.
🗺️ Roadmap sugerido
[ ] Parametrizar rutas y/o conectar a una base de datos
[ ] Normalizar moneda (MXN / COP / USD)
[ ] Corregir relación con dim_fecha y encabezado de dim_clientes
[ ] Agregar análisis de margen por canal y por barrio
🛠️ Stack

Power BI Desktop · DAX · Power Query (M) · CSV

👤 Autor

Proyecto desarrollado por Diego Tenorio Martinez.
