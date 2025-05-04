# Contribuyendo

[Software Carpentry][swc-site] y [Data Carpentry][dc-site] son proyectos de código abierto,
y agradecemos contribuciones de todo tipo:
nuevas lecciones,
Se aceptan correcciones al material existente,
informes de errores,
y revisiones de los cambios propuestos.

## Acuerdo de colaborador

Al contribuir,
, aceptas que podemos redistribuir tu trabajo bajo [nuestra licencia](LICENSE.md).
A cambio,
abordaremos sus problemas y/o evaluaremos su propuesta de cambio lo antes posible,
y le ayudaremos a convertirse en miembro de nuestra comunidad.
Todos los involucrados en [Software Carpentry][swc-site] y [Data Carpentry][dc-site]
aceptan cumplir con nuestro [código de conducta](CONDUCT.md).

## Cómo contribuir

La forma más fácil de comenzar es presentar un problema
para informarnos sobre un error ortográfico,
alguna redacción incómoda,
o un error factual.
Esta es una buena manera de presentarse
y conocer a algunos de los miembros de nuestra comunidad.

1. Si no tienes una cuenta de [GitHub][github],
    puedes \[enviarnos comentarios por correo electrónico]\[contacto].
    Sin embargo,
    podremos responder más rápidamente si utiliza uno de los otros métodos que se describen a continuación.

2. Si tienes una cuenta [GitHub][github],
    o estás dispuesto a [crear una][github-join],
    pero no sabes cómo usar Git,
    Puede informar problemas o sugerir mejoras \[creando un problema]\[problemas].
    Esto nos permite asignar el elemento a alguien
    y responderle en una discusión encadenada.

3. Si se siente cómodo con Git,
    y le gustaría agregar o cambiar material,
    , puede enviar una solicitud de extracción (PR).
    Las instrucciones para hacer esto se [incluyen a continuación] (#using-github).

## Dónde contribuir

1. Si desea cambiar esta lección,
    , trabaje en <https://github.com/swcarpentry/shell-novice>,
    , que se puede ver en <https://swcarpentry.github.io/shell-novice>.

2. Si desea cambiar la lección de ejemplo,
    , trabaje en <https://github.com/carpentries/lesson-example>,
    , que documenta el formato de nuestras lecciones
    y se puede ver en <https://carpentries.github.io/lesson-example>. .

3. Si desea cambiar la plantilla utilizada para los sitios web de los talleres,
    trabaje en <https://github.com/carpentries/workshop-template>.
    La página de inicio de ese repositorio explica cómo configurar sitios web de talleres,
    , mientras que las páginas adicionales en <https://carpentries.github.io/workshop-template>
    brindan más antecedentes sobre nuestras opciones de diseño.

4. Si desea cambiar archivos de estilo CSS, herramientas,
    o texto estándar HTML para lecciones o talleres almacenados en `_includes` o `_layouts`,
    , trabaje en <https://github.com/carpentries/styles>.

## Qué contribuir

There are many ways to contribute,
from writing new exercises and improving existing ones
to updating or filling in the documentation
and submitting [bug reports][issues]
about things that don't work, aren't clear, or are missing.
Si está buscando ideas,
consulte \[la lista de problemas para este repositorio]\[problemas],
o los problemas para [Data Carpentry][dc-issues]
y proyectos de [Carpintería de Software][swc-issues].

Los comentarios sobre problemas y revisiones de solicitudes de extracción son igualmente bienvenidos:
somos más inteligentes juntos que solos.
Las reseñas de principiantes y recién llegados son particularmente valiosas:
es fácil para las personas que han estado usando estas lecciones por un tiempo
olvidar lo impenetrable que puede ser parte de este material,
tan fresco Los ojos siempre son bienvenidos.

## Qué _no_ contribuir

Nuestras lecciones ya contienen más material del que podemos cubrir en un taller típico,
, por lo que generalmente _no_ buscamos más conceptos o herramientas para agregarles.
Como regla general,
si quieres presentar una idea nueva,
debes (a) estimar cuánto tiempo tomará enseñar
y (b) explicar lo que tomaría para hacerle espacio.
El primero anima a los contribuyentes a ser honestos acerca de los requisitos;
el segundo, pensar mucho en las prioridades.

Tampoco buscamos ejercicios u otro material que solo se ejecute en una plataforma.
Nuestros talleres suelen contener una combinación de usuarios de Windows, macOS y Linux;
para que sean utilizables,
nuestras lecciones deben funcionar igualmente bien en los tres.

## Usando GitHub

Si eliges contribuir a través de GitHub,
es posible que desees consultar
\[Cómo contribuir a un proyecto de código abierto en GitHub]\[cómo contribuir].
En breve:

1. La copia publicada de la lección se encuentra en la rama `gh-pages` del repositorio
    (para que GitHub la regenere automáticamente).
    Cree todas las ramas a partir de eso,
    y combine la rama `gh-pages` del [repositorio maestro][repo] con su rama `gh-pages`
    antes de comenzar a trabajar.
    Por favor _no_ trabaje directamente en su rama `gh-pages`,
    ya que eso le dificultará trabajar en otras contribuciones.

2. Usamos [GitHub flow][github-flow] para gestionar los cambios:
    1. Cree una nueva rama en su copia de escritorio de este repositorio para cada cambio significativo.
    2. Confirme el cambio en esa rama.
    3. Empuje esa rama a su bifurcación de este repositorio en GitHub.
    4. Envíe una solicitud de extracción desde esa rama al \[repositorio maestro]\[repositorio].
    5. Si recibe comentarios,
        realice cambios en su escritorio y envíelos a su sucursal en GitHub:
        la solicitud de extracción se actualizará automáticamente.

Cada lección tiene dos mantenedores que revisan los problemas y generan solicitudes
o alientan a otros a hacerlo.
Los mantenedores son voluntarios de la comunidad,
y tienen la última palabra sobre lo que se integra en la lección.

## Otros recursos

La discusión general sobre [Software Carpentry][swc-site] y [Data Carpentry][dc-site]
ocurre en la \[lista de correo de discusión]\[lista de discusión],
, donde todos son bienvenidos unir.
También puede \[comunicarse con nosotros por correo electrónico]\[contactar].

[contact]: <correo a:admin@software-carpentry.org>
[dc-issues]: <https://github.com/issues?q=user%3Acarpintería de datos>
[dc-lessons]: http://datacarpentry.org/lessons/
[dc-site]: http://datacarpentry.org/
[discuss-list]: http://lists.software-carpentry.org/listinfo/discuss
[github]: http://github.com
[github-flow]: https://guides.github.com/introduction/flow/
[github-join]: https://github.com/join
[how-contribute]: https://egghead.io/series/how-to-contribute-to-an-open-source-project-on-github
[issues]: https://github.com/swcarpentry/shell-novice/issues/
[repo]: https://github.com/swcarpentry/shell-novice/
[swc-issues]: https://github.com/issues?q=user%3Aswcarpentry
[swc-lessons]: http://software-carpentry.org/lessons/
[swc-site]: http://software-carpentry.org/
