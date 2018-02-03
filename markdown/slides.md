layout: true

---
class: middle, center, general
# Integración Continua
## Una introducción usando Docker y GitLab
Una pequeña charla por [Carlos González](http://caal-15.github.io)

Síguela en [caal-15.github.io/ci-talk](http://caal-15.github.io/image-stitching-talk)
.footnote[.alt-link[[Documentación de GitLab CI](https://about.gitlab.com/features/gitlab-ci-cd/)]]

---
class: center, middle

# Conceptos Básicos

---
class: left, middle

# Integración Continua

.big[
  La meta principal de la __CI__ es evitar problemas de integración en el
  software, en la práctica suelen usarse un conjunto de herramientas para
  cumplirla como:

  * _Git_ (Manejador de versiones).
  * _Librerías de unittest_ (Varían de lenguaje a lenguaje).
  * Un _pipeline en la nube_ que corra las pruebas (__Jenkins__, __Shippable__,
    etc.).
]

---
class: left, middle

# ¿Por qué usar CI?

* .big[Porque somos _humanos_.]

* .big[Facilita el mantinimiento del código e integración de código nuevo.]

* .big[Evita tirarse el ambiente de producción y mejor aún __tener que explicar
  por qué no funciona el ambiente de producción!__.]

---
class: left middle

# Docker

.big[
  Herramienta de virtualización y aislamiento en _contenerdores_, __Docker__
  es relativamente reciente, y posee muchas ventajas:

  * Los _contenerdores_ proveen un ambiente de ejecución aislado y consistente
    para que las aplicaciones corran siempre bajo las __mismas condiciones__.
  * Habla directamente con el __kernel__ de __Linux__ lo que lo hace
    más _ágil_ que otras herramientas de virtualización.
  * Debido a su manera de funcionar, no requiere un __sistema operativo__
    separado, lo que hace a sus imágenes _livianas_.
]

.footnote[.alt-link[[Página de Docker](https://www.docker.com/)]]

---
class: left middle

# GitLab

.big[
  Es un servicio de __hosting__ para repositorios basados en __Git__, es
  relativamente joven comparado con __GitHub__, sin embargo tiene grandes
  ventajas:

  * Infinitos _repositorios privados_ sin costo.
  * Incluye un _pipeline_ de __CI__, __CD__.
  * Se puede usar como servicio _self-hosted_ de manera privada.
]

.footnote[.alt-link[[Página de GitLab](https://about.gitlab.com/)]]

---
class: center, middle

# El pipeline

---
class: left, middle

# ¿Cómo funciona?

.big[
El _pipeline_ requiere de dos tecnologías diferentes, ambas desarrolladas por
__GitLab__:

* Un _hosting_ de __GitLab__ como tal (por ejemplo __gitlab.com__).
* _Runners_, los encargados de correr las pruebas y otros comandos (se pueden
  usar los runners comunitarios de __gitlab.com__).
]

---
class: left, middle

# En el hosting

.big[
  El hosting se encarga de mantener el código disponible para los _runners_,
  también se encarga de mostrar el progreso de los runners en la _UI_, pero lo
  que más nos interesa es el archivo __.gitlab-ci.yml__ que se mantiene en cada
  _repositorio_.
]

---
class: left, middle

# En el hosting: el archivo de CI

.big[
  Es un archivo __YAML__ (__.gitlab-ci.yml__) encargado de especificar la
  configuración de __CI__ para cada repositorio, usualmente contiene las
  secciones:

  * _image:_ la imagen de __Docker__ a usar.
  * _stages:_ los nombres de las diferentes etapas del _pipeline_.
  * _before_script:_ comandos a ejecutar antes de cada etapa.
  * _secciones de usuario:_ cada una de las tareas que el _pipeline_ va a
    ejecutar.
]

---
class: left, middle

# Ejemplo
```YAML
image: node:8.9.4

stages:
  - test

before_script:
  - npm install

test_unit:
  stage: test
  script: npm test
```

---
class: left, middle

# En el Runner:

.big[
  En el corazón del _Runner_ se encarga continuamente de pedir trabajos al
  _hosting_, en su corazón está __Docker__ que se encarga de descargar y
  construir la imagen provista en __.gitlab-ci.yml__, y luego:

  * Se descarga el _repositorio_ en la máquina.
  * Se corren los comandos provistos en _before_script_.
  * Se corren las etapas en el orden en el que aparezcan en _stages_,
  * Si una etapa falla, las siguientes no se ejecutan
  * Las tareas de cada etapa se ejecutan en el orden en el que aparecen en el
    archivo.
]

.footnote[.alt-link[[Instala tus propios runners](https://docs.gitlab.com/runner/install/)]]

---
class: center, middle

# Consideraciones Adicionales.

.big[
  Para sacar el mayor provecho del _pipeline_ se debe usar en conjunto con
  otras prácticas, incluyendo buen desarrollo de _tests_, y el uso de otras
  herramientas que proporciona __GitLab__ como _merge requests_ del código.
]

---
class: center, middle

# Ahora a practicar!, Muchas Gracias por su Atención.

.footnote[_Me pueden invitar una cerveza despúes de la charla_]
