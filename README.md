# Segmentación de clientes para CRM

Análisis de clientes de una marca de moda (tienda online y 5 tiendas físicas) para responder tres preguntas del equipo de CRM:

1. ¿Quiénes se interesan por las nuevas colecciones?
2. ¿Quiénes compran de forma recurrente?
3. ¿Quiénes son los clientes de mayor valor?

Además, se construyen arquetipos de clientes según su comportamiento de compra.

## Estructura

```
├── data/          # CSV originales (no incluidos en el repositorio)
├── notebooks/
│   └── analisis.ipynb
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
2. Copiar los 5 CSV originales en la carpeta `data/`:
   `customers.csv`, `orders.csv`, `order_items.csv`, `products.csv`, `stores.csv`
3. Abrir y ejecutar `notebooks/analisis.ipynb` de arriba hacia abajo.

## Manejo de datos

Los datos incluyen información personal (email, teléfono y fecha de nacimiento), por eso:
- La carpeta `data/` está excluida del repositorio con `.gitignore`.
- Ningún dato de clientes se compartió con herramientas de IA externas. La IA se usó solo como apoyo para el código y la metodología.

## Estado

🚧 En desarrollo