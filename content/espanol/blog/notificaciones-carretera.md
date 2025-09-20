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
Al pasar todos los días por la carretera México-Toluca para ir mi lugar de trabajo me pregunto si siempre vale la pena pagar el precio de un tramo de este trayecto. En días que se me hace tarde tomo la autopista sin pensar dos veces, pero para mi sorpresa otros automovilistas tuvieron esa misma idea por lo que la fila de espera en la caseta se vuelve larga. 

Veo el tramo de la libre despejado y ahí pienso que no valió la pena el pago de la caseta. 

## Primera solución

Al ir con mi esposa me puedo anticipar a este escenario al pedirle que revise la carretera desde su aplicación de google maps, esto resulta práctico ya que ambos sabemos que tramo recorremos y discutimos como se ve la carretera antes de tomar la decisión.

Este acercamiento se complica cuando manejo solo a mi lugar de trabajo, al ser peligroso manipular el teléfono o el Android auto de mi carro.

## Solución técnica

Al no tener un copiloto todo el tiempo debo buscar obtener esta información de otra forma. Android auto soporta notificaciones de aplicaciones de mensajeria como Whatsapp, telegram, messenger etc. Y es capaz de reproducir estas notificaciones con su propio TTS (Text to speech)

Estas notificaciones lo vuelve perfecto para recibir un mensaje y reproducirlo para obtener la información.

### Definir las rutas y los tiempos

Mi trayecto es bastante regular, mi lugar de trabajo es fijo, trabajo de lunes a viernes con un horario de entrada y salida fijo. Los mensajes que reciba pueden ser programados o agendados los mismos días a la misma hora. 

Primero debo encontrar las rutas en google maps que me interesan, es decir los puntos de decisión cruciales

![punto de desviación en la carretera con la posibilidad de eligir cuota o libre](/images/notificaciones-carretera/desviacion-carretera.png)

Dónde existe el tramo de entrada y los tramos donde ambos caminos convergen en uno solo. Esto para poder medir el tiempo de recorrido por cada camino.

Al jugar con google maps se pueden buscar estos tramos, los cuales se definen por coordenadas dentro de la Url de google maps
[https://www.google.com/maps/dir/19.3005883,-99.3786061/19.3903276,-99.2390682/@19.3630724,-99.3662328,12.5z/...](https://www.google.com/maps/dir/19.3005883,-99.3786061/19.3903276,-99.2390682/@19.3630724,-99.3662328,12.5z/data=!4m5!4m4!1m1!4e1!1m0!3e0!5m1!1e1?entry=ttu&g_ep=EgoyMDI1MDkxNy4wIKXMDSoASAFQAw%3D%3D)

### Notificaciones

Al ser un proyecto personal no quise usar el uso de plataformas de mensajeria, principalmente por el costo y los registros en las plataformas (si, las plataformas necesitan saber el fin de su plataforma) que pueden tomar algún tiempo para su aprobación.

Soy desarrollador dotnet pero me gusta hacer prototipos en python, he utilizado selenium varias veces para realizar webscraping pero esta vez voy a interactuar con whatsapp web. Para esto es necesario otra cuenta de whatsapp de la cual enviar mensajes a mi número personal.

Con un sim adicional solo necesito iniciar sesión en el navegador y selenium puede interactuar con él.

### Reglas o heurística

Al momento de estar conduciendo no quiero pensar en los minutos que se tarda cada tramo y tomar una decisión al mismo tiempo. solamente necesito un *VETE POR LA LIBRE!!*. Para esto se definen reglas que toman en cuenta el tiempo de traslado y el costo de la caseta (fijo). 

El principio es el siguiente, si me ahorro mas de 20 minutos por la cuota me voy por ahí. 

![codigo regla](/images/notificaciones-carretera/codigo-regla.png)
## Experiencia de uso

El sistema de notificaciones me daba la información que necesitaba en el momento que necesitaba

![notifiaciones whatsapp](/images/notificaciones-carretera/notificaciones-whatsapp.png)

funcionaba bien... cuando funcionaba...

Aprendí muchisimo del funcionamiento de selenium, guardar sesiones y para darle más sabor metí esto en un cluster de kubernetes (k3s en una mini pc) de la cual voy a hablar despues en mi pequeñisisisisisimo homelab.

fue tal vez demasiado que mantener y arreglar pero era exactamente lo que quería aprender

## Intento de monetización

Estuve en un grupo de facebook donde se daban reportes de la carretera por los usuarios, de ahí pensé que otras personas podrían tener este mismo problema. Este sistema podría escalar para servir a más usuarios (estimo que máximo 30) y tal vez monetizarlo. Publique un par de veces en los grupos relacionados y obtuve 4 usuarios, incluso cree un speech como si estuvieran participando en la prueba beta de un servicio.

![introducción a los usuarios](/images/notificaciones-carretera/intro-usuarios.png)

Cada fin de semana y a la tercera semana les mandé una pequeña encuesta para saber si el servicio era lo que esperaban. Solo dos usuarios respondían las encuestas :c

Al final de tres semanas con la prueba piloto les agradecí y les propuse un monto mensual para continuar con el servicio. 

## Reflexión

Obviamente no hubo usuarios que querían pagar pero aprendí varias cosas técnicas con kubernetes, que promocionarse en grupos de facebook es muy dificil con este tipo de servicios, que es dificil intentar hacer todo al mismo tiempo y que siempre que me sirva o aprenda algo vale la pena hacer el esfuerzo.