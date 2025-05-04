---
title: Setup
---

- Veuillez vous assurer d'avoir accès à un tableur, tel que LibreOffice, Microsoft Excel ou Google Sheets.

- Installez R, RStudio et les packages (voir ci-dessous).

### R et RStudio

- R et RStudio sont des programmes a télécharger séparemment et demandent des installations distinctes. R est l'environnement de calcul statistique; il peut être utilisé seul, mais cela peut être pénible. RStudio est un environnement de développement graphique intégré (IDE) qui rend l'utilisation de R beaucoup plus simple et plus interactive. Vous avez besoin d'installer R avant d'installer RStudio. Après avoir installé les deux programmes, vous devrez installer des _packages_ (paquets) R spécifiques depuis RStudio. Suivez les instructions pour votre système d'exploitation ci-dessous, puis suivez les instructions pour installer des paquets.

### You are running Windows

<br>

:::::::::::::::  solution

## Si vous avez déjà installé R et RStudio

- Ouvrez RStudio et cliquez sur « Aide » > « Vérifier les mises à jour ». Si une nouvelle version est disponible, quittez RStudio et téléchargez la dernière version de RStudio.

- Pour vérifier quelle version de R vous utilisez, démarrez RStudio et la première chose qui apparaît dans la console indique la version de R que vous exécutez. Alternativement, vous pouvez taper `sessionInfo()`, qui affichera également quelle version de R est installée. Allez sur le [site Web de CRAN](https://cran.r-project.org/bin/windows/base/) et vérifiez si une version plus récente est disponible. Si c'est le cas, veuillez la télécharger et l'installer. Vous pouvez [consulter cette page](https://cran.r-project.org/bin/windows/base/rw-FAQ.html#How-do-I-UNinstall-R_003f) pour plus d'informations sur la façon de supprimer les anciennes versions de votre système si vous souhaitez le faire.

- Suivez maintenant les étapes décrites dans les instructions [pour tout le monde](#for-everyone) en bas de cette page.

:::::::::::::::::::::::::

:::::::::::::::  solution

## Si vous n'avez pas encore installé R et RStudio

- Téléchargez R depuis le [site Web CRAN](https://cran.r-project.org/bin/windows/base/release.htm).

- Exécutez le fichier « .exe » qui vient d’être téléchargé

- Accédez à la [page de téléchargement de RStudio](https://www.rstudio.com/products/rstudio/download/#download)

- Sous _All Installers_, sélectionnez **RStudio xxxx.yy.zz-uuu.exe - Windows 10/11** (où x, y, z et u représentent les numéros de version)

- Double-cliquez sur le fichier pour l'installer

- Une fois installé, ouvrez RStudio pour vous assurer qu'il fonctionne et que vous n'obtenez aucun message d'erreur

- Suivez maintenant les étapes décrites dans les instructions [pour tout le monde](#for-everyone) en bas de cette page.

:::::::::::::::::::::::::

### Vous utilisez macOS

<br>

:::::::::::::::  solution

## Si vous avez déjà installé R et RStudio

- Ouvrez RStudio et cliquez sur « Aide » > « Vérifier les mises à jour ». Si une nouvelle version est disponible, quittez RStudio et téléchargez la dernière version de RStudio.

- Pour vérifier quelle version de R vous utilisez, démarrez RStudio et la première chose qui apparaît dans la console indique la version de R que vous exécutez. Alternativement, vous pouvez taper `sessionInfo()`, qui affichera également quelle version de R est installée. Allez sur le [site Web de CRAN](https://cran.r-project.org/bin/macosx/) et vérifiez si une version plus récente est disponible. Si c'est le cas, veuillez la télécharger et l'installer.

- Suivez maintenant les étapes décrites dans les instructions [pour tout le monde](#for-everyone) en bas de cette page.

:::::::::::::::::::::::::

:::::::::::::::  solution

## Si vous n'avez pas encore installé R et RStudio

- Téléchargez R depuis le [site Web CRAN](https://cran.r-project.org/bin/macosx/).

- Sélectionnez le fichier « .pkg » avec la dernière version de R

- Double-cliquez sur le fichier téléchargé pour l'installer

- Ça peut aussi être utile d'installer [XQuartz](https://www.xquartz.org/) (qui est nécessaire pour certains _packages_)

- Accédez à la [page de téléchargement de RStudio](https://www.rstudio.com/products/rstudio/download/#download)

- Sous _All Installers_, sélectionnez **RStudio xxxx.yy.zz-uuu.dmg - macOS 10.15+** (où x, y, z et u représentent les numéros de version)

- Double-cliquez sur le fichier pour l'installer

- Une fois installé, ouvrez RStudio pour vous assurer qu'il fonctionne et que vous n'obtenez aucun message d'erreur.

- Suivez maintenant les étapes décrites dans les instructions [pour tout le monde](#for-everyone) en bas de cette page.

:::::::::::::::::::::::::

### Vous utilisez Linux

<br>

:::::::::::::::  solution

## Installez R à l'aide de votre gestionnaire de paquets et de RStudio

- Suivez les instructions pour votre distribution
  sur [CRAN](https://cloud.r-project.org/bin/linux), elles fournissent des informations pour obtenir la version la plus récente de R pour la plupart des  distributions Linux. Pour la plupart des distributions, vous pourriez utiliser votre gestionnaire de paquets (par exemple, pour Debian/Ubuntu, exécutez
  `sudo apt-get install r-base`, et pour Fedora `sudo yum install R`), mais nous
  ne recommandons pas cette approche car les versions fournies par ces gestionnaires sont généralement obsolètes. Dans tous les cas, assurez-vous d'avoir au moins R 4.2.0.
- Accédez à la [page de téléchargement de RStudio](https://www.rstudio.com/products/rstudio/download/#download)
- Sous _All Installers_, sélectionnez la version qui correspond à votre distribution et installez-la avec votre méthode préférée (par exemple, avec Debian/Ubuntu `sudo dpkg -i rstudio-xxxx.yy.zz-uuu-amd64.deb` sur le terminal).
- Une fois installé, ouvrez RStudio pour vous assurer qu'il fonctionne et que vous n'obtenez aucun message d'erreur.
- Suivez maintenant les étapes dans les [instructions pour tout le monde](#for-everyone)

:::::::::::::::::::::::::

### Pour tout le monde

Après avoir installé R et RStudio, vous devez installer quelques packages
qui seront utilisés pendant l'atelier. Pendant le cours, nous verrons en détail comment installer des packages et les commandes ci-dessous. Pour l'instant, suivez simplement les instructions ci-dessous :

- Démarrez RStudio en double-cliquant sur l'icône puis tapez :

```r
install.packages(c("BiocManager", "remotes"))
BiocManager::install(c("tidyverse", "SummarizedExperiment", "hexbin", "patchwork", "gridExtra", "lubridate"))
```
