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

De manera general una vez instalado Hugo en el sistema, podemos crear cualquier carpeta y dentro de ella utilizar los comandos anteriores, no es necesarios, que 
se encuentre en el mismo sitio que la instalación de Hugo. 


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

Todas las configuraciones de hugo se realizan atraves del archivo *config.toml*, alli se debe especificar el tema a utilizar y algunos otros parámetros del sitio que estamos creando. En este caso lo modificaremos para agregar la plantilla a utilizar. En este caso se agrega al archivo la siguiente línea: 

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

En construcciones

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

  Aqui probaremos si la configuracion de mathjax funciono en cupper

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
