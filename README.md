
## Wazuh: Plataforma de Seguridad Open Source



## Introducción

Wazuh es una plataforma de seguridad open source que proporciona detección de intrusiones, monitorización de integridad, análisis de logs, detección de vulnerabilidades y respuesta ante incidentes.

Está basada en un modelo cliente-servidor donde los agentes Wazuh se instalan en los endpoints y envían información a un servidor central para su análisis


## Instalación
![](ANEXOS/Pasted%20image%2020250402193408.png)


Una vez ejecutado el comando ``curl -sO https://packages.wazuh.com/4.11/wazuh-install.sh && sudo bash ./wazuh-install.sh -a`` se nos iniciara la instalación de los diferentes componentes de wazuh



![](ANEXOS/Pasted%20image%2020250402224626.png)


### Ventana principal de gestión en wazuh

---

![](ANEXOS/Pasted%20image%2020250402222541.png)

### Ventana de adición de un nuevo agente

---

![](ANEXOS/Pasted%20image%2020250402225644.png)


### Una vez añadido el agente podemos ver el panel de control de cada uno

---

![](ANEXOS/Pasted%20image%2020250402230345.png)

### Nos dice las posibles vulnerabilidades que ha encontrado en el sistema

---

![](ANEXOS/Pasted%20image%2020250402233649.png)

### Tiene un escáner SCA que nos da recomendaciones de seguridad

---

![](ANEXOS/Pasted%20image%2020250402233513.png)

![](ANEXOS/Pasted%20image%2020250402233628.png)


###  **¿Por qué es necesario habilitar ``ufw?``**

El servicio **UFW (Uncomplicated Firewall)** debe estar **activo** y **habilitado** para proteger el sistema contra tráfico no autorizado. Si no está funcionando, **el sistema queda expuesto** a conexiones no filtradas.


 **Solución paso a paso**


 **Activar y habilitar UFW para que se ejecute en cada arranque**

```bash
systemctl --now enable ufw.service
```

 **Habilitar el firewall**

```bash
ufw enable
```

Con esto, te aseguras de que UFW está protegiendo el sistema de manera adecuada. 

---

![](ANEXOS/Pasted%20image%2020250402233128.png)
![](ANEXOS/Pasted%20image%2020250402233843.png)
![](ANEXOS/Pasted%20image%2020250402233832.png)

### **¿Por qué modificar `/tmp`?**

El directorio `/tmp` es un espacio de almacenamiento temporal de uso global, lo que significa que **todos los usuarios y aplicaciones pueden escribir en él**. Sin embargo, esta característica también lo convierte en un **objetivo atractivo para ataques**, especialmente los que explotan la ejecución de código malicioso o vulnerabilidades en programas del sistema.

Configurar `/tmp` como un sistema de archivos separado con opciones de seguridad ayuda a **mitigar varios riesgos de seguridad**, como:

---

## **Riesgos mitigados**

### **Ejecución de código malicioso (`noexec`)**

Si un atacante sube un archivo malicioso a `/tmp` y lo ejecuta, puede comprometer el sistema. La opción `noexec` **impide la ejecución de archivos binarios** en `/tmp`, bloqueando muchos ataques.

###  **Escalada de privilegios con archivos SUID (`nosuid`)**

Algunos programas tienen el bit **SUID (Set User ID)** activado, lo que les permite ejecutarse con permisos elevados. Un atacante podría **crear un enlace duro (hard link)** en `/tmp` apuntando a un programa SUID, y cuando este se actualiza, el enlace quedaría apuntando a una versión antigua y vulnerable.

- La opción `nosuid` **evita la ejecución de binarios SUID en `/tmp`**, eliminando este riesgo.

### **Creación de dispositivos maliciosos (`nodev`)**

Si un atacante consigue crear archivos de dispositivos (`/dev/...`) en `/tmp`, podría interactuar con hardware o procesos del sistema de forma peligrosa.

- La opción `nodev` **bloquea la creación de dispositivos especiales en `/tmp`**.

###  **Ataques de denegación de servicio (`size=2G`)**

Si `/tmp` no tiene una limitación de tamaño, un atacante podría llenar el disco con archivos temporales, causando un **Denial of Service (DoS)**.

- La opción `size=2G` **limita el espacio que `/tmp` puede consumir**, evitando que un usuario malintencionado bloquee el sistema.

![](ANEXOS/Pasted%20image%2020250402234718.png)

Una vez hecho vemos que se han arreglado varios fallos

---

![](ANEXOS/Pasted%20image%2020250402235608.png)

También podemos añadir dashboard personalizadas y siguiendo la guía de wazuh he creado varias

![](ANEXOS/Pasted%20image%2020250403000104.png)


![](ANEXOS/Pasted%20image%2020250403001019.png)

![](ANEXOS/Pasted%20image%2020250403001328.png)

![](ANEXOS/Pasted%20image%2020250403001719.png)

![](ANEXOS/Pasted%20image%2020250403002024.png)

![](ANEXOS/Pasted%20image%2020250403002233.png)

![](ANEXOS/Pasted%20image%2020250403002242.png)

Y puedes combinarlas creando personalizaciones dependiendo de las necesidades

## Monitorización de Integridad de Archivos (FIM)

Wazuh permite monitorear archivos y directorios críticos del sistema para detectar modificaciones, eliminaciones o creaciones inesperadas. Esto es útil para identificar posibles alteraciones maliciosas o cambios no autorizados.

### Configuración de FIM en el Agente

La configuración del **File Integrity Monitoring (FIM)** se realiza en el agente Wazuh. Para habilitar el monitoreo de archivos o directorios específicos, sigue estos pasos:


![](ANEXOS/Pasted%20image%2020250403135036.png)
![](ANEXOS/Pasted%20image%2020250403142124.png)
![](ANEXOS/Pasted%20image%2020250403143431.png)
![](ANEXOS/Pasted%20image%2020250403143506.png)
![](ANEXOS/Pasted%20image%2020250403143601.png)

![](ANEXOS/Pasted%20image%2020250403143753.png)
![](ANEXOS/Pasted%20image%2020250403143806.png)

Una vez hecho nos avisa si hay modificaciones en el archivo o en el directorio especificado
