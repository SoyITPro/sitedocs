---
title: Dejé Google por SearXNG y NO PIENSO VOLVER
date: 2026-06-27
draft: false
categories:
  - Docker
tags:
  - homelab
  - self-hosting
---
# ¿Qué es SearXNG y cómo te protege?

**SearXNG** es un **metabuscador** libre y de código abierto. Su principal objetivo es ofrecer resultados de búsqueda sin comprometer tu privacidad . En lugar de rastrear tus datos o crear un perfil sobre ti, actúa como un intermediario que realiza búsquedas en varios motores de búsqueda de forma anónima .

## ¿Para qué sirve?

SearXNG sirve para realizar búsquedas en internet de forma privada y sin ser rastreado . Agrega los resultados de más de 70 servicios de búsqueda diferentes (como Google, DuckDuckGo, Wikipedia, etc.) en una sola página . Esto te permite:

- **Obtener resultados diversos** de múltiples fuentes.
- **Evitar la burbuja de filtros** que crean los motores de búsqueda tradicionales al personalizar los resultados según tu historial.
- **No ver anuncios** ni contenido de rastreo .

## ¿Cómo ayuda en la privacidad?

La protección de la privacidad es la característica fundamental de SearXNG. Funciona de varias maneras:

1.  **Eliminación de datos privados**: SearXNG elimina los datos de identificación de tus solicitudes de búsqueda (como cookies y la información de tu navegador) antes de enviarlas a los motores de búsqueda externos . Esto evita que los motores de búsqueda puedan rastrearte o crear un perfil de tus hábitos.
2.  **Anonimato de IP**: Las búsquedas se realizan desde la dirección IP de la instancia de SearXNG, no desde tu IP personal. Esto se puede llevar un paso más allá configurando SearXNG para que use un proxy o la red Tor .
3.  **Sin publicidad ni rastreadores**: SearXNG no muestra anuncios ni contiene rastreadores de terceros, a diferencia de la mayoría de los motores de búsqueda .
4.  **Código abierto**: Todo su código es público y puede ser revisado por cualquier persona, lo que garantiza la transparencia sobre cómo se manejan los datos .

## Implementación con Docker

Una de las formas más rápidas y populares de desplegar tu propia instancia privada de SearXNG es usando Docker . Aquí tienes una guía básica para hacerlo.

### Requisitos previos

- Tener **Docker** y **Docker Compose** instalados en tu sistema.

### Pasos de instalación

1.  **Descargar archivos del repositorio oficial**: El proyecto `searxng` proporciona todos los archivos necesarios. https://docs.searxng.org/

    ```bash
    mkdir searxng
    cd searxng
    
    #Descargar archivos
    curl -fsSL -O https://raw.githubusercontent.com/searxng/searxng/master/container/docker-compose.yml
    
    curl -fsSL -O https://raw.githubusercontent.com/searxng/searxng/master/container/.env.example
    ```


2.  **Configurar el archivo `.env`**: Edita el archivo `.env` para establecer tu nombre de dominio (`SEARXNG_HOSTNAME`) y una dirección de correo electrónico para el proxy inverso (Caddy) .

3.  **Personalizar la configuración (Opcional)**: Puedes editar el archivo `searxng/settings.yml` para ajustar SearXNG a tus necesidades (por ejemplo, cambiar los motores de búsqueda habilitados) .

4.  **Iniciar el contenedor**: Usa Docker Compose para iniciar todos los servicios en segundo plano.

    ```bash
    docker compose up -d
    ```
### Componentes incluidos

El repositorio `searxng` orquesta dos contenedores principales para una instalación completa :

| Componente           | Función                                                                                                                                     |
| :------------------- | :------------------------------------------------------------------------------------------------------------------------------------------ |
| **SearXNG**          | El propio metabuscador.                                                                                                                     |
| **Valkey (o Redis)** | Una base de datos en memoria utilizada para almacenar en caché y gestionar la limitación de peticiones, mejorando el rendimiento.           |

### Gestión y mantenimiento

- **Ver logs**: Para solucionar problemas, puedes ver los registros de todos los servicios con `docker compose logs -f` o de uno en concreto, como `docker compose logs -f searxng` .
- **Actualizar**: Para actualizar a la última versión, ejecuta:

    ```bash
    docker compose pull
    docker compose up -d
    ```

## 📽️ Video completo
{{< youtube CCYQmSnZBiY >}}
