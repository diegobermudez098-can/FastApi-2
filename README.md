# API de la Tienda (FastAPI)

Guía 2 – APIs con FastAPI · SENA ADSO · Evidencia GA2-AA3-EV04

API REST de una tienda con **productos** y **categorías**. Implementa el CRUD completo
(crear, leer, actualizar, eliminar) con modelos Pydantic y routers separados por recurso (`APIRouter`).
Los datos viven en memoria: al reiniciar el servidor vuelven a los valores iniciales.

**Autor:** Diego Andrés Bermúdez

## Estructura

```
tienda-api/
|-- main.py                 <- crea la app e incluye los routers
|-- requirements.txt
|-- routers/
|   |-- __init__.py         <- marca la carpeta como paquete
|   |-- productos.py        <- modelo + CRUD de productos
|   |-- categorias.py       <- modelo + CRUD de categorías
```

## Instalación y ejecución

Requiere Python 3.9 o superior.

```bash
# 1. Crear y activar el entorno virtual
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate     # Linux / macOS

# 2. Instalar dependencias
pip install -r requirements.txt

# 3. Ejecutar el servidor
uvicorn main:app --reload
```

Abrir la documentación interactiva: http://127.0.0.1:8000/docs

## Endpoints

| Método | Ruta                       | Descripción              | Éxito | Errores |
|--------|----------------------------|--------------------------|-------|---------|
| GET    | `/`                        | Mensaje de bienvenida    | 200   | –       |
| GET    | `/productos`               | Listar productos         | 200   | –       |
| GET    | `/productos/{producto_id}` | Obtener un producto      | 200   | 404     |
| POST   | `/productos`               | Crear producto           | 201   | 422     |
| PUT    | `/productos/{producto_id}` | Actualizar producto      | 200   | 404, 422|
| DELETE | `/productos/{producto_id}` | Eliminar producto        | 200   | 404     |
| GET    | `/categorias`              | Listar categorías        | 200   | –       |
| GET    | `/categorias/{categoria_id}` | Obtener una categoría  | 200   | 404     |
| POST   | `/categorias`              | Crear categoría          | 201   | 422     |
| PUT    | `/categorias/{categoria_id}` | Actualizar categoría   | 200   | 404, 422|
| DELETE | `/categorias/{categoria_id}` | Eliminar categoría     | 200   | 404     |

## Cuerpos de ejemplo (JSON)

Producto (`POST /productos`, `PUT /productos/{id}`):

```json
{ "nombre": "Webcam HD", "precio": 99000, "categoria": "Perifericos" }
```

Categoría (`POST /categorias`, `PUT /categorias/{id}`):

```json
{ "nombre": "Gaming" }
```

El `id` lo genera el servidor; el cliente no lo envía.

## Cómo probar los errores

- **404:** `GET /categorias/999` → `{"detail": "Categoria no encontrada"}`
- **422:** `POST /categorias` con cuerpo `{}` (falta `nombre`), o `POST /productos` con `"precio": "abc"`.
