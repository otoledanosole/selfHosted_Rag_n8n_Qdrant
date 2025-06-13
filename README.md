# selfHosted_Rag_n8n_Qdrant
Creating a Self Hosted RAG for testing and learning purposes using Self-hosted AI starter kit (by the n8n team)
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



El siguiente paso serà instalar NVIDIA Container Tools. Para ello iremos a:  https://github.com/ollama/ollama/blob/main/docs/docker.md

