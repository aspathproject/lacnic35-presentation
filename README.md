---
marp: true
theme: default
html: true
class:
footer: ASPATH - Software c贸digo abierto para monitoreo de colectores de rutas https://aspath.app
---
<style>
.blue {
  color: blue;
}
.red {
  color: red;
}
</style>

# ASPATH Project

Software open-source para monitoreo de colectores de rutas BGP.

##### Presenta:
###### Rodrigo Pe帽a <br>Software Engineer
![bg right 50%](img/vertical_logo.svg)

---

# Problemas comunes
- "Cierta p谩gina no carga/funciona 馃" 
- "A que proveedor corresponde una direcci贸n IP?"

- B煤squeda de causa
  - DNS ok? `dig lacnic.net`
  - Ping destino? `ping -f -c 100 lacnic.net`
  - **Problemas de ruteo**? bgp.he.net / BGPlay / `show ip bgp` / `traceroute` / ...

---

# Mikrotik way

![bg right 100%](img/mikrotikbgp.gif)
La tentaci贸n existe, pero est谩s poniendo mucho en juego por querer visualizar la tabla de rutas

---

# Al momento de diagnosticar el problema ya desapareci贸

-<span class="blue">**Azul**</span>: Ya se arregl贸, sigo con mi vida
-<span class="red">**Roja**</span>: Quiero saber la verdad
![bg right 100%](img/matrixpill.gif)

---

# Por qu茅 es tan valioso investigar

- Posible Hijack BGP
- Definir responsabilidades
- **Robustecer estabilidad de la red**

---

![bg right 100%](img/morpheus.jpg)
- Posibilidad de explorar la tabla de ruteo a lo largo del tiempo
- Desde la comodidad del navegador web
- Implementable con cualquier router o servidor de rutas

---

![bg right 50%](img/vertical_logo.svg)
- Explorador de snapshots de tabla de ruteo con tecnolog铆as web.
- Permite **almacenar y visualizar** tablas de ruteo de m煤ltiples equipos a lo largo del tiempo.
- Desplegable de manera privada
- C贸digo abierto bajo licencia MIT

---

<div style="display: block;"><h1 style="margin: 0 auto;">C贸mo funciona</h1></div>
<div style="display: flex;">
<div style="width: 70%; display: inline-block;">
<img src="img/collector_view.gif" style="width: 100%" />
</div>
<div style="display: inline-block;width: 30%;">
<ul>
  <li>
     Vista de snapshot proveniente de route collector de PCH.
  </li>
  <li>
     Filtros din谩micos para buscar dentro de tabla de ruteo.
  </li>
  <li>
     Lista de prefijos incluyendo otros datos como nombre de sistema aut贸nomo.
  </li>
</ul>
</div>
</div>

---

<div style="display: block;"><h1 style="margin: 0 auto;">Caso pr谩ctico: buscando anuncios extra帽os en intercambio de tr谩fico</h1></div>
<div style="display: flex;">
<div style="width: 70%; display: inline-block;">
<img src="img/prefix_search.gif" style="width: 100%" />
</div>
<div style="display: inline-block;width: 30%;">
<ul>
  <li>
     B煤squeda r谩pida sobre bloques con incidentes t铆picos.
  </li>
  <li>
     Se logra encontrar hijack presente por largo tiempo.
  </li>
</ul>
</div>
</div>

---

<div style="display: block;">
  <h1 style="margin: 0 auto;">Caso pr谩ctico: encontrando BGP Leaks en servidor de ruta</h1>
</div>
<div style="display: flex;">
<div style="width: 70%; display: inline-block;">
<img src="img/bgp_leak.gif" style="width: 100%" />
</div>
<div style="display: inline-block;width: 30%;">
<ol>
  <li>
     B煤squeda de rutas provenientes de microsoft.
  </li>
  <li>
     Tras explorar tabla, se encuentran rutas con AS_PATH sospechosos.
  </li>
  <li>
     Se detecta BGP leak proveniente de inyecci贸n de rutas de tr谩nsito hacia IXP.
  </li>
</ol>
</div>
</div>

---

# C贸mo se implementa software ASPATH

- Actualmente, se consideran 2 m茅todos para agregar datos de ruteo al software:
  - **Quagga dump grabber**: 脷til para trabajar con routing snapshots de prove铆do por terceros. **PCH provee routing snapshots diarios de sus colectores de rutas en este formato**.
  - **Colector GoBGP**: Implementaci贸n moderna de BGP. Se puede desplegar en una m谩quina virtual y hacer sesi贸n multihop contra router de borde. <b>Implementaci贸n compatible con cualquier router que implemente BGP</b>.

---

<div style="display: block;">
  <h1 style="margin: 0 auto;">Diagrama de funcionamiento</h1>
</div>
<div style="display: flex;">
<div style="width: 70%; display: inline-block;">
<img src="img/aspath_diagram.png" style="width: 100%" />
</div>
<div style="display: inline-block;width: 30%;">
<ul>
  <li>
     Proceso para alimentar al software con los Routing Snapshots disponibles desde Packet Clearing House.
  </li>
  <li>
     Este despliegue no requiere interacci贸n con ning煤n router.
  </li>
</ul>
</div>
</div>

---

<div style="display: block;">
  <h1 style="margin: 0 auto;">Diagrama de funcionamiento para ISP</h1>
  <h2>Backbone de ejemplo</h2>
</div>
<div style="display: flex;">
<div style="width: 30%; display: inline-block;">
<img src="img/isp_deployment.png" style="width: 100%" />
</div>
<div style="display: inline-block;width: 70%;">
<ul>
  <li>
   Se instala <a href="https://osrg.github.io/gobgp/">goBGP</a> como colector de rutas.
  </li>
  <li>
   goBGP mantendr谩 sesiones multihop contra routers de borde
  </li>
  <li>
     ASPATH extraer谩 peri贸dicamente la tabla de ruteo de goBGP para alimentar la base de datos.
  </li>
</ul>
</div>
</div>

---

# Roadmap
## Q2 2021
<ul>
  <li><b>Lanzamiento versi贸n Beta</b></li>
  <li>Explorador de rutas  <small style="font-size: 11pt;"><strong>en progreso</strong></small></li>
  <li>Ver snapshots pasados  <small style="font-size: 11pt;"><strong>en progreso</strong></small></li>
  <li>Grabber Quagga y goBGP</li>
</ul>

## Q3 2021
<ul>
  <li>Lanzamiento versi贸n estable</li>
  <li>Capacidad de compartir snapshots entre organizaciones</li>
</ul>

---

# Te necesitamos
- Operadores de red que deseen implementar software
- Desarrolladores que quieran ser parte del proyecto
  - Python, Javascript, Ruby

- Interesados: https://aspath.app
- Otros: Suscribirse al newsletter para recibir noticias del proyecto

---

# Proyecto bajo licencia MIT 驴馃?
- C贸digo abierto y disponible en https://aspath.app.
- Sin fees ni royalties.
- Se permite:
  - Uso comercial del software
  - Libre distribuci贸n y modificaci贸n de este.
  - Uso privado
---

# 驴Preguntas? 馃

Software open-source para monitoreo de colectores de rutas BGP.

##### Presenta:
###### Rodrigo Pe帽a <br>Software Engineer
![bg right 50%](img/vertical_logo.svg)
