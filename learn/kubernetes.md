# Kubernetes
Kubernetes es una herramienta de orquestración de contenedores. Nace por el auge del uso de aplicaciones que se componen de microservicios. Para gestionarlos, se utilizaban scripts; se hacía muy complicado de mantener.

## Qué ofrece

- High Availability: No downtime
- Scalability: Scale up and down based on load
- Disaster recovery: Backup and restore

## Kubernetes Architecture

1. At least one master node. This contains the neccesary applications to run the orchestration:
  - API Server: Entrypoint to K8s cluster. UI, API and CLI talks to it.
  - Controller Manager: Keeps track of whats happening in the cluster.
  - Scheduler: Ensure Pods placement.
  - etcd: Kubernetes backing store. Stores all the configuration and state of each node and container.
2. Conected to it a couple a working nodes. In each of them there is a kubelet process running. It makes the node able to talk to each other and run processed. Each node can have multiple containers running on each. They are the ones running the actual application
3. Virtual Network: Creates one unified machine

Worker Nodes are ussually much bigger and have more resources to run the workload, sin embargo, el master node es esencial para el funcionamiento del cluster, sin él no podemos controlarlo. Por lo tanto, es usual disponer de una copia del master por si crashea, de forma que podamos recuperar el control del cluster.

## Main Kubernetes Components

### Node and Pod

Un pod es la unidad mínima en un cluster de Kubernetes. Es una abstracción de un contenedor, de forma que tratamos únicamente con esta interfaz y no con cómo es gestionado el contenedor por debajo (e.g. Docker). Normalmente se ejecuta una aplicación por Pod.

 Cada Pod tendrá una IP dentro de la red virtual del Cluster. Si un pod se muere, se creará una nuevo y este obtendrá una nueva IP. Si queremos comunicarnos entre Pods, esto parece un drawback, ya que no sabremos la IP que tendrá el servicio en un pod, ya que estos son considerados efímeros.

### Service and Ingress

Un servicio permite establecer una IP estática a un Pod. El ciclo de vida de un Service y de un Pod no están conectados. Es decir, cuando un Pod muere y levantamos uno nuevo para sustituirlo, como tenemos asociado a él un Servicio, este tomará la misma IP que el anterior. El tipo de servicio puede ser Interno y Externo.

Para exponer el servicio al exterior de forma adecuada, se utiliza el componente Ingress, que permite hacer forwarding the un domain name al servicio expuesto por un servicio externo en un Pod.

Es también un load balancer.

### ConfigMap & Secret

ConfigMap parace que es un gestor de variables de entornos. Secrets es lo mismo, pero más seguro, para contraseñas, etc.


### Volume

Permite utilizar volúmenes tanto locales como remotos y mapearlos dentro del Pod.

### Deployment & StatefulSet

Cuando deployeamos una nueva versión de la aplicación, para evitar downtimes, Kubernetes replica la configuración en otro nodo y la levanta con la nueva versión. Cuando el nuevo despliegue se ha completado satisfactoriamente, el Servicio (que es un DNS), apuntará ahora a la nueva versión de la aplicación desplegada. Si algo falla en el despliegue, todo seguirá funcionando como antes porque no se ha eliminado la versión que corría anteriormente.

Para evitar downtimes, lo que ocurre realmente es que no tendremos un único pod corriendo en un servicio, sino que lo replicaremos en varios nodos para garantizar la disponibilidad del servicio. Dependiendo de si nuestra aplicación es state**less** o state**ful**, utilizaremos un **Deployment** o un **StatefulSet**. En este blueprint, además de otras cosas, se definen cuántas réplicas queremos tener, cómo queremos que las réplicas escalen en función de la carga, etc. Es una abstracción por encima de los Pods para no tener que gestionarlos manualmente, sino de forma declarativa a través de este blueprint.

Ocurre un problema para el tipo de aplicaciones las cuales necesitan de consistencia en los datos, que puede verse afectadas por varias réplicas, como es el caso de las bases de datos. Para estos casos existe el componente **StatefulSet** (sts) y tiene que ser usado en vez de un **Deployment**, ya que es necesario que exista un intermediario que tenga en cuenta qué Pod está escribiendo a la base de datos, para evitar incosistencias, por ejemplo.

Usar StatefulSet is normalmente bastante complicado, por lo que muchas veces las DB se deployean fuera del cluster de Kubernetes y las aplicaciones stateless dentro.

### Kubernetes Configuration

1. Metedata
2. Specification
3. Status: Cuando status no es igual a specification, el manager intentará hacer lo posible para que ocurra, por ejemplo levantando un nuevo pod.

K8s obtiene el status data de etcd.


### Minikube & kubectl

Normalmente en un entorno de producción, tendremos un cluster con múltiples master nodes y múltiples worker nodes, cada uno en varias máquinas físicas o virtuales. Sin embargo, cuando queremos probar algo, queremos poder probarlo en nuestra máquina. Por eso existe una herramineta opensource llamada MiniKube, donde un node cluster contiene los procesos del master y worker en un único nodo, y que tiene docker preinstalado. Para controlar Minikube se utiliza kubectl como CLI. Kubectl no solo te permite controlar un cluster de Minikube, sino también uno en la nube.


## Info

(Kubernetes Crash Course for Absolute Beginners [NEW])[https://www.youtube.com/watch?v=s_o8dwzRlu4]
