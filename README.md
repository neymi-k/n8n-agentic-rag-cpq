# Agentic B2B Workflow: RAG Support & Automated CPQ (n8n)



Este repositorio contiene un flujo avanzado de n8n diseñado para automatizar la atención al cliente B2B, la calificación de leads y la generación de propuestas comerciales (CPQ - Configure, Price, Quote) utilizando agentes de IA.

**Nota:** Por motivos de confidencialidad, los datos del cliente original, tokens, URLs de producción y credenciales han sido sanitizados y reemplazados por variables de entorno genéricas.

## Arquitectura y Características Principales

El flujo está construido sobre **n8n** integrando **LangChain** para el manejo de agentes y herramientas. Se divide en los siguientes módulos clave:

1. **Gestión de Concurrencia (Redis):** Implementación de un sistema de buffer y bloqueo (Locks) en Redis para manejar múltiples mensajes simultáneos del mismo usuario de forma ordenada.
2. **Memoria a Largo Plazo (PostgreSQL):** Almacenamiento del contexto de la sesión para mantener conversaciones fluidas.
3. **Guardrails de Seguridad:** Filtros de protección contra inyecciones de prompt y validación de alineación temática (Topical Alignment) para asegurar que el bot solo responda temas comerciales.
4. **Soporte Técnico Especializado (RAG):**
   - **Qdrant (Vector Store):** Búsqueda semántica para inyectar contexto técnico sobre productos, ingredientes y normativas.
   - **Cohere Reranker:** Optimización de los resultados recuperados.
   - **WP Search API:** Herramienta personalizada para obtener URLs exactas del catálogo en tiempo real.
5. **Cerebro Analítico y Enrutador Estratégico:** Un agente evalúa el "Nivel de Interés" y los "Datos Faltantes" (Nombre, Email, Empresa) para forzar la recolección de leads antes de avanzar a ventas.
6. **Integración CRM:** Verificación de leads existentes y creación de nuevos leads mediante llamadas API REST al CRM de la empresa.
7. **Motor CPQ (Propuesta Digital Automática):** Extracción de intenciones de compra del historial de chat para generar un presupuesto en formato JSON estructurado, cachearlo en Redis y enviar un enlace de validación por correo vía SMTP.

## Stack Tecnológico

* **Orquestación:** n8n, LangChain
* **LLMs:** OpenAI (GPT-4o, GPT-4o-mini)
* **Bases de Datos:** Qdrant (Vectores), PostgreSQL (Memoria), Redis (Caché y Locks)
* **Herramientas API:** WordPress Search API, CRM Custom API, SMTP

## Cómo importar este flujo

1. Clona este repositorio o descarga el archivo `n8n-rag-cpq-workflow.json`.
2. Abre tu instancia de n8n.
3. Ve a la vista de flujos (Workflows) y selecciona **"Import from File"**.
4. Configura tus propias credenciales para OpenAI, Qdrant, Redis y Postgres en los nodos correspondientes.
5. Ajusta las URLs del webhook y del CRM según tu entorno local o de pruebas.

## 💡 Casos de Uso Demostrados
* Prompt Engineering avanzado y uso de LangChain Output Parsers.
* Inyección dinámica de herramientas (Function Calling) en modelos LLM.
* Manejo de estado avanzado y lógica de enrutamiento condicional.
