Implementación y Resolución de Nombres de Dominio en la Nube mediante Azure Private DNS Zones

Este proyecto demuestra el diseño, despliegue y validación de un sistema de resolución de nombres global utilizando el servicio de Azure DNS. La arquitectura simula un entorno de producción público donde se configura una zona DNS autoritativa para el dominio corporativo wideworldimports.com, permitiendo asociar nombres de host lógicos a direcciones IP expuestas a internet. Para verificar la resiliencia y el comportamiento operativo de la infraestructura sin depender de un registrador externo, se implementaron conjuntos de registros de tipo A, se auditaron las respuestas de autoridad (SOA/NS) y se ejecutaron consultas directas y dirigidas mediante herramientas de diagnóstico perimetral, garantizando una topología de identidad en la nube de alta disponibilidad.

* Azure Resource Groups
* Azure Private DNS Zones
* Azure Virtual Networks 
* Virtual Network Links
* DNS Record Sets (Type A)

Guía de Despliegue y Configuración Técnica
Creación e inicialización del Grupo de Recursos principal denominado LAB-DNS ubicado en la región geográfica East US

<img width="688" height="362" alt="image" src="https://github.com/user-attachments/assets/cfd6a308-18fb-44bc-98e4-b7bd6b287779" />

Aprovisionamiento y Enlaces de Red (DNS Zones & VNet Links)
Búsqueda y selección del recurso centralizado Private DNS zones dentro del catálogo global de servicios del portal de Azure para iniciar la configuración del dominio privado.
<img width="480" height="144" alt="image" src="https://github.com/user-attachments/assets/d11d2cbb-1e15-418c-a1c4-ac8802df0ee0" />

Configuración de los parámetros básicos del servicio asignando la zona al grupo LAB-DNS. En el campo crítico de Name, se establece el nombre del dominio único: wideworldimportsXXXX.com. Se procede con la validación y despliegue del recurso.
<img width="753" height="673" alt="image" src="https://github.com/user-attachments/assets/3ddcc04d-2d1c-4f82-9627-398fb3a5ab7f" />
*NOTA*: A diferencia de las zonas privadas, Azure Public DNS expone estos registros a internet. Para que el dominio resuelva globalmente en un entorno real, debes copiar los 4 servidores NS que te entrega Azure y pegarlos en tu proveedor de dominio (como GoDaddy).

Una vez creada guardamos los registros NS
<img width="1426" height="459" alt="image" src="https://github.com/user-attachments/assets/b140c585-9c1f-443b-9731-ef76ca7e26ff" />

Acceso al panel principal de la zona creada. Para poblar la base de datos de resolución, se hace clic en el botón +Agregar de la barra de herramientas superior.
<img width="1717" height="746" alt="image" src="https://github.com/user-attachments/assets/c5432e1a-f33b-4b89-a10d-6d13d7c26673" />

En el formulario lateral, se añade el subdominio www en el campo Name. Se selecciona el tipo en A - Address record, se define el TTL en 1 con la unidad en Horas, y se digita la dirección IPv4 de destino (10.10.10.10). Se hace clic en Add.
<img width="1716" height="872" alt="image" src="https://github.com/user-attachments/assets/55dea96d-af88-4cb2-a29f-fc4febd1e6e2" />
*NOTA*: El registro "A" asocia un nombre de texto amigable a una IP. Si tu servidor web tuviera múltiples direcciones IP por alta disponibilidad, puedes agregarlas todas dentro de este mismo registro para balancear el tráfico.

Auditoría y Validación Operativa
Desde la consola de comandos de Azure Cloud Shell, se ejecuta una consulta dirigida utilizando la herramienta nslookup, especificando el FQDN completo y apuntando a uno de los servidores NS asignados por Azure para saltarse la propagación global. El sistema responde devolviendo de forma exacta la IP 10.10.10.10.
*nslookup www.wideworldimportsXXXX.com ns1-04.azure-dns.com* (xxxx equivale a los numeros anteriromente colocados)
<img width="1665" height="861" alt="image" src="https://github.com/user-attachments/assets/d41da4d9-58fc-409f-8bfa-9f7a05c0c80e" />




