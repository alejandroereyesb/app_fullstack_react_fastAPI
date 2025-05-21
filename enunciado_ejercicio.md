# Ejercicio: Dockerización de una aplicación Fullstack (FastAPI + React)

En este ejercicio, dockerizarás una aplicación fullstack que consta de un backend desarrollado con **FastAPI** y un frontend desarrollado con **React** utilizando **Vite**. El objetivo es completar los `Dockerfile` y el archivo `docker-compose.yml` necesarios para levantar la aplicación completa con Docker.

## Objetivos del ejercicio

1. Completar el `Dockerfile` para el frontend (React + Vite) que construya y sirva la aplicación utilizando **Nginx**.
2. Completar el archivo `docker-compose.yml` para orquestar los contenedores del backend, frontend y base de datos (PostgreSQL).
3. Verificar que la aplicación funcione correctamente accediendo a las URLs del frontend y backend.

---

## Archivos proporcionados

Se te entregarán los siguientes archivos:

1. **`backend/Dockerfile`**: Ya está completo y funcional. No necesitas modificarlo.
2. **`frontend/Dockerfile`**: Vacío. Debes completarlo para construir y servir la aplicación React con Nginx.
3. **`docker-compose.yml`**: Incompleto. Debes completarlo para orquestar los contenedores.

---

## Instrucciones

### 1. Backend (FastAPI)

Debes completar el `Dockerfile` para el frontend. Asegúrate de que el backend pueda conectarse a la base de datos PostgreSQL utilizando las variables de entorno definidas en el archivo `.env`.

### 2. Frontend (React + Vite)

Debes completar el `Dockerfile` para el frontend. Este debe:
- Construir la aplicación con Vite.
- Servir los archivos estáticos utilizando **Nginx**.

A continuación, se proporciona un ejemplo de cómo debería verse el `Dockerfile` completo:

```dockerfile
# filepath: frontend/Dockerfile
# Usa una imagen base de Node.js para construir la aplicación
FROM node:18 as build

# Establece el directorio de trabajo
WORKDIR /app

# Copia los archivos necesarios
COPY package*.json ./
COPY . .

# Instala las dependencias y construye la aplicación
RUN npm install && npm run build

# Usa una imagen base de Nginx para servir los archivos estáticos
FROM nginx:1.23

# Copia los archivos construidos al directorio de Nginx
COPY --from=build /app/dist /usr/share/nginx/html

# Expone el puerto 80
EXPOSE 80

# Comando para ejecutar Nginx
CMD ["nginx", "-g", "daemon off;"]
```

Este ejemplo te servirá como referencia para completar el archivo `frontend/Dockerfile`.

### 3. Archivo `docker-compose.yml`

Debes completar el archivo `docker-compose.yml` para orquestar los contenedores del backend, frontend y base de datos. Asegúrate de:
- Configurar las variables de entorno para el backend y la base de datos.
- Exponer los puertos necesarios para acceder al frontend y backend desde el navegador.

Investiga cómo configurar un archivo `docker-compose.yml` para orquestar múltiples servicios, incluyendo un backend, un frontend y una base de datos PostgreSQL.

### 4. Verificación

1. Construye y levanta los contenedores:
   ```bash
   docker-compose up --build
   ```

2. Accede a los servicios:
   - **Frontend**: [http://localhost:3000](http://localhost:3000)
   - **Backend**: [http://localhost:8000](http://localhost:8000)
   - **Documentación de la API** (Swagger): [http://localhost:8000/docs](http://localhost:8000/docs)

3. Verifica que el frontend pueda comunicarse con el backend y que los datos se almacenen correctamente en la base de datos.

---

## Notas

- Asegúrate de que los puertos `3000`, `8000` y `5432` estén libres en tu máquina antes de ejecutar el proyecto.
- Si encuentras problemas, revisa los logs de los contenedores:
  ```bash
  docker-compose logs <nombre_del_servicio>
  ```
- Si necesitas reiniciar los contenedores, usa:
  ```bash
  docker-compose down
  docker-compose up --build
  ```

¡Buena suerte!
