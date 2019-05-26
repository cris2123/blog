---
title: "Hello Hugo"
date: 2019-05-21T06:36:15-04:00
draft: true
mathjax: true
---

# Primer post en Hugo

<nav class="toc" aria-labelledby="toc-heading">
  <h2 id="toc-heading">Table of contents</h2>
  <ol>
    <li>
      <a href="#que-es-hugo">Que es Hugo?</a>
    </li>
    <li>
      <a href="#instalar-hugo">Instalar Hugo</a>
    </li>
    <li>
      <a href="#comandos-en-hugo">Comandos para hugo</a>
    </li>
    <li>
      <a href="#shortcodes-en-hugo">Shortcodes in Hugo</a>
    </li>
    <li>
      <a href="#latex-con-hugo-y-cupper">Latex con Hugo y Cupper</a>
    </li>
    <li>
      <a href="#deployment-de-hugo-en-github-pages">Deployment de hugo en github pages</a>
    </li>
  </ol>
</nav>

## Que es hugo ? 

Hugo es un generador de páginas estáticas el cual se encuentra escrito en Go, con el podemos  generar sitios estáticos escritos en  markdown de una manera rápida y sencilla.


## Instalar Hugo 

Para instalarlo y configurarlo es mejor seguir la documentación oficial que se encuentra [aquí](https://gohugo.io/getting-started/installing/)

Para esta instalación se usarón las instrucciones para *_Windows 10_* presentes en el link anterior con la siguiente estructura de carpetas:

{{% fileTree %}}
*  C:\\Hugo
    * bin
    * sites
{{% /fileTree %}}

Una vez instalado y configurado el path como en la documentación se utiliza el siguiente comando para generar el sitio

{{< cmd >}}

hugo new site <my_site>

{{< /cmd >}}

Y de alli se generara una nueva carpeta, quedando la estructura de archivos de la siguiente manera:

{{% fileTree %}}
*  C:\\Hugo
    * bin
    * sites
		* my_site
			* content/
			* data/
			* layouts/
			* archetypes/
			* resources/
			* static/
			* themes/
			* config.toml
			
{{% /fileTree %}}

De manera general una vez instalado Hugo en el sistema, podemos crear cualquier carpeta y dentro de ella utilizar los comandos anteriores, no es necesarios, que se encuentre en el mismo sitio que la instalación de Hugo. 


{{% note %}}
Hugo no viene por defecto con ningún tema definido asi que al momento de configurarse es necesario especificar que tema será el elegido
Se pueden revisar los temas disponibles en [el sitio oficial](https://themes.gohugo.io/)
{{% /note %}}

Para instalar el tema simplemente debemos tomar el tema como un submodulo de nuestro repositorio, para ello primero debemos iniciar nuestro sitio como un repositorio y luego traer el submodulo, para control de versiones en estos momentos se esta usando git.

Para instalar el tema se utiliza:

```shell
C:\Users\your_user> git init
C:\Users\your_user> git submodule add link_to_repo themes/theme_name
```

Todas las configuraciones de hugo se realizan atraves del archivo *config.toml*, alli se debe especificar el tema a utilizar y algunos otros parámetros del sitio que estamos creando. Modificaremos el archivo para agregar la plantilla a utilizar con la siguiente linea: 

`theme = "your_theme"`

Por ultimo correremos el servior que viene con Hugo para el sitio generado:

{{< cmd >}}

hugo server -D

{{< /cmd >}}


## Comandos para hugo

Para crear un nuevo post se usa el comando:

{{< cmd >}}

  hugo new /posts/<post_name.md>

{{< /cmd >}}



## Configuraciones de Hugo

En construccion

## Shortcodes en Hugo

En hugo los shortcodes son snippets de templates que se colocan cuando se estan escribiendo el markdown, estos 
son muy utiles para extender las capacidades del markdown de una manera mucho mas limpia usando el template engine
por defecto. 

Entre algunos shortcodes se tiene

*cmd*:

```
{{< cmd >}}

hugo server -D

{{< /cmd >}}
```

*notas*

{{% note %}}

  Esto es una nota
{{% /note %}}

Entre otros que se iran agregando a medida que se vayan utilizando

## Latex con Hugo y Cupper

  Para este setup se instalará el tema de cupper para nuestro blog de Hugo, pero debido a que haremos modificaciones 
  del tema para darle soporte a mathjax, necesitaremos hacer un fork del repositorio original del tema y en nuestra pagina de `Hugo` agregaremos como submodulo a este fork, como se vió en la sección de `instalar hugo` 

  Para instalar mathjax con el template de Hugo debemos modificar la plantilla de cupper. Para ello primero
  crearemos el archivo `mathjax_support.html` con lo siguiente:

  
  ```html
  <script type="text/javascript" async
    src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
    MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [['$','$'], ['\\(','\\)']],
        displayMath: [['$$','$$'], ['\[','\]']],
        processEscapes: true,
        processEnvironments: true,
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
        TeX: { equationNumbers: { autoNumber: "AMS" },
            extensions: ["AMSmath.js", "AMSsymbols.js"] }
    }
    });

    MathJax.Hub.Queue( function() {
        // Fix <code> tags after MathJax finishes running. This is a
        // hack to overcome a shortcoming of Markdown. Discussion at
        // https://github.com/mojombo/jekyll/issues/199
        var all = MathJax.Hub.getAllJax();
        for(var i = 0; i < all.length; i++){
            all[i].SourceElement().parentNode.className += ' has-jax';
        }
    });

  </script>


  ```

  Este archivo se debera colocar en la ruta del tema de cupper `theme/cupper-hugo-theme/layouts/partials`

  Ya con el archivo en su lugar queda es llamar a dicho archivo desde algun elemento de la plantilla que siempre 
  se renderizará, puede ser el footer o el header, para este caso se colocara en el archivo de header.html en la 
  misma carpeta de partials. 

  Se agregara de la siguiente forma al archivo

  ```html

  <header class="intro-and-nav" role="banner">
      .
      .
      .
      .
      <!-- header content  -->
      .
      .

    {{ if .Params.mathjax }}
      {{ partial "mathjax_support.html" . }}
    {{ end }}
</header>

  ``` 

  Como se hizo que mathjax fuera renderizado de manera condicional, es decir, se verifica un flag
  para cargar el archivo, debemos configurar en cada post que se realize el flag de 
  `mathjax: true` para poder utilizarlos. 

  Algunos ejemplos se muestran a continuación:

  `$\sqrt{(3x-1)}+(1+x)^2$`

  `$\sqrt{3}$`

  $$ 
    \lim_{ n \to \infty } {\rm Pr} 
    \left ( 
        \sqrt{n} \left ( \frac{ \bar{ X_n } - \mu }{ \sigma } 
                \right ) \leq x 
    \right ) 
    = \int^x\_{ -\infty } \frac{ 1 }{ \sqrt{ 2 \pi } } e^{ -y^2 / 2 }\,{\rm d}y 
  $$

  Also inline mode: $ \mathbf{ \hat{ \beta } }
 = \left ( \mathbf{X}' \mathbf{X} \right )^{-1} \mathbf{X}' \mathbf{y} $.

  `$S_n = \sum_{i=1}^n X_i$`


{{% note %}}
En algunos browsers como chrome para windows algunos simbolos no son renderizados correctamente, 
debido a esto pueden existir problemas al renderizar algunas ecuaciones, entre los simbolos 
problemas encontrados actualmente se tienen: 


`$\vec{B}$` --> `$\overrightarrow{B}$`
`\vec{B}` --> cambiarse por `\overrightarrow{B}`
{{% /note %}}

  

## Deployment de hugo en github pages

En las secciones anteriores hablamos de como utilizar `Hugo` para crear posts, configurarlo y agregarle temas y modificarlos. basandonos en esas secciones crearemos una página de github con nuestro blog de hugo.

Para hacer el deployment de Hugo en una página de Github, es necesario crear un repositiorio
con el mismo nombre de nuestro usuario de github y la terminacion github.io,  (`<githubser>.github.io`). Un ejemplo es este blog `cris2123.github.io`.

Los pasos para hacer todo el proceso se pueden encontrar en este enlace [Github Pages](https://pages.github.com/)

Para este caso no se generará nuestro site con `Jekyll` (herramienta que soporta nativamente github), sino con Hugo por lo que la configuración será un poco distinta.

Dado que `Hugo` no esta soportado directamente por Github en estos momentos, lo que es necesario es crear otro repositorio
donde tendremos los archivos de nuestro blog, y contenerá como submodulo el repositorio donde estan cargados todos los 
assest de nuestra web (el repositorio que fue creado al inicio de esta  en mi caso `cris2123.github.io`)

Este nuevo repositorio se puede llamar de cualquier manera, en mi caso lo llamé `blog`.

Ya creado nuestros repositorios, debemos clonarlo en el directorio local que queramos para hacer modificaciones a los archivos que crearemos para nuestro blog y hacer algunas otras configuraciones pertinentes.

{{< cmd >}}

  mkdir blog
  
  cd blog
  git clone <remote_url_blog>
  git clone <remote_url_page>

  cd ../blog/

{{< /cmd >}}

Donde <remote_url_blog> es el repositorio `blog` que creamos para guardar los post del blog
y <remote_url_page> es el repositorio donde github almacena nuestra pagina `<githubser>.github.io`

Ingresamos a la carpeta `blog` que clonamos de nuestro repositorio remoto y utilizamos el siguiente comando para agregar nuestro blog de Hugo

{{< cmd >}}

  hugo new site <new_site_name>

{{< /cmd >}}

Donde `<new_site_name>` es el nombre de nuestro proyecto de Hugo. La estructura de archivos quedó de la siguiente manera:

{{% fileTree %}}
*  C:\\Hugo
    * bin
    * sites
		* new_site_name
			* content/
			* data/
			* layouts/
			* archetypes/
			* resources/
			* static/
			* themes/
			* config.toml
			
{{% /fileTree %}}


Despues de crear el directorio ingresamos al directorio `<new_site_name>` y ejecutamos el siguiente comando:

{{< cmd >}} 
  git submodule add <repo_url>
{{< cmd >}} 

donde <repo_url> es el enlace del repositorio remoto en donde esta nuestra pagina, que es el mismo que utilizamos
en los pasos anteriores. 

Como se vio en la sección de `instalación de Hugo` debemos agregar como submodulo el repositorio del tema, si queremos usar mathjax , agregar como submodulo al fork del tema con :

{{< cmd >}} 
  git submodule add <repo_theme_fork_url>
{{< cmd >}} 

Cuandp ya hemos creado el submodulo dentro del repositorio principal, podemos hacer un commit de nuestros cambios y un push a la branch remota, para ya tener configurado el remoto, si no de igual manera es posible continuar con el siguiente paso, el cual consiste en configurar el archivo `config.toml`  generado por Hugo.

En dicho archivo agregaremos lo siguiente: 

```toml

baseURL = "https://<github_username>.github.io/"
languageCode = "en-us"
title = "Your title"
theme = "your-theme"
publishDir = "<github_username>.github.io"

```

Lo mas importante de esta configuración es el `baseURL` el cual indicará la página 
o host en el cual se mostrarán  los archivos de nuestros repos y el parámetro de `publishDir`, el cual le indicará a hugo donde queremos que se guarden los archivos generados al compilar nuestros Markdowns con el comando `hugo`. En este caso lo seleccionamos para que apunte al directorio de nuestro submodulo, por lo cual cada vez que editemos los posts, assets, etc, para nuestro blog y decidamos publicarlos, los mismos se copiaran en el repositiorio de las páginas de github y podrems hacerles commit directamente. 

Ya con esto configurado, crearemos un post con el comando

{{< cmd >}}

  hugo new /posts/<post_name.md>

{{< /cmd >}}

Escribiremos algo en nuestro post y utilizaremos el comando

{{< cmd >}}

  hugo --buildDrafts

{{< /cmd >}}

Este último comando generará dentro de nuestro submodulo <github_username>.github.io todos los archivos necesarios para 
la página estática.

Por ultimo para actualizar nuestro repositorio remoto, entramos a nuestro submodulo <github_user>.github.io 
y haces un git commit y git push de nuestros cambios. De alli, podremos visualizar la data desde nuestra página de github


Otra cosa importante de destacar es que si a `hugo` no se le especifica el párametro `publishDir`, crea una carpeta llamada `public` en donde se generan los archivos para el deployment. Si no quisieramos cambiar este comportamiento, es posible correr el comando `git submodule add <repo_url> public` de manera que la carpeta de nuestro submodulo se llame `public`, y los archivos generados se guarden alli directamente.