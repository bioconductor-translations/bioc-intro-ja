---
title: Configuración
---

- Asegúrese de tener a mano un editor de hojas de cálculo, como
  LibreOffice, Microsoft Excel o Google Sheets.

- Instale R, RStudio y paquetes (ver más abajo).

### R y RStudio

- R y RStudio son descargas e instalaciones independientes. R es el
  entorno informático estadístico subyacente, pero usar R solo
  no es divertido. RStudio es un entorno de desarrollo gráfico integrado
  (IDE) que hace que el uso de R sea mucho más fácil e interactivo. Necesita
  para instalar R antes de instalar RStudio. Después de instalar ambos programas
  , necesitarás instalar algunos paquetes R específicos dentro de
  RStudio. Siga las instrucciones a continuación para su sistema operativo,
  y luego siga las instrucciones para instalar paquetes.

### Estas ejecutando Windows

<br>

::::::::::::::: solución

## Si ya tienes R y RStudio instalados

- Abra RStudio y haga clic en "Ayuda" > "Buscar actualizaciones". Si hay una nueva versión
  disponible, salga de RStudio y descargue la última versión de RStudio.

- Para verificar qué versión de R estás usando, inicia RStudio y lo primero
  que aparece en la consola indica la versión de R que estás
  ejecutando. Alternativamente, puede escribir `sessionInfo()`, que también mostrará
  qué versión de R está ejecutando. Vaya
  al [sitio web de CRAN](https://cran.r-project.org/bin/windows/base/) y verifique
  si hay una versión más reciente disponible. Si es así, descárguelo e instálelo
  . Puede [consulte aquí](https://cran.r-project.org/bin/windows/base/rw-FAQ.html#How-do-I-UNinstall-R_003f) para obtener
  más información sobre cómo eliminar versiones antiguas de su sistema si así lo desea.

- Siga los pasos de las instrucciones [para todos](#para-todos) en la
  parte inferior de esta página.

::::::::::::::::::::::::::::

::::::::::::::: solución

## Si no tienes R y RStudio instalados

- Descargue R desde
  el [sitio web de CRAN](https://cran.r-project.org/bin/windows/base/release.htm).

- Ejecute el archivo `.exe` que acaba de descargar

- Vaya a la [página de descarga de RStudio](https://www.rstudio.com/products/rstudio/download/#download)

- En _Todos los instaladores_ seleccione **RStudio xxxx.yy.zz-uuu.exe - Windows 10/11** (donde x, y, z y u representan números de versión)

- Haga doble clic en el archivo para instalarlo.

- Una vez que esté instalado, abra RStudio para asegurarse de que funcione y no reciba ningún mensaje de error
  .

- Siga los pasos de las instrucciones [para todos](#para-todos) en la
  parte inferior de esta página.

::::::::::::::::::::::::::::

### Estás ejecutando macOS

<br>

::::::::::::::: solución

## Si ya tienes R y RStudio instalados

- Abra RStudio y haga clic en "Ayuda" > "Buscar actualizaciones". Si hay una nueva versión
  disponible, salga de RStudio y descargue la última versión de RStudio.

- Para comprobar la versión de R que estás utilizando, inicia RStudio y lo primero
  que aparece en el terminal indica la versión de R que estás ejecutando. Alternativamente, puede escribir `sessionInfo()`, que
  también mostrará qué versión de R está ejecutando. Vaya
  al [sitio web de CRAN](https://cran.r-project.org/bin/macosx/) y verifique
  si hay una versión más reciente disponible. Si es así, descárguelo e instálelo
  .

- Siga los pasos de las instrucciones [para todos](#para-todos) en la
  parte inferior de esta página.

::::::::::::::::::::::::::::

::::::::::::::: solución

## Si no tienes R y RStudio instalados

- Descargue R desde
  el [sitio web de CRAN](https://cran.r-project.org/bin/macosx/).

- Seleccione el archivo `.pkg` para la última versión de R

- Haga doble clic en el archivo descargado para instalar R

- También es una buena idea instalar [XQuartz](https://www.xquartz.org/) (necesario
  en algunos paquetes)

- Vaya a la [página de descarga de RStudio](https://www.rstudio.com/products/rstudio/download/#download)

- En _Todos los instaladores_ seleccione **RStudio xxxx.yy.zz-uuu.dmg - macOS 10.15+** (donde x, y, z y u representan números de versión)

- Haga doble clic en el archivo para instalar RStudio

- Una vez que esté instalado, abra RStudio para asegurarse de que funcione y no reciba ningún mensaje de error
  .

- Siga los pasos de las instrucciones [para todos](#para-todos) en la
  parte inferior de esta página.

::::::::::::::::::::::::::::

### Estás ejecutando Linux

<br>

::::::::::::::: solución

## Instale R usando su administrador de paquetes y RStudio

- Siga las instrucciones para su distribución
  de [CRAN](https://cloud.r-project.org/bin/linux), ellas brindan información
  para obtener la versión más reciente de R para distribuciones comunes. Para la mayoría de las distribuciones
  , puede usar su administrador de paquetes (por ejemplo, para Debian/Ubuntu ejecute
  `sudo apt-get install r-base`, y para Fedora `sudo yum install R`), pero
  no recomendamos este enfoque ya que las versiones proporcionadas por este
  generalmente están desactualizadas. En cualquier caso, asegúrese de tener al menos R 4.2.0.
- Vaya a la página de descarga de RStudio

- En _Todos los instaladores_ seleccione la versión que coincida con su distribución e
  instálela con su método preferido (por ejemplo, con Debian/Ubuntu `sudo dpkg -i rstudio-xxxx.yy.zz-uuu-amd64.deb ` en la terminal).
- Una vez que esté instalado, abra RStudio para asegurarse de que funcione y no reciba ningún mensaje de error
  .
- Sigue los pasos de las [instrucciones para todos](#para-todos)

::::::::::::::::::::::::::::

### Para todo el mundo

Después de instalar R y RStudio, necesita instalar un par de paquetes
que se utilizarán durante el taller. También aprenderemos
sobre la instalación de paquetes durante el curso para explicar los siguientes comandos
. Por ahora, simplemente siga las instrucciones a continuación:

- Inicie RStudio haciendo doble clic en el icono y luego escriba:

```r
install.packages(c("BiocManager", "remotes"))
BiocManager::install(c("tidyverse", "SummarizedExperiment", "hexbin",
                       "patchwork", "gridExtra ", "lubricar"))
```
