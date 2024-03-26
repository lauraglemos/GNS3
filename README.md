Durante esta primera tarea debéis crear una pequeña red de un operador que dé servicio a, al menos, tres clientes (dos de ellos compartiendo la misma red de acceso/bucle de abonado). Estos (empresas o usuarios domésticos), dispondrán de una mínima intranet (red privada interna con una o dos subredes) con los servicios DHCP y NAT activos para configurar dinámicamente los parámetros de red de los equipos locales y posibilitar su acceso a Internet, respectivamente. El router de acceso a Internet propio de cada cliente (CPE) es el lugar idóneo para implementar tales servicios.

Suponga también que algunos clientes ofrecerán algún servicio (por ejemplo un portal web publicitando un producto) desde dentro de su intranet, que deberá poder ser accedido desde el exterior (en general, desde Internet).

La red del operador debe tener redundancia suficiente para impedir que la caída de un solo enlace y/o equipo deje sin servicio a cualquier cliente (Obviad, por ahora, el posible fallo en el router de acceso del cliente (CPE) o del ISP (PE), o del enlace que los une). Y, adelantándonos a su seguro crecimiento en sucesivas evoluciones del proyecto, descarte ya configurar la tabla de encaminamiento de sus nodos utilizando rutas estáticas.

Para dar su servicio, el operador ha adquirido el rango 202.101.16.0/23.

La que originalmente era solo una pequeña explotación ganadera se ha hecho un hueco importante en el sector lácteo. Lo que empezó siendo un negocio familiar alcanza ya dimensiones considerables, como muestran los cuatro centros de distribución, con sus respectivos almacenes y oficinas, que la empresa —“A Rubia Galega S.L.”, aka “ARG”— tiene desplegados por la geografía nacional.

Para la comunicación entre las diferentes sedes corporativas, y su conectividad global, la empresa contrata cierto operador —en adelante, ISP1— que, no por sorpresa, posee infraestructura de comunicaciones (bucles de abonado) en los cuatro emplazamientos donde están las sedes del futurible gigante lácteo:

Para la conectividad de sus sedes se ha elegido una topología Hub and Spoke, teniendo la sede principal —el Hub— unas características singulares:
Grosso modo, el Hub dispone internamente de tres subredes diferentes para dar cobijo a su Data Center, servicios online e intranet corporativa, respectivamente. Además, el cliente desea conservar su propio prefijo de red (199.242.32.0/23), que utilizará en la VLAN en la que ofrece sus servicios al exterior y, en general, para la navegación por Internet, utilizando direccionamiento privado para sus otras subredes. El cliente acarrea, también, su anterior identificador de sistema autónomo (AS-64501).
En cada una de las demás sucursales —Spokes— solo se implementa un subconjunto de la intranet corporativa (una única VLAN por sede, con direccionamiento privado).
Como no podía ser de otra forma en los tiempos que corren, en el modelo de negocio de “ARG” tiene un peso muy importante la venta online, y los recursos que se han dispuesto para ello (servidores, bases de datos…) residen exclusivamente en la sede principal (el hub), por lo que la disponibilidad en este punto es crítica:
Para evitar quedarse sin conectividad por algún fallo en la infraestructura de comunicaciones, en la sede principal han duplicado los equipos de red (switches/routers/firewalls) proveyendo redundancia que soporte al menos la caída de uno de dichos dispositivos (véase figura adjunta, y modelo simplificado para ATR).
En la misma línea, contratan con un segundo proveedor (ISP2) una conexión a Internet alternativa, de tal forma que este actuará de backup en caso de que el ISP principal (ISP1) deje de dar servicio. Este ISP2 podría ser diferentes en hub y sucursales. Para unir las sucursales con la sede, los ingenieros telemáticos de la empresa implementan per se (ahorro de costes) un servicio de VPN corporativo, usando el acceso a Internet adicional contratado a operadores secundarios.
Pero el mundo no gira (obviamente) alrededor de “A Rubia Galega S.L.”:

