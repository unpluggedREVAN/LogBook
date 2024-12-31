### **Descripción Técnica del Sistema Web: DECIPHER**

---

**Nombre del Sistema:**  
**DECIPHER**  
(_Dynamic Extraction and Compilation Interface for Processed Human-readable Evaluations and Reports_)

---

### **Propósito**

DECIPHER es una plataforma web diseñada para facilitar la gestión, visualización y exportación de información generada en consultorías de inteligencia artificial y análisis de datos. Su objetivo principal es servir como un **hub interactivo y dinámico**, permitiendo a los clientes cargar, visualizar y personalizar proyectos entregados en formato comprimido (`.zip` o `.rar`) con una estructura estándar.

El sistema es ligero, autónomo y no requiere infraestructura de backend ni bases de datos, lo que lo hace ideal para implementaciones sencillas, económicas y escalables en plataformas de hosting estático.

---

### **Principales Funcionalidades**

1. **Carga de Proyectos**:
    
    - Los usuarios suben un archivo `.zip` o `.rar` generado por la empresa consultora.
    - DECIPHER valida y procesa la estructura estándar del archivo, que incluye carpetas como `agents`, `intel`, y `repo`.
2. **Procesamiento y Visualización Dinámica**:
    
    - Los archivos dentro del proyecto son interpretados dinámicamente:
        - **Markdown (`.md`)**: Se renderiza como texto estructurado.
        - **JSON**: Se procesa para generar gráficos.
        - **CSV**: Se convierte en tablas.
        - **PDF**: Se muestran como enlaces descargables o vistas embebidas.
    - Cada carpeta genera una sección específica en la interfaz web.
3. **Edición y Personalización**:
    
    - Los usuarios pueden agregar contenido nuevo, como agentes, gráficos o descripciones, directamente desde la interfaz.
    - Los cambios se almacenan localmente en el navegador usando **Local Storage**.
4. **Exportación de Proyectos**:
    
    - Los usuarios pueden exportar el proyecto actualizado como un nuevo archivo `.zip` o `.rar`, que respeta la estructura original e incluye los cambios realizados.
5. **Compatibilidad Totalmente Estática**:
    
    - DECIPHER está diseñado para ser hospedado en plataformas como **Vercel** o **GitHub Pages**, funcionando completamente en el frontend sin necesidad de servidores o bases de datos.

---

### **Arquitectura del Sistema**

#### **Frontend**

- **Framework:**
    - **Next.js**: Permite desarrollar una aplicación web modular y eficiente, con capacidades de renderizado estático.
- **Librerías**:
    - **JSZip**: Para descomprimir y generar archivos comprimidos directamente en el navegador.
    - **react-markdown**: Para procesar y renderizar contenido markdown.
    - **Chart.js**: Para gráficos interactivos.
    - **Papaparse**: Para procesar archivos CSV en tablas.
    - **TailwindCSS** (opcional): Para estilos modernos y responsivos.

#### **Almacenamiento Local**

- **Local Storage**:
    - Permite almacenar datos temporales del proyecto (como agentes o configuraciones) directamente en el navegador del usuario.

#### **Estructura Estándar del Proyecto**

- **Carpeta principal (`main`)**:
    - **`agents/`**:
        - Archivos `.md` (uno por agente).
        - `MANIFEST.json` para describir agentes y sus propiedades.
    - **`intel/`**:
        - Archivos `.json`, `.csv` y `.md`.
        - `MANIFEST.json` con configuraciones para interpretar cada archivo.
    - **`repo/`**:
        - Markdown, PDFs u otros documentos relevantes.
        - `MANIFEST.json` para definir el contenido.

---

### **Flujo de Usuario**

1. **Carga del Archivo**:
    
    - El usuario arrastra un archivo `.zip` o `.rar` al sistema.
    - DECIPHER lo procesa, valida la estructura y extrae su contenido.
2. **Visualización**:
    
    - El sistema genera una página interactiva con las secciones `Agents`, `Intel` y `Repo`, mostrando los archivos en su formato interpretado.
3. **Edición**:
    
    - El usuario agrega o modifica contenido directamente desde la interfaz (por ejemplo, agregar un nuevo agente).
4. **Exportación**:
    
    - El usuario exporta el proyecto actualizado como un archivo `.zip` o `.rar`.

---

### **Características Técnicas Clave**

1. **Despliegue Simple**:
    
    - **Sin Backend:** Todo el procesamiento ocurre en el navegador.
    - **Hosting Estático:** Compatible con **Vercel** o **GitHub Pages**.
2. **Compatibilidad de Formatos**:
    
    - Markdown (`.md`) → Renderizado de texto.
    - JSON → Generación de gráficos interactivos.
    - CSV → Transformación en tablas.
    - PDF → Enlaces descargables o vistas previas.
3. **Seguridad**:
    
    - Los datos nunca abandonan el navegador del usuario.
    - No se requiere conexión a internet después de cargar la página.
4. **Interfaz Modular y Escalable**:
    
    - Componentes reutilizables permiten añadir nuevas secciones o formatos fácilmente.
5. **Portabilidad**:
    
    - Los proyectos exportados pueden ser reutilizados o compartidos con facilidad gracias al estándar comprimido.

---

### **Tecnologías y Herramientas Utilizadas**

|**Componente**|**Tecnología**|
|---|---|
|**Framework**|Next.js|
|**Lenguaje**|JavaScript/React|
|**Librerías**|JSZip, react-markdown, Chart.js, Papaparse|
|**Hosting**|Vercel, GitHub Pages|
|**Estilos**|TailwindCSS (opcional)|

---

### **Posibilidades Futuras**

1. **Colaboración Multiusuario**:
    
    - Integrar almacenamiento en la nube (ej. Firebase) para permitir múltiples usuarios trabajando en el mismo proyecto.
2. **Automatización de Reportes**:
    
    - Generar reportes en PDF directamente desde los datos cargados.
3. **Extensión de Formatos**:
    
    - Soporte para otros formatos como imágenes o videos.
4. **Modo Offline**:
    
    - Garantizar que todo el sistema funcione sin conexión a internet.

---

### **Conclusión**

DECIPHER es un sistema web diseñado para simplificar la visualización y personalización de proyectos de consultoría. Su enfoque en un diseño estático y procesamientos locales garantiza bajos costos operativos, facilidad de despliegue y un rendimiento confiable. Ideal para consultorías modernas que necesitan un sistema dinámico y sin complicaciones.

¿Te gustaría una maqueta o prototipo inicial basado en esta descripción?