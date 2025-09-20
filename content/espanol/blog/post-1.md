---
title: "Documentando con un generador de sitios estáticos"
meta_title: "Uso de generador de sitios estáticos para un blog"
description: "Un blog dónde se documenta el uso de un generador de sitios estaticos y archivos tipo markdown para publicar de forma fácil en github pages"
date: 2025-09-17T12:00:00Z
image: "/images/hugo/hugo.png"
categories: ["Proyectos", "Ingeniería"]
author: "Erick López"
tags: ["proyecto","jamstack","blog", "hugo"]
draft: false
---

# Introducción
Este proyecto es mi tercera iteración del uso de Generadores de Sitios Estáticos (SSG static site generator). Estos sitios son construidos con un framework y el contenido es basado en plantillas con archivos markdown (.md) lo cual tiene una gran flexibilidad al crear contenido escrito (títulos, substitulos, listas, imágenes etc.)

En previos proyectos encontré la simplicidad de generar archivos html, al no necesitar un servicio no se necesita un hosting.

# Developer experience

Al configurar el ambiente local (proyecto y scripts) y Github pages (2 clicks) el proceso de desarrollar se detiene y empieza la creación de contenido.

## Ambiente local

* Instalar [Hugo](https://gohugo.io/) y sus dependencias (windows 11 en mi caso)
* Crear proyecto `hugo new site <site-name>` e iniciar el control de versiones `git init`
* Buscar una [plantilla](https://themes.gohugo.io/) de la lista oficial, agregar como submodulo
* Personalizar al sitio de acuerdo a tu gusto
  
## Github pages

Al publicar tu proyecto en github y tener tu repositorio público en la sección de ajustes se encuentra el menú de Pages, al seleccionar el folder dónde se encuentra el proyecto compilado Github genera una url pública la cual es accesible.

## Creación de contenido

Al crear un nuevo archivo .md en el folder "blog", generar el compilado y realizar commit. El proceso inicia automaticamente y a los pocos minutos se encuentra disponible en la url pública.