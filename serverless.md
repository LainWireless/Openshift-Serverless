## **2. Red Hat OpenShift Serverless**

OpenShift Serverless es una plataforma que funciona como una capa de abstracción sobre la plataforma de contenedores de Red Hat OpenShift (RHOCP) para facilitar la implementación de aplicaciones de forma estandarizada. Al estar basada en el proyecto de código abierto Knative, proporciona una interfaz uniforme y estrechamente integrada en todo el ecosistema de nube híbrida, incluyendo Edge Computing.

La implementación y actualización de aplicaciones se simplifican gracias a OpenShift Serverless, que también ofrece funcionalidades como interconexión de aplicaciones, enrutamiento de tráfico y escalado automático. Además, se integra con otros productos de Red Hat OpenShift, como el servicio de monitorización de clúster y Service Mesh. 

También admite cientos de fuentes de eventos, incluidos mensajes de Kafka a través de Apache Camel K. El Operador oficial proporciona un proceso de instalación rápido y sencillo.

OpenShift Serverless tiene dos componentes principales, cada uno con sus propias responsabilidades distintas:

**Serving:** Componente encargado del despliegue y la escalabilidad automática de las aplicaciones.

**Eventing:** Infraestructura para consumir y producir eventos que impulsan las aplicaciones.

OpenShift Serverless se comunica principalmente a través de documentos YAML, los cuales permiten definir de manera declarativa el estado deseado de la aplicación. No obstante, también dispone de una alternativa de interacción imperativa, la cual puede realizarse a través de la herramienta de línea de comandos kn o mediante la consola web de Red Hat OpenShift.

### **2.1. Ventajas de Serverless**

**Provisión automatizada**

La principal ventaja del serverless es que los servidores son completamente proporcionados por el proveedor serverless, quien se encarga de la provisión, mantenimiento y escalado de los mismos.

También puede llevar a minimizar el uso de recursos, y esto viene con una reducción de costos: ser cobrado solo por el uso de recursos significa que si no se usan recursos, entonces no se cobra.

**Enfoque en el desarrollo**

Eliminar la necesidad de provisión y mantenimiento del servidor permite a los desarrolladores concentrarse en la lógica de negocios y evitar cualquier fricción durante el despliegue porque el ambiente de desarrollo y producción son iguales.

**Escalar a cero**

Las infraestructuras serverless se escalan a cero instancias de aplicación cuando no hay tráfico y reaccionan a las solicitudes entrantes poniendo la solicitud en espera mientras se activan las aplicaciones necesarias.

Esto tiene el efecto de que algunas solicitudes pueden experimentar retrasos debido al inicio de los recursos: si la latencia es crucial para la carga de trabajo, entonces los desarrolladores pueden desactivar este comportamiento estableciendo un número mínimo de instancias activas en la aplicación requerida.

**Eliminar la planificación de capacidad**

Con la infraestructura tradicional, la capacidad de cómputo se reserva y paga por adelantado, mientras que las aplicaciones serverless solo se ejecutan cuando se necesitan. Esto reduce el trabajo de planificación de capacidad para las aplicaciones desplegadas.

### **2.2. Cuándo utilizar Serverless**

**Tareas periódicas:** tareas pequeñas que necesitan ejecutarse en momentos predefinidos para tareas de mantenimiento.

**Procesamiento de IoT:** recibir y procesar datos y eventos de dispositivos inteligentes.

**Reaccionar a eventos externos:** actuar en los cambios de aplicaciones externas.

**Back-end de aplicaciones web:** lado del servidor de aplicaciones web que pueden dividirse en microservicios.

**Procesamiento por lotes:** trabajos de procesamiento en segundo plano.

### **2.3. Cuándo NO utilizar Serverless**

No todas las aplicaciones son adecuadas para la arquitectura Serverless, algunos casos de uso lo hacen muy difícil de utilizar.

Algunos ejemplos de esto son:

**Baja latencia:** los requisitos de latencia extremadamente bajos son difíciles de lograr debido a su naturaleza dinámica.

**Pruebas y depuración fáciles:** las aplicaciones Serverless se dividen en muchos fragmentos pequeños, por lo que son más difíciles de probar y depurar como un todo.

**Observabilidad:** al igual que con las pruebas y depuración, una aplicación dividida en muchas partes es más difícil de rastrear y monitorear.

### **2.4. Instalación del Operador OpenShift Serverless**

La instalación del Operador OpenShift Serverless nos permite instalar y usar Knative Serving, Knative Eventing y Knative Kafka en un clúster de plataforma de contenedores OpenShift. El Operador OpenShift Serverless administra las definiciones de recursos personalizados (CRDs) de Knative para nuestro clúster y nos permite configurarlos sin modificar directamente los mapas de configuración individuales para cada componente.

**Desde la Consola Web:**

**Prerrequisitos**

- Tener acceso a una cuenta de OpenShift Container Platform con acceso de administrador de clúster.

- Haber iniciado sesión en la consola web de la plataforma de contenedores OpenShift.
  
**Instalación**

1. En la consola web de la plataforma de contenedores OpenShift, vamos a la página Operadores → OperatorHub.

<div align="center">
<img src="serverless1.png" alt="serverless1" height="300px" width="350px">
</div>

2. Nos desplazamos, o escribimos la palabra clave "Serverless" en el cuadro Filtrar por palabra clave para encontrar el Operador OpenShift Serverless.

3. Haremos clic en instalar, aunque antes podremos revisar la información se proporciona sobre el Operador.

4. A la hora de instalar el Operador, podremos seleccionar diversas opciones según nuestras necesidades.

**Desde la CLI:**

**Prerrequisitos**

- Tener acceso a una cuenta de OpenShift Container Platform con acceso de administrador de clúster.

- Que el clúster tenga habilitada la capacidad del Marketplace o la fuente de catálogo del operador de Red Hat configurada manualmente.

- Haber iniciado sesión en el clúster de OpenShift Container Platform.

**Instalación**

1. Creamos un archivo YAML que contenga objetos de Namespace, OperatorGroup y Subscription para suscribir un espacio de nombres al Operador OpenShift Serverless. Por ejemplo, creamos el archivo serverless-subscription.yaml con el siguiente contenido:
```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: openshift-serverless
---
apiVersion: operators.coreos.com/v1
kind: OperatorGroup
metadata:
  name: serverless-operators
  namespace: openshift-serverless
spec: {}
---
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: serverless-operator
  namespace: openshift-serverless
spec:
  channel: stable
  name: serverless-operator
  source: redhat-operators
  sourceNamespace: openshift-marketplace
```

2. Creamos el objeto de suscripción:
```bash
oc apply -f serverless-subscription.yaml
```