Ante la escasez de direcciones IPv4 públicas propias, y la imperiosa necesidad de captar muchos abonados (domésticos y corporativos), nuestro operador (ISP1) ha decidido ofrecer un servicio de acceso a Internet a través de CG-NAT para clientes no premium, realizando la traducción del rango privado de CG-NAT a un pool particular (al menos dos IP) antes de salir a Internet.
Paralelamente, deseamos que el servicio al cliente sea inmediato (“plug&play”), de tal forma que en el mismo momento en que conecte el equipo recibido (CPE) a la toma de red de su emplazamiento, la conectividad debe quedar establecida sin la intervención de operario alguno. La configuración dinámica que ello implica debe estar, además, lo más centralizada posible, evitando configurar un servidor DHCP en cada equipo frontera (PE) de la red del operador.
Cada uno de los puntos de presencia (POP) deberá consumir, a lo sumo, 3 direcciones IPv4 públicas —sin contar, obviamente, las entregadas a los propios clientes mediante DHCP.
Para la salida a Internet (tráfico IPv4), operador principal (ISP1) y secundario (IPS2) utilizan como operador de tránsito a ISP-T, al que alcanzan directamente a través de alguno de sus respectivos nodos frontera.

Varios son los motivos que llevan a los operadores a utilizar MPLS en su red de transporte, como, por ejemplo, una conmutación de paquetes más eficiente, ingeniería de tráfico o seguridad. Active dicha funcionalidad en el IPS1, consiguiendo que no aparezcan en las tablas de encaminamiento de los nodos de su core las redes donde están situados los clientes/CPEs (redes de los bucles de abonado), ni las de AS externos.

Utilice iBGP para el establecimiento de los LSPs (entre los PEs y los BR), y hágalo de forma escalable imaginando un escenarios con decenas de bucles de abonados (usando uno o varios reflectores).
El advenimiento de las tecnologías IoT (Internet of Things) —fundamentales para el desarrollo de hogares, edificios, ciudades e infraestructuras inteligentes, así como para la industria 4.0— ha puesto las pilas a nuestro operador principal (ISP1) que ya ofrece conectividad IPv6 a cualquiera de sus clientes usando la tecnología 6RD.

El ISP administra el rango 2002:cafe::/38, usando un prefijo mayor (/40, por ejemplo) para el servicio 6RD propiamente dicho.
El servicio IPv6 a implementar debe permitir que terminales internos (tras los CPEs) adquieran una IPv6 global de forma dinámica (SLAAC), para que puedan comunicarse unos con otros independientemente del emplazamiento en que se encuentren (ello incluye a recursos/equipos IPv6 de Internet).
Los clientes 6Rd —CPEs— recibirán un prefijo /60 que subdividirán internamente según sus necesidades.
Como ocurre con parte de su direccionamiento IPv4, ARG S.L. acarrea un prefijo global IPv6 que desea conservar, y utilizar en las subredes de su HUB. Administre dicho rango (2001:2001:2001:2000::/56) desde el propio FW interno, y utilice alguna de las múltiples opciones posibles (6PE, túnel SIT, otras) para la conexión del HUB al mundo IPv6.
El ISP-T tiene una red de transporte IPv4, implementa MPLS, y utiliza 6PE para el tráfico IPv6 que debe atravesarlo.
Al ISP primario (ISP1) también se le contrata un servicio de VPN corporativo (Trusted VPN), que posibilite la visibilidad de las subredes internas del Hub (intranet y data center) en cada una de las sucursales conectadas al mismo ISP.

Nuestro operador mantiene un bucle de abonados virtual cuyos clientes gestiona/configura —de forma centralizada— usando el protocolo PPP. Además, también actúa como proveedor de acceso para otros ISPs que físicamente no disponen de tendido propio en el bucle de abonado para llegar a esos clientes a los que sí llega el nuestro.

Implemente las funcionalidades necesarias para ofrecer ambos servicios:

1. el mayorista (para otros ISPs),
2. y el bucle virtual propio.
Considere únicamente dos clientes (CPEs), uno de cada tipo, que puede situar en cualquiera de los tres bucles de abonado implementados.
