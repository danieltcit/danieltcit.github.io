---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Git
permalink: /git/
---

# Git

* Todos los nombre de ramas de git deben escribirse utilizando kebab-case

```sh
# Use
git checkout -b new-branch
# instead of
git checkout -b new_branch
# or
git checkout -b newBranch

```

* Realizar commits periodicos minimamente a medio día y al finalizar el día, 
  utilice mensajes de commit descriptivos del módulo o incidencia que se encuentre trabajando

```sh
# Use WIP ("Work In Progress") to identify unstable commits
git add .
git commit -m "Nuevo Módulo WIP"
```

*