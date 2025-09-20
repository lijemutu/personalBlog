---
title: "¿Vale la pena tomar la caseta?"
meta_title: "Reflexión y mirada técnica al problema de saber si vale la pena el dinero gastado en un tramo de cobro vs tramo libre"
description: "eflexión y mirada técnica al problema de saber si vale la pena el dinero gastado en un tramo de cobro vs tramo libre"
date: 2025-09-17T12:00:00Z
image: "/images/notificaciones-carretera/carretera-mexico-toluca-maps.png"
categories: ["Projects", "Engineering"]
author: "Erick López"
tags: ["traffic","python","selenium", "paying","highway", "carretera"]
draft: false
---
# ¿Vale la pena tomar la caseta?

![carretera mexico toluca](/images/notificaciones-carretera/carretera-mexico-toluca-maps.png)

Paso todos los días por la carretera México–Toluca para ir al trabajo y, a menudo, me pregunto si siempre vale la pena pagar la caseta. 

Cuando voy con prisa suelo elegir la autopista de cuota, pero no soy el único: muchas veces la fila en la caseta es larga y, al ver la carretera libre despejada, pienso que no valió la pena pagar.

## Primera solución (manual)

Cuando voy acompañado pido a mi esposa que revise el estado del tráfico en Google Maps y me diga si conviene tomar la cuota o la libre. Es práctico porque ambos conocemos la ruta y podemos decidir antes de llegar al punto de bifurcación.

Sin embargo, esta solución falla cuando viajo solo: es peligroso manipular el teléfono o Android Auto mientras conduzco.

## Solución técnica propuesta

Al no contar siempre con un copiloto, busqué una forma automatizada de recibir esa información. Android Auto puede reproducir notificaciones de mensajería mediante TTS (Text To Speech), así que la idea fue recibir un mensaje con la recomendación y que se reprodujera en el auto.

Para obtener esas notificaciones programadas opté por un prototipo que utiliza Selenium para controlar WhatsApp Web desde una sesión secundaria.

Con una SIM y una cuenta adicional puedo iniciar sesión en un navegador que Selenium controle y enviar mensajes a mi número personal con la recomendación ("Toma la libre" / "Toma la cuota").

### Definir rutas y puntos de decisión

Mi trayecto es bastante regular: trabajo fijo de lunes a viernes y paso por los mismos puntos de decisión. El primer paso fue identificar en Google Maps los puntos donde se separan las rutas y donde convergen nuevamente, de modo que pudiera medir el tiempo de recorrido por cada alternativa.

![punto de desviación en la carretera con la posibilidad de eligir cuota o libre](/images/notificaciones-carretera/desviacion-carretera.png)

Google Maps codifica coordenadas en la URL, lo que facilita definir los segmentos que quiero medir y comparar:
[https://www.google.com/maps/dir/19.3005883,-99.3786061/19.3903276,-99.2390682/...](https://www.google.com/maps/dir/19.3005883,-99.3786061/19.3903276,-99.2390682/@19.3630724,-99.3662328,12.5z/data=!4m5!4m4!1m1!4e1!1m0!3e0!5m1!1e1?entry=ttu&g_ep=EgoyMDI1MDkxNy4wIKXMDSoASAFQAw%3D%3D)

### Reglas y heurística

No quiero calcular minutos al volante: necesito una recomendación simple.

Definí una regla sencilla: si la cuota me ahorra más de 20 minutos respecto a la libre, tomar la cuota; si no, ir por la libre. La regla compara tiempo estimado y costo fijo de la caseta.

![codigo regla](/images/notificaciones-carretera/codigo-regla.png)

### Implementación y notificaciones

Preferí no depender de plataformas de mensajería pagadas por motivos de costo y privacidad. 

Aunque soy desarrollador .NET, para prototipar usé Python y Selenium para automatizar WhatsApp Web y enviar las notificaciones. Guardar la sesión del navegador permite que el bot actúe sin iniciar sesión cada vez.

El flujo es:
- medir tiempo estimado por las dos rutas usando las APIs de Maps o simulando consultas en la web;
- aplicar la heurística (diferencia de tiempo vs costo);
- enviar un mensaje a mi número con la recomendación;
- Android Auto reproduce la notificación mediante TTS.

## Experiencia de uso

El sistema me daba la información justo cuando la necesitaba y funcionó bien, aunque no siempre: mantener sesiones de WhatsApp Web automatizadas y un servicio en casa (lo ejecuté en un cluster k3s en una mini-PC) requiere mantenimiento.

Aprendí mucho sobre Selenium, persistencia de sesiones y despliegue en Kubernetes. Fue más trabajo del que esperaba, pero perfecto para aprender.

![notifiaciones whatsapp](/images/notificaciones-carretera/notificaciones-whatsapp.png)

## Intento de monetización

Probé una versión piloto con usuarios de un grupo de Facebook donde la gente comparte reportes de la carretera. Pensé que otras personas tendrían el mismo problema; estimé que el servicio podría escalar hasta unas 30 personas.

Publiqué la invitación varias veces y conseguí cuatro usuarios para la prueba.

Durante tres semanas envié encuestas periódicas; solo dos personas respondieron.

![introducción a los usuarios](/images/notificaciones-carretera/intro-usuarios.png)

Al terminar la prueba agradecí a los participantes y propuse una suscripción mensual, pero nadie aceptó pagar por el servicio.

## Reflexión

Aunque no hubo usuarios dispuestos a pagar, el proyecto me dejó mucho aprendizaje: técnicas de automatización con Selenium, administración de sesiones, despliegue en Kubernetes y las dificultades de promocionar un servicio en redes sociales. 

También me recordó que, aunque algo no sea rentable, vale la pena si aprendes y obtienes experiencia práctica.