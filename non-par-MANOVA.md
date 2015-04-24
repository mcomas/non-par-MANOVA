# Non parametric Manova



## `vegan` package

Extret del paquet:

> Analysis of variance using distance matrices — for partitioning distance matrices among sources of variation and fitting linear models (e.g., factors, polynomial regression) to distance matrices; uses a permutation test with pseudo-F ratios.

La metolodogia està explicada a  [(Anderson, M.J. 2001)](http://onlinelibrary.wiley.com/doi/10.1111/j.1442-9993.2001.01070.pp.x/abstract?deniedAccessCustomisedMessage=&userIsAuthenticated=false). Defineix un estadístic de contrast F a partir de distàncies entre individus (equació (3)). Després, per calcular un p-valor, realitza un test de permutacions per decidir si els grups són igual o no.




```r
library(vegan)
```

```
> Loading required package: permute
> Loading required package: lattice
> This is vegan 2.2-1
```

```r
?adonis
```

El paquet treballa amb dos data.frames. El primer és on es defineix la variable multivariant explicada. En l'exemple utilitzen comptatges de plantes trobades en 20 zones diferents (fa tota la pinta a ser composicional, això si, amb molts zeros :-( )).


```r
data(dune)
str(dune)
```

```
> 'data.frame':	20 obs. of  30 variables:
>  $ Achimill: num  1 3 0 0 2 2 2 0 0 4 ...
>  $ Agrostol: num  0 0 4 8 0 0 0 4 3 0 ...
>  $ Airaprae: num  0 0 0 0 0 0 0 0 0 0 ...
>  $ Alopgeni: num  0 2 7 2 0 0 0 5 3 0 ...
>  $ Anthodor: num  0 0 0 0 4 3 2 0 0 4 ...
>  $ Bellpere: num  0 3 2 2 2 0 0 0 0 2 ...
>  $ Bromhord: num  0 4 0 3 2 0 2 0 0 4 ...
>  $ Chenalbu: num  0 0 0 0 0 0 0 0 0 0 ...
>  $ Cirsarve: num  0 0 0 2 0 0 0 0 0 0 ...
>  $ Comapalu: num  0 0 0 0 0 0 0 0 0 0 ...
>  $ Eleopalu: num  0 0 0 0 0 0 0 4 0 0 ...
>  $ Elymrepe: num  4 4 4 4 4 0 0 0 6 0 ...
>  $ Empenigr: num  0 0 0 0 0 0 0 0 0 0 ...
>  $ Hyporadi: num  0 0 0 0 0 0 0 0 0 0 ...
>  $ Juncarti: num  0 0 0 0 0 0 0 4 4 0 ...
>  $ Juncbufo: num  0 0 0 0 0 0 2 0 4 0 ...
>  $ Lolipere: num  7 5 6 5 2 6 6 4 2 6 ...
>  $ Planlanc: num  0 0 0 0 5 5 5 0 0 3 ...
>  $ Poaprat : num  4 4 5 4 2 3 4 4 4 4 ...
>  $ Poatriv : num  2 7 6 5 6 4 5 4 5 4 ...
>  $ Ranuflam: num  0 0 0 0 0 0 0 2 0 0 ...
>  $ Rumeacet: num  0 0 0 0 5 6 3 0 2 0 ...
>  $ Sagiproc: num  0 0 0 5 0 0 0 2 2 0 ...
>  $ Salirepe: num  0 0 0 0 0 0 0 0 0 0 ...
>  $ Scorautu: num  0 5 2 2 3 3 3 3 2 3 ...
>  $ Trifprat: num  0 0 0 0 2 5 2 0 0 0 ...
>  $ Trifrepe: num  0 5 2 1 2 5 2 2 3 6 ...
>  $ Vicilath: num  0 0 0 0 0 0 0 0 0 1 ...
>  $ Bracruta: num  0 0 2 2 2 6 2 2 2 2 ...
>  $ Callcusp: num  0 0 0 0 0 0 0 0 0 0 ...
```

El segon data.frame, és el que conté les covariables. Hi han diferents caracteristiques del terreny on es fan els comptatges.


```r
data(dune.env)
str(dune.env)
```

```
> 'data.frame':	20 obs. of  5 variables:
>  $ A1        : num  2.8 3.5 4.3 4.2 6.3 4.3 2.8 4.2 3.7 3.3 ...
>  $ Moisture  : Ord.factor w/ 4 levels "1"<"2"<"4"<"5": 1 1 2 2 1 1 1 4 3 2 ...
>  $ Management: Factor w/ 4 levels "BF","HF","NM",..: 4 1 4 4 2 2 2 2 2 1 ...
>  $ Use       : Ord.factor w/ 3 levels "Hayfield"<"Haypastu"<..: 2 2 2 2 1 2 3 3 1 1 ...
>  $ Manure    : Ord.factor w/ 5 levels "0"<"1"<"2"<"3"<..: 5 3 5 5 3 3 4 4 2 2 ...
```

Imagino que per un contrast MANOVA, només caldrà considerar variables catagoriques.

**Hi han diferències entre els grups  BF (Biological farming), HF (Hobby farming), NM (Nature Conservation Management), and SF (Standard Farming)?**


```r
adonis(dune ~ Management, data=dune.env, permutations=99)
```

```
> 
> Call:
> adonis(formula = dune ~ Management, data = dune.env, permutations = 99) 
> 
> Permutation: free
> Number of permutations: 99
> 
> Terms added sequentially (first to last)
> 
>            Df SumsOfSqs MeanSqs F.Model      R2 Pr(>F)  
> Management  3    1.4686 0.48953  2.7672 0.34161   0.03 *
> Residuals  16    2.8304 0.17690         0.65839         
> Total      19    4.2990                 1.00000         
> ---
> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

**Hi han diferències entre 4 nivels d'humitat (`Moisture`)**


```r
adonis(dune ~ Moisture, data=dune.env, permutations=99)
```

```
> 
> Call:
> adonis(formula = dune ~ Moisture, data = dune.env, permutations = 99) 
> 
> Permutation: free
> Number of permutations: 99
> 
> Terms added sequentially (first to last)
> 
>           Df SumsOfSqs MeanSqs F.Model      R2 Pr(>F)   
> Moisture   3    1.7282 0.57606  3.5851 0.40199   0.01 **
> Residuals 16    2.5709 0.16068         0.59801          
> Total     19    4.2990                 1.00000          
> ---
> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

**Un cas en el que no hi haurien d'haver diferències**

Per veure un cas en que no hi hagi diferències entre grups, podem provar de separar la mostra en tres grups de forma de forma aleatoria.



```r
set.seed(1)
dune.env$group = sample(1:3, nrow(dune.env), replace=T)
```


```r
adonis(dune ~ group, data=dune.env, permutations=99)
```

```
> 
> Call:
> adonis(formula = dune ~ group, data = dune.env, permutations = 99) 
> 
> Permutation: free
> Number of permutations: 99
> 
> Terms added sequentially (first to last)
> 
>           Df SumsOfSqs MeanSqs F.Model      R2 Pr(>F)
> group      1    0.1643 0.16434 0.71546 0.03823   0.63
> Residuals 18    4.1347 0.22970         0.96177       
> Total     19    4.2990                 1.00000
```

 * El paquet té definides diferents distàncies entre observacions (per defecte "bray"): "manhattan", "euclidean", "canberra", "bray", "kulczynski", "jaccard", "gower", "altGower", "morisita", "horn", "mountford", "raup" , "binomial", "chao", "cao" or "mahalanobis". Veure `?vegdist` per la definició.
 * Imagino que en el cas CoDa, la més interessant serà la euclideana. En el darrer cas només caldria definir la distància euclidea.
 

```r
adonis(dune~group, data=dune.env, permutations=99, method = "euclidean")
```

```
> 
> Call:
> adonis(formula = dune ~ group, data = dune.env, permutations = 99,      method = "euclidean") 
> 
> Permutation: free
> Number of permutations: 99
> 
> Terms added sequentially (first to last)
> 
>           Df SumsOfSqs MeanSqs F.Model      R2 Pr(>F)
> group      1     52.23  52.230 0.60807 0.03268   0.89
> Residuals 18   1546.12  85.896         0.96732       
> Total     19   1598.35                 1.00000
```

 * Ja que el test es basa en distàncies, crec que el mètode no necessita de tranformació ILR. Què creus?

 * Per fer un analis MANOVA two-way, només cal afegir més covariables a la formula. Per exemple, utilitzant la variables creada alateoriament i la humitat, tenim:
 
 

```r
adonis(dune~Moisture+group, data=dune.env, permutations=99, method = "euclidean")
```

```
> 
> Call:
> adonis(formula = dune ~ Moisture + group, data = dune.env, permutations = 99,      method = "euclidean") 
> 
> Permutation: free
> Number of permutations: 99
> 
> Terms added sequentially (first to last)
> 
>           Df SumsOfSqs MeanSqs F.Model      R2 Pr(>F)   
> Moisture   3    522.24  174.08 2.49827 0.32674   0.01 **
> group      1     30.90   30.90 0.44345 0.01933   0.91   
> Residuals 15   1045.21   69.68         0.65393          
> Total     19   1598.35                 1.00000          
> ---
> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

------

## `ade4` package

Hi ha un altre paquet que es diu `ade4`. No m'he mirat la metodologia de l'article [Excoffier, L., Smouse, P.E. and Quattro, J.M. (1992)](http://www.genetics.org/content/131/2/479.abstract)


```r
library(ade4)
```

```
> 
> Attaching package: 'ade4'
> 
> The following object is masked from 'package:vegan':
> 
>     cca
```

```r
?amova
```


```r
data(humDNAm)
amovahum <- amova(humDNAm$samples, sqrt(humDNAm$distances), humDNAm$structures)
amovahum
```

```
> $call
> amova(samples = humDNAm$samples, distances = sqrt(humDNAm$distances), 
>     structures = humDNAm$structures)
> 
> $results
>                                 Df     Sum Sq    Mean Sq
> Between regions                  4  78.238115 19.5595288
> Between samples Within regions   5   9.284744  1.8569488
> Within samples                 662 316.197379  0.4776395
> Total                          671 403.720238  0.6016695
> 
> $componentsofcovariance
>                                                 Sigma          %
> Variations  Between regions                0.13380659  21.119144
> Variations  Between samples Within regions 0.02213345   3.493396
> Variations  Within samples                 0.47763955  75.387459
> Total variations                           0.63357958 100.000000
> 
> $statphi
>                           Phi
> Phi-samples-total   0.2461254
> Phi-samples-regions 0.0442870
> Phi-regions-total   0.2111914
```


**Quina forma tenen les dades de la funció?**

* _Mostra_

a data frame with haplotypes (or genotypes) as rows, populations as columns and abundance as entries


```r
str(humDNAm$samples)
```

```
> 'data.frame':	56 obs. of  10 variables:
>  $ oriental: int  32 0 1 0 2 4 0 0 2 2 ...
>  $ tharu   : int  48 0 0 0 2 5 0 0 0 23 ...
>  $ wolof   : int  23 39 0 29 0 0 2 0 0 0 ...
>  $ peul    : int  11 19 0 12 2 0 0 0 0 0 ...
>  $ pima    : int  59 0 2 0 0 0 0 0 0 0 ...
>  $ maya    : int  30 0 0 0 0 0 0 0 0 0 ...
>  $ finnish : int  87 0 2 0 0 0 0 4 0 0 ...
>  $ sicilian: int  50 3 9 0 0 0 0 0 0 0 ...
>  $ israelij: int  15 0 14 0 0 0 0 1 0 0 ...
>  $ israelia: int  22 1 1 1 0 0 0 0 0 0 ...
```

* _Distàncies_

an object of class dist computed from Euclidean distance. If distances is null, equidistances are used.


```r
str(humDNAm$distances)
```

```
> Class 'dist'  atomic [1:1540] 2 1 1 1 2 2 3 1 1 1 ...
>   ..- attr(*, "Labels")= chr [1:56] "1" "2" "6" "7" ...
>   ..- attr(*, "Size")= int 56
>   ..- attr(*, "Diag")= logi FALSE
>   ..- attr(*, "Upper")= logi FALSE
>   ..- attr(*, "call")= language as.dist(m = distance)
```

* _Structures_

a data frame containing, in the jth row and the kth column, the name of the group of level k to which the jth population belongs


```r
str(humDNAm$structures)
```

```
> 'data.frame':	10 obs. of  1 variable:
>  $ regions: Factor w/ 5 levels "africa","america",..: 3 3 1 1 2 2 4 4 5 5
```
