## **7. Conclusiones**

Como hemos visto, Openshift Serverless es una plataforma que nos permite desplegar aplicaciones sin preocuparnos por la infraestructura. Además, nos permite escalar automáticamente nuestras aplicaciones en función de la demanda de los usuarios, y nos permite desplegar nuestras aplicaciones en cualquier nube pública o privada. A mi parecer es una plataforma muy potente y muy fácil de usar, y que nos permite desplegar aplicaciones de una manera muy sencilla y rápida. 

Tal vez para un administrador de sistemas no sea tan útil, ya que no tiene que preocuparse por la infraestructura; pero para un desarrollador sí que lo es, ya que le permite desplegar sus aplicaciones de una manera muy sencilla y rápida, no necesitando tener conocimientos de infraestructura o de administración de sistemas. Por no hablar de lo que a mi parecer es la principal ventaja, no pagar por recursos que no se están usando.

En cuanto a Knative Serving, es la herramienta que nos ha permitido desplegar nuestra aplicación, sin embargo, no es la única herramienta que nos ofrece Knative. Knative también nos ofrece Knative Eventing, que nos permite crear eventos y reaccionar a ellos.

Aunque este último se ha quedado fuera del proyecto por motivos obvios, ya que el proyecto entonces hubiera sido demasiado largo y complejo, me parece importante recalcar que con el uso de esto hubieramos podido incluso convertir el backend en serverless o por ejemplo crear un sistema de notificaciones que nos avisara cuando la temperatura de una ciudad superarase un cierto umbral.

También se ha dejado fuera del proyecto el uso de las Serverless Functions, que nos facilita plantillas para crear funciones serverless en distintos lenguajes de programación. Estas plantillas proporcionan al desarrollador los archivos y dependencias necesarios, y un código inicial vacío para una función. Finalmente, Serverless Functions acabaría construyendo una imagen de ejecución y tras esto instantáneamente OpenShift Serverless se encargaría de subir, desplegar y gestionar las funciones como cualquier otra aplicación.

Esto último a mi parecer sería útil principalmente para un desarrollador, no le veo el sentido a un administrador usando Serverless Functions. 

Con todo esto que he explicado, lo que quiero dar a entender la gran cantidad de funcionalidades y opciones que nos proporciona Openshift Serverless y Knative, y que algunas de ellas muy importantes (como Knative Eventing) no se han podido ver en este proyecto.
