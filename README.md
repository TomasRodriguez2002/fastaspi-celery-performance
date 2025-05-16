# FastAPI + Celery + Redis + Docker - Tareas asíncronas y procesamiento concurrente

Este proyecto es un experimento práctico para entender y demostrar cómo manejar tareas intensivas de manera asíncrona y distribuida usando **FastAPI, Celery, Redis y Docker**.

El objetivo es simular un escenario donde una API debe procesar múltiples tareas IO-bound (como llamadas HTTP que demoran varios segundos) y comparar cómo se comporta de manera **secuencial** vs. **paralela usando Celery**.

## 🚀 Tecnologías utilizadas

- **FastAPI**: API web ligera y de alto rendimiento.
- **Celery**: Sistema de cola de tareas distribuido.
- **Redis**: Usado como broker y backend de Celery.
- **Docker / Docker Compose**: Para orquestar y aislar los servicios.

## ⚙️ Arquitectura
```
+----------------+       +----------------+      +----------------+
|  FastAPI App   |  ---> |     Redis      | ---> | Celery Worker  |
| (fastapi_app)  |       |    (redis)     |      |(celery_worker) |
+----------------+       +----------------+      +----------------+
```

- **FastAPI App** expone endpoints REST para ejecutar las tareas.
- **Redis** actúa como intermediario, gestionando el estado y la cola de tareas.
- **Celery Worker** procesa las tareas en paralelo (con 5 workers de concurrencia).

## 📍 Endpoints disponibles

- `GET /sequential`  
  Ejecuta 10 tareas IO-bound (llamadas a `https://httpbin.org/delay/3`) de manera **secuencial**.
  
- `GET /sequential_celery`  
  Ejecuta las mismas 10 tareas pero usando **Celery**, distribuyéndolas en paralelo.

- `GET /result/{task_id}`  
  Consulta el resultado de una tarea lanzada desde el endpoint `/sequential_celery`.

Accede a la documentación interactiva de la API en:  
[http://localhost:8000/docs](http://localhost:8000/docs)

## 📊 Resultados de la prueba

| Método                | Tiempo de ejecución |
|-----------------------|---------------------|
| Secuencial            |  39.54 segundos     |
| Asíncrono con Celery  |  8.58 segundos      |

### ✅ Mejora obtenida

Reducción del **78.29%** en el tiempo total, logrando un **x4.6 de aceleración** simplemente aplicando procesamiento distribuido con **Celery** y **Redis**.

## 🚢 Cómo correr el proyecto

Clonar el repositorio y ejecutar:

```bash
docker-compose up
```

La API estará disponible en:
http://localhost:8000/docs

## Próximos pasos
- Integrar Flower para monitorizar en tiempo real el estado y rendimiento de los workers y las tareas.

- Explorar escalabilidad horizontal con múltiples workers.
