# Infra-on-off
*Proceso para iniciar y detener la infraestructura de AFP y NWS, donde esta configurado como Schedule y tambien puede ser gatillado de manera on-demand*

## Como funciona
*El proceso interactua con el SDK de GCP y ocupa los comandos para detener VM y GKE*

**Ejemplo:**
```bash
# Iniciar la lista de VM seteadas
bash stop-start-vm.sh start_vm "$VM_NAME_AFP" "$PROJECT_NAME_AFP"

# Detener la lista de las VM seteadas
bash stop-start-vm.sh stop_vm "$VM_NAME_AFP" "$PROJECT_NAME_AFP"

# Escalado GKE a 3 nodos
bash scale-gke.sh "gke-cluster-omni-afp-dev" "pool-2" "3" "omni-afp-minisitio-dev" "$ZONE"

# Escalado GKE a 0 nodos
bash scale-gke.sh "gke-cluster-omni-afp-dev" "pool-2" "0" "omni-afp-minisitio-dev" "$ZONE"
```
## Variables
*A continuacion se describen las variables necesarias para el correcto funcionamiento del pipeline.*

| Variable               | Tipo    | Descripción                |
|------------------------|---------|----------------------------|
| PROJECT_NAME_AFP       | var     | Lista de proyectos de AFP para recorrer donde se podrá iniciar/detener las VM   |
| VM_NAME_AFP            | var     | Lista de VM para recorrer donde se podrá iniciar/detener   |
| SA_INFRA_ON_OFF        | var     | Service Account con Permisos para interactuar con GKE y GCE |
| ZONE                   | var     | Zona por defecto donde están creados los recursos |
| WEBHOOK                | var     | Valor: URL del Webhook  |
| START_INFRA_AFP        | var     | Variable de control para configurar en el schedule, valor `true/false` |
| START_INFRA_NWS        | var     | Variable de control para configurar en el schedule, valor `true/false` |
| STOP_INFRA_AFP         | var     | Variable de control para configurar en el schedule, valor `true/false` |
| STOP_INFRA_NWS         | var     | Variable de control para configurar en el schedule, valor `true/false` |

## Schedules y On-Demand
*Se configuro dos formas para ser gatillado el pipeline de acuerdo a la necesidad*
#### Schedules
*Se tiene configurado los Schedules correspondientes:*
![image.png](./img/schedules-1.png)

*Que tienen una variable de control para poder ser identificado en el Job correspondiente*
![image.png](./img/schedules-2.png)

*Estos tienen el horarrio:*
- Apagado a las **20:00** de lunes a viernes hora Chile *(UTC - 3)*
- Inicio a las **06:00** de lunes a viernes hora Chile *(UTC - 3)*

#### On-Demand
*Se puede gatillar mediante el pipeline ocupando la rama **main** y donde ejecutar de forma manual los jobs y ambientes correspondientes:*
![image.png](./img/on-demand-1.png)
![image.png](./img/on-demand-2.png)

#### ToDo
- gatillar a demanda `Done`
- webhook   `Done`
