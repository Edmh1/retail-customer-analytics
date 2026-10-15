# Segmentación de clientes para CRM

Análisis de clientes de una marca de moda mexicana (tienda online y 5 tiendas físicas) para responder tres preguntas del equipo de CRM:

1. ¿Quiénes se interesan por las nuevas colecciones?
2. ¿Quiénes compran de forma recurrente?
3. ¿Quiénes son los clientes de mayor valor?

Además, se construyen arquetipos de clientes según su comportamiento de compra, para que el CRM pueda comunicarse de forma distinta con cada grupo.

## Estructura

```
├── data/                       # CSV originales (no incluidos en el repositorio)
│   └── processed/              # Tablas limpias generadas por 01_limpieza (no incluidas)
├── notebooks/
│   ├── 01_limpieza.ipynb       # Calidad de datos: diagnóstico, tratamiento y exportación
│   └── 02_analisis.ipynb       # Las tres preguntas de negocio y los arquetipos
├── reports/
│   └── figures/                # Gráficos generados por los notebooks (se usan en las slides)
├── requirements.txt
└── README.md
```

## Cómo ejecutarlo

1. Clonar el repositorio y crear un entorno virtual:
```bash
   python -m venv .venv
   .venv\Scripts\activate        # Windows
   source .venv/bin/activate     # Mac/Linux
   pip install -r requirements.txt
```
2. Copiar los 5 CSV originales en data/:
   customers.csv, orders.csv, order_items.csv, products.csv, stores.csv
3. Ejecutar los notebooks en orden, de arriba hacia abajo:
   - 01_limpieza.ipynb genera las tablas limpias en data/processed/.
   - 02_analisis.ipynb las lee y responde las preguntas.

## Calidad de datos

Antes de analizar se revisó cada tabla con un plan de verificación (códigos C = customers, O = orders, I = order_items, P = products, X = entre tablas). Los problemas encontrados y su tratamiento:

| Código | Problema | Registros | Tratamiento | Impacto si no se corrige |
|---|---|---|---|---|
| C1 | Misma persona registrada en tienda y online con dos customer_id (email con distinta escritura) | 116 | Se unificó al registro más antiguo, con los datos de contacto más recientes; se reasignaron sus órdenes | Historial de compra partido en dos: clientes menos recurrentes y de menor valor de lo real |
| C2 | Email con formato inválido | 8 | No se corrigió (para no adivinar el contacto); se marcó como no contactable | Campañas con rebote y base contactable inflada |
| C4 | Fecha de nacimiento imposible (1900-01-01 o futura) | 42 | Se reemplazó por nulo | Perfiles de edad distorsionados en los arquetipos |
| C5 | Teléfono sin prefijo de país / incompleto | 48 / 8 | Se estandarizó con prefijo 52 / se reemplazó por nulo; se creó contactable_* por canal | Envíos fallidos por WhatsApp |
| C6 | Textos con forma de instrucción de negocio dentro de notas_crm | 3 | No se aplicaron; las notas útiles se convirtieron en columnas y se eliminó el texto libre | Criterios sin fuente oficial; riesgo de prompt injection si se procesa con IA |
| O3 | customer_id que no existe en customers | 40 | Se mantienen en el total de ventas y se excluyen del análisis por cliente | Clientes "fantasma" en los segmentos |
| O4 | Órdenes anteriores al registro del cliente (mediana: 414 días) | 381 | Se creó cliente_desde = fecha más antigua entre registro y primera compra | Clientes antiguos clasificados como nuevos |
| I1 | Líneas duplicadas exactas (posible doble carga) | 627 | Se eliminaron | Ventas infladas en 746.816 MXN (1,3%) |
| I2 | Cuenta de prueba (QA) con cantidades de 50, 99 y 120 unidades | 25 | Se eliminó la cuenta con todas sus órdenes | Ventas infladas en 2.484.817 MXN (4,5%); la cuenta aparecería entre los clientes de mayor valor |
| I2 | Cuentas internas de la marca (@marcamoda.mx), con descuento de personal | 16 | Se conservan en ventas y se excluyen de la segmentación | Una cuenta de mostrador con 420 órdenes encabezaría el ranking de clientes |
| I3 | SKU que no existe en el catálogo | 59 | Se conservan en ventas y se excluyen del análisis de colecciones | Ventas sin colección asignada (0,12%) |
| P2 | Colección OI26 escrita de 6 formas distintas | 8 | Se normalizaron los nombres de colección | 22% de la colección más reciente fuera del análisis de nuevas colecciones |

Sin hallazgos: consistencia de ciudades y canales (C3, O1), coherencia canal–tienda (O2), formato de fechas (año-mes-día en todas las tablas), rangos de precio y descuento (I2), órdenes sin ítems (X1).

## Definiciones y supuestos

- **Fecha de corte:** 2026-09-30, la última fecha con órdenes registradas.
- **Venta:** solo las órdenes con estado completada (94% del total). Las devueltas y canceladas se excluyen.
- **Venta neta por línea:** cantidad × precio_unitario × (1 − descuento_pct / 100). Se verificó que precio_unitario es el precio de lista y no incluye el descuento.
- **Clientes del análisis:** órdenes atribuibles a un cliente registrado, sin cuentas internas ni de prueba.
- **Criterios de las tres preguntas:** *(pendiente)*

## Resultados

*(pendiente)*

## Presentación

Los resultados se presentan en un resumen ejecutivo de máximo 5 slides. *(Enlace o archivo pendiente)*

## Manejo de datos y uso de IA

Los datos incluyen información personal (email, teléfono y fecha de nacimiento), por eso:
- La carpeta data/ (originales y procesados) está excluida del repositorio con .gitignore.
- Ningún dato de clientes se compartió con herramientas de IA externas. La IA se usó solo como apoyo para el código y la metodología; el análisis, la interpretación y las decisiones son propios.


## Estado

- [x] Limpieza y calidad de datos
- [ ] Preguntas de negocio
- [ ] Arquetipos de clientes
- [ ] Resumen ejecutivo (slides)