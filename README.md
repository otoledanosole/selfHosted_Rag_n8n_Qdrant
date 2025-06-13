# selfHosted_Rag_n8n_Qdrant_WebUI
Creating a Self Hosted RAG for testing and learning purposes using Self-hosted AI starter kit (by the n8n team) and Open WebUI as interface
--
El primer paso serà clonar el repositorio de git que vamos a usar para estas pruebas:

[Git Repository](https://github.com/coleam00/ai-agents-masterclass/tree/main/local-ai-packaged#self-hosted-ai-starter-kit-by-the-n8n-team)

---

1. Iremos al repositorio y copiaremos la url para clonar:

  <img width="697" alt="image" src="https://github.com/user-attachments/assets/e69721a3-6e09-4c5b-bb0c-a80e5b367429" />

2. Abriremos una consola CMD y nos ubicaremos en el directorio donde queremos clonar el repositorio.
3. Ejecutmos el comando: git clone https://github.com/coleam00/ai-agents-masterclass.git

---

Si entramos en el repositorio que hemos clonado, en la ruta ai-agents-masterclass\local-ai-packaged, encontraremos el fichero docker-compose.yml que es el fichero con el que Docker Desktop va a crear los contenedores necesarios.

Después de hacer el clonado con el comando git clone https://github.com/coleam00/ai-agents-masterclass.git

Nos ubicaremos en la carpeta donde esta el fichero docker: ai-agents-masterclass\local-ai-packaged

---

En esta carpeta encontramos un fichero que se llama .env.example, lo abriremos con un editor de texto o VSCode.

<img width="246" alt="image" src="https://github.com/user-attachments/assets/eb50c88a-063c-49c2-a5fa-019b88e9f147" />

Escribiremos el usuario y la contraseña para la base de datos de postgres y las claves de encriptación de n8n.

Lo guardaremos con el nombre .env

---

Abriremos el documento docker-compose.yml

Por defecto, el puerto de postgres esta oculto y solo lo usa n8n internamente. Esto no nos permite usar postgre para otros usos, por ejemplo, para almacenar el historial de las conversaciones con el usuario.

Para solucionarlo, buscaremos en el documento el servicio de postgre y anadiremos las siguientes lineas:

ports:

\- 5432:5432

<img width="361" alt="image" src="https://github.com/user-attachments/assets/bf51edf6-3946-47f6-9712-2f9bbcf7798b" />



Otro detalle importante de este documento es el comando que define Ollma como el modelo de embeding.

El comando base es: "sleep 3; OLLAMA_HOST=ollama:11434 ollama pull llama3.1; OLLAMA_HOST=ollama:11434 ollama pull nomic-embed-text"

- Dormimos durante 3 segundos
- Usamos Ollama 3.1 por defecto
- Usamos nomic como modelo de embeding

Si queremos cambiar/añadir otro modelo, iremos a la pagina: [Ollama Library](https://www.ollama.com/library) de donde podremos descargar diferentes modelos con diferentes cuantificaciones.

Por ejemplo, si elegimos gemma3

<img width="404" alt="image" src="https://github.com/user-attachments/assets/4e8f4c9e-76e3-4c96-ac0a-ca4da4a5370e" />

Podemos ver los diferentes modelos con sus cuantificaciones.

Para istalar una, buscaremos el comando correspondiente a la cuantificación que deseemos:

<img width="398" alt="image" src="https://github.com/user-attachments/assets/d1196a16-5b73-486c-b5c9-6abff3559c09" />

---

Ahora ya podemos generar el contenedor de Docker: docker compose --profile gpu-nvidia up  (Para usuarios con GPU Nvidia)

---

Si no hemos añadido el modelo en el Docker Compose, lo podemos instalar desde la consola de Docker.

1. Abriremos el contenedor de ollama y nos iremos a la pestaña EXEC:

<img width="452" alt="image" src="https://github.com/user-attachments/assets/91349a53-0457-4470-828e-8fa1f7bc154a" />

2. Ejecutaremos el comando: ollama pull [nombre del modelo]. Por ejemplo: ollama pull gemma3:4b

<img width="1070" alt="image" src="https://github.com/user-attachments/assets/b4c68b6a-c967-4dd7-b730-32980b5d461f" />

---

Comprobaremos que Docker tiene acceso a la GPU de Nvidia. Primero nos aseguraremos de tener los drivers CUDA instalados ejecutando en una CMD: nvidia-smi

Después comprobaremos el acceso desde Docker ejecutando el comando que aparece en esta pagina: [GPU support in Docker Desktop for Windows](https://docs.docker.com/desktop/features/gpu/)

---

# Guia Rapida para inicializar el contenedor

1. El primer paso serà abrir el contenedor de n8n http://localhost:5678. Solo hay que hacer esto la primera vez. No estas creando una cuenta real, es solo una cuenta local para el servicio

2. Abriremos el Workflow incluido en el ejemplo: Local RAG AI Agent

3. Buscaremos Ollama Chat Model, lo abriremos y crearemos una nueva credencial. En el campo Base URL pondremos: http://ollama:11434

4. Buscaremos Postgre Chat Memory, lo abriremos y crearemos una nueva credencial.

   <img width="601" alt="image" src="https://github.com/user-attachments/assets/58c8fef3-6e2e-4fc9-9da1-f65de433c6b4" />

   Postgre creará automaticamente el nombre de la tabla necesaria.

5. Buscaremos Qdrant Vector Store, lo abriremos y crearemos una nueva credencial.

   <img width="602" alt="image" src="https://github.com/user-attachments/assets/b1b04cd4-d786-4a5f-b51b-a442942a7793" />

   La API Key puede ser cualquier valor ya que el servicio esta corriendo localmente, en nuestro caso usaremos la N8N_ENCRYPTION_KEY que hemos usado en el .env

   Si abrimos http://localhost:6333/dashboard podremos ver nuestra base de datos de Qdrant donde podremos ver todos los vectores que tenemos creados.

6. Configuraremos las credenciales de las diferentes herramientas de Google utilizadas. Para ello, seguiremos las instrucciones de n8n: [Google: OAuth2 single service](https://docs.n8n.io/integrations/builtin/credentials/google/oauth-single-service/?utm_source=n8n_app&utm_medium=credential_settings&utm_campaign=create_new_credentials_modal)

7. Abre http://localhost:3000/ en el navegador e inicia sesión. Solo hay que hacer esto la primera vez. No estas creando una cuenta real, es solo una cuenta local para el servicio.

   En el menu izquierdo, ves a Tu usuario en la parte inferior -> Administración -> Funciones -> Añade una función con el simbolo + -> Pon un nombre y pega el codigo n8n_pipe.py ubicado en ai-agents-masterclass\local-ai-packaged.

   También la podemos obtener aquí [OpenWebUI N8N Pipe](https://openwebui.com/f/coleam/n8n_pipe)

8. En n8n, abriremos el bloque Webhook y copiaremos el Production URL.
9. En Open WebUI, seleccionaremos el engranaje al lado de la función y pegaremos el Production URL en el campo correspondiente. ¡Importante: Hay que substituir localhost por el nombre del contenedor de n8n, en este caso: n8n!
10. Habilitamos la función de Open WebUI.
   


