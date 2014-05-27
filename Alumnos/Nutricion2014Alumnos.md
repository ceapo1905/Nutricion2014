# Alumnos - Nutrición 2014
## Descripción
Aquí presentamos los resultados de las encuestas de `evaluación institucional` llevada a cabo en mayo del 2014 en la que se recabaron datos de los alumnos de la carrera de `Nutrición` de la Facultad de Medicina (Sede Asunción) de la Universidad del Norte. La encuesta consistió en un formulario de `91 preguntas` evaluando cada una de las siguientes dimensiones:
* Dimensión 1: Organización & Gestión
    * 1.1: Organización
    * 1.2: Gestión
* Dimensión 2: Proyecto Académico
    * 2.1: Objetivos de carrerra y Perfil de Egreso
    * 2.2: Plan de estudios
    * 2.3: Proceso enseñanza-aprendizaje
    * 2.4: Evaluación del proceso enseñanza-aprendizaje
    * 2.5: Investigación y extensión
* Dimensión 3: Personas
    * 3.1: Directivos
    * 3.2: Docentes
    * 3.3: Estudiantes
    * 3.4: Personal administrativo y de apoyo
* Dimensión 4: Recursos
    * 4.1: Infraestructura, equipamientos e insumos

El formulario de encuesta también incluyó preguntas acerca del `Año` (1º, 2º, 3º, 4º) en curso y el `Turno` (mañana, tarde, noche) del estudiante. Se obtuvieron `282` formularios de respuestas, los cuales fueron cargados en una base de datos en formato `.csv` para su análisis estadístico. Los datos fueron preprocesados y analizados con [R](http://www.r-project.org) versión 3.1.0. Este documento fue redactado utilizando MarkDown en [RStudio](http://www.rstudio.com) versión 0.98.501 y el paquete [knitr](http://cran.r-project.org/web/packages/knitr/index.html) versión 1.6 de [Yihui Xie](http://yihui.name/knitr).

## Preprocesamiento de datos
Primero cargamos la base de datos:

```r
data.alumnos <- read.csv("../Central - Nutrición - Alumnos - Dimensión 1.csv")
```

Excluimos la pregunta Q2.25.k ("En caso afirmativo, indicar ...") por no corresponder al diseño apropiado:

```r
data.alumnos <- data.alumnos[, -54]
```

Convertimos todos los valores perdidos en `4`, basándonos en la interpretación de que si el alumno no completa la pregunta es porque no sabe la respuesta o no tiene una opinión al respecto:

```r
data.alumnos[is.na(data.alumnos)] <- 4
```

Recodificamos todos los niveles para todas las variables que representan preguntas, utilizando las etiquetas establecidas en el formulario:

```r
for (i in 4:93) {
    data.alumnos[, i] <- factor(data.alumnos[, i], labels = c(`1` = "Sí", `2` = "No", 
        `3` = "En parte", `4` = "Sin opinión/No sabe"))
}
```

Arreglamos el nombre de las columnas para hacerlas más legibles y solucionamos errores en la denominación de las columnas:

```r
columnas <- colnames(data.alumnos)
columnas <- gsub("X1", "Q", x = columnas)
columnas <- gsub("X2", "Q", x = columnas)
columnas <- gsub("X3", "Q", x = columnas)
columnas <- gsub("X4", "Q", x = columnas)
columnas <- gsub(".", "", x = columnas, fixed = TRUE)
columnas <- gsub("Q23f1", "Q25f", x = columnas, fixed = TRUE)
columnas <- gsub("Año", "Curso", x = columnas, fixed = TRUE)
colnames(data.alumnos) <- c(columnas)
```

Reordenamos los niveles del `Año` y `Turno` en el orden creciente correcto:

```r
data.alumnos$Curso <- factor(data.alumnos$Curso, levels(data.alumnos$Curso)[c(2:4, 
    1)])
data.alumnos$Turno <- factor(data.alumnos$Turno, levels(data.alumnos$Turno)[c(1, 
    3, 2)])
```

Cargamos las preguntas contenidas en el formulario:

```r
source("../AlumnosQ.R")
```

```
## Warning: cannot open file '../AlumnosQ.R': No such file or directory
```

```
## Error: cannot open the connection
```



## Resultados
Para la presentación de los resultados utilizaremos gráficos de barras, según la siguiente función:

```r
Q.plots <- function(x, y) {
    table.Q <- prop.table(table(x)) * 100
    par(mar = c(2, 5, 7, 2) + 0.1)
    plot.Q <- barplot(table.Q, main = y, cex.main = 2, col = terrain.colors(9), 
        ylab = "% Alumnos", cex.lab = 1.5, cex.axis = 1.2, cex.names = 1.5)
    text(plot.Q, 2, round(table.Q, 1), cex = 1.5)
}
```

Las distribuciones por `Año` en curso y `Turno` fueron las siguientes:

```r
Q.plots(data.alumnos$Curso, Curso)
```

```
## Error: object 'Curso' not found
```

![plot of chunk Cursos](figure/Cursos.png) 



```r
Q.plots(data.alumnos$Turno, Turno)
```

```
## Error: object 'Turno' not found
```

![plot of chunk Turnos](figure/Turnos.png) 


A continuación presentarmos los resultados de las respuestas dadas por los estudiantes.

***
### Dimensión 1.1 - Organización
Los resultados de esta dimensión son los siguientes:

```r
Q.plots(data.alumnos$Q11a, Q11a)
```

```
## Error: object 'Q11a' not found
```

![plot of chunk Q11](figure/Q111.png) 

```r
Q.plots(data.alumnos$Q11b, Q11b)
```

```
## Error: object 'Q11b' not found
```

![plot of chunk Q11](figure/Q112.png) 

```r
Q.plots(data.alumnos$Q11c, Q11c)
```

```
## Error: object 'Q11c' not found
```

![plot of chunk Q11](figure/Q113.png) 

```r
Q.plots(data.alumnos$Q11d, Q11d)
```

```
## Error: object 'Q11d' not found
```

![plot of chunk Q11](figure/Q114.png) 

```r
Q.plots(data.alumnos$Q11e, Q11e)
```

```
## Error: object 'Q11e' not found
```

![plot of chunk Q11](figure/Q115.png) 

```r
Q.plots(data.alumnos$Q11f, Q11f)
```

```
## Error: object 'Q11f' not found
```

![plot of chunk Q11](figure/Q116.png) 

```r
Q.plots(data.alumnos$Q11g, Q11g)
```

```
## Error: object 'Q11g' not found
```

![plot of chunk Q11](figure/Q117.png) 

```r
Q.plots(data.alumnos$Q11h, Q11h)
```

```
## Error: object 'Q11h' not found
```

![plot of chunk Q11](figure/Q118.png) 

***
### Dimensión 1.2 - Gestión
Los resultados de esta dimensión son los siguientes:

```r
Q.plots(data.alumnos$Q12a, Q12a)
```

```
## Error: object 'Q12a' not found
```

![plot of chunk Q12](figure/Q121.png) 

```r
Q.plots(data.alumnos$Q12b, Q12b)
```

```
## Error: object 'Q12b' not found
```

![plot of chunk Q12](figure/Q122.png) 

```r
Q.plots(data.alumnos$Q12c, Q12c)
```

```
## Error: object 'Q12c' not found
```

![plot of chunk Q12](figure/Q123.png) 

```r
Q.plots(data.alumnos$Q12d, Q12d)
```

```
## Error: object 'Q12d' not found
```

![plot of chunk Q12](figure/Q124.png) 

```r
Q.plots(data.alumnos$Q12e, Q12e)
```

```
## Error: object 'Q12e' not found
```

![plot of chunk Q12](figure/Q125.png) 

```r
Q.plots(data.alumnos$Q12f, Q12f)
```

```
## Error: object 'Q12f' not found
```

![plot of chunk Q12](figure/Q126.png) 

```r
Q.plots(data.alumnos$Q12g, Q12g)
```

```
## Error: object 'Q12g' not found
```

![plot of chunk Q12](figure/Q127.png) 

```r
Q.plots(data.alumnos$Q12h, Q12h)
```

```
## Error: object 'Q12h' not found
```

![plot of chunk Q12](figure/Q128.png) 

```r
Q.plots(data.alumnos$Q12i, Q12i)
```

```
## Error: object 'Q12i' not found
```

![plot of chunk Q12](figure/Q129.png) 

***
### Dimensión 2.1 - Objetivos de la carrera y Perfil de Egreso
Los resultados de esta dimensión son los siguientes:

```r
Q.plots(data.alumnos$Q21a, Q21a)
```

```
## Error: object 'Q21a' not found
```

![plot of chunk Q21](figure/Q211.png) 

```r
Q.plots(data.alumnos$Q21b, Q21b)
```

```
## Error: object 'Q21b' not found
```

![plot of chunk Q21](figure/Q212.png) 

```r
Q.plots(data.alumnos$Q21c, Q21c)
```

```
## Error: object 'Q21c' not found
```

![plot of chunk Q21](figure/Q213.png) 

```r
Q.plots(data.alumnos$Q21d, Q21d)
```

```
## Error: object 'Q21d' not found
```

![plot of chunk Q21](figure/Q214.png) 

```r
Q.plots(data.alumnos$Q21e, Q21e)
```

```
## Error: object 'Q21e' not found
```

![plot of chunk Q21](figure/Q215.png) 

***
### Dimensión 2.2 - Plan de Estudios
Los resultados de esta dimensión son los siguientes:

```r
Q.plots(data.alumnos$Q22a, Q22a)
```

```
## Error: object 'Q22a' not found
```

![plot of chunk Q22](figure/Q221.png) 

```r
Q.plots(data.alumnos$Q22b, Q22b)
```

```
## Error: object 'Q22b' not found
```

![plot of chunk Q22](figure/Q222.png) 

```r
Q.plots(data.alumnos$Q22c, Q22c)
```

```
## Error: object 'Q22c' not found
```

![plot of chunk Q22](figure/Q223.png) 

```r
Q.plots(data.alumnos$Q22d, Q22d)
```

```
## Error: object 'Q22d' not found
```

![plot of chunk Q22](figure/Q224.png) 

```r
Q.plots(data.alumnos$Q22e, Q22e)
```

```
## Error: object 'Q22e' not found
```

![plot of chunk Q22](figure/Q225.png) 

```r
Q.plots(data.alumnos$Q22f, Q22f)
```

```
## Error: object 'Q22f' not found
```

![plot of chunk Q22](figure/Q226.png) 

***
### Dimensión 2.3 - Proceso enseñanza-aprendizaje
Los resultados de esta dimensión son los siguientes:

```r
Q.plots(data.alumnos$Q23a, Q23a)
```

```
## Error: object 'Q23a' not found
```

![plot of chunk Q23](figure/Q231.png) 

```r
Q.plots(data.alumnos$Q23b, Q23b)
```

```
## Error: object 'Q23b' not found
```

![plot of chunk Q23](figure/Q232.png) 

```r
Q.plots(data.alumnos$Q23c, Q23c)
```

```
## Error: object 'Q23c' not found
```

![plot of chunk Q23](figure/Q233.png) 

```r
Q.plots(data.alumnos$Q23d, Q23d)
```

```
## Error: object 'Q23d' not found
```

![plot of chunk Q23](figure/Q234.png) 

```r
Q.plots(data.alumnos$Q23e, Q23e)
```

```
## Error: object 'Q23e' not found
```

![plot of chunk Q23](figure/Q235.png) 

```r
Q.plots(data.alumnos$Q23f, Q23f)
```

```
## Error: object 'Q23f' not found
```

![plot of chunk Q23](figure/Q236.png) 

```r
Q.plots(data.alumnos$Q23g, Q23g)
```

```
## Error: object 'Q23g' not found
```

![plot of chunk Q23](figure/Q237.png) 

```r
Q.plots(data.alumnos$Q23h, Q23h)
```

```
## Error: object 'Q23h' not found
```

![plot of chunk Q23](figure/Q238.png) 

```r
Q.plots(data.alumnos$Q23i, Q23i)
```

```
## Error: object 'Q23i' not found
```

![plot of chunk Q23](figure/Q239.png) 

***
### Dimensión 2.4 - Evaluación del proceso enseñanza-aprendizaje
Los resultados de esta dimensión son los siguientes:

```r
Q.plots(data.alumnos$Q24a, Q24a)
```

```
## Error: object 'Q24a' not found
```

![plot of chunk Q24](figure/Q241.png) 

```r
Q.plots(data.alumnos$Q24b, Q24b)
```

```
## Error: object 'Q24b' not found
```

![plot of chunk Q24](figure/Q242.png) 

```r
Q.plots(data.alumnos$Q24c, Q24c)
```

```
## Error: object 'Q24c' not found
```

![plot of chunk Q24](figure/Q243.png) 

***
### Dimensión 2.5 - Investigación & extensión
Los resultados de esta dimensión son los siguientes:

```r
Q.plots(data.alumnos$Q25a, Q25a)
```

```
## Error: object 'Q25a' not found
```

![plot of chunk Q25](figure/Q251.png) 

```r
Q.plots(data.alumnos$Q25b, Q25b)
```

```
## Error: object 'Q25b' not found
```

![plot of chunk Q25](figure/Q252.png) 

```r
Q.plots(data.alumnos$Q25c, Q25c)
```

```
## Error: object 'Q25c' not found
```

![plot of chunk Q25](figure/Q253.png) 

```r
Q.plots(data.alumnos$Q25d, Q25d)
```

```
## Error: object 'Q25d' not found
```

![plot of chunk Q25](figure/Q254.png) 

```r
Q.plots(data.alumnos$Q25e, Q25e)
```

```
## Error: object 'Q25e' not found
```

![plot of chunk Q25](figure/Q255.png) 

```r
Q.plots(data.alumnos$Q25f, Q25f)
```

```
## Error: object 'Q25f' not found
```

![plot of chunk Q25](figure/Q256.png) 

```r
Q.plots(data.alumnos$Q25g, Q25g)
```

```
## Error: object 'Q25g' not found
```

![plot of chunk Q25](figure/Q257.png) 

```r
Q.plots(data.alumnos$Q25h, Q25h)
```

```
## Error: object 'Q25h' not found
```

![plot of chunk Q25](figure/Q258.png) 

```r
Q.plots(data.alumnos$Q25i, Q25i)
```

```
## Error: object 'Q25i' not found
```

![plot of chunk Q25](figure/Q259.png) 

```r
Q.plots(data.alumnos$Q25j, Q25j)
```

```
## Error: object 'Q25j' not found
```

![plot of chunk Q25](figure/Q2510.png) 

***
### Dimensión 3.1 - Directivos
Los resultados de esta dimensión son los siguientes:

```r
Q.plots(data.alumnos$Q31a, Q31a)
```

```
## Error: object 'Q31a' not found
```

![plot of chunk Q31](figure/Q311.png) 

```r
Q.plots(data.alumnos$Q31b, Q31b)
```

```
## Error: object 'Q31b' not found
```

![plot of chunk Q31](figure/Q312.png) 

```r
Q.plots(data.alumnos$Q31c, Q31c)
```

```
## Error: object 'Q31c' not found
```

![plot of chunk Q31](figure/Q313.png) 

```r
Q.plots(data.alumnos$Q31d, Q31d)
```

```
## Error: object 'Q31d' not found
```

![plot of chunk Q31](figure/Q314.png) 

***
### Dimensión 3.2 - Docentes
Los resultados de esta dimensión son los siguientes:

```r
Q.plots(data.alumnos$Q32a, Q32a)
```

```
## Error: object 'Q32a' not found
```

![plot of chunk Q32](figure/Q321.png) 

```r
Q.plots(data.alumnos$Q32b, Q32b)
```

```
## Error: object 'Q32b' not found
```

![plot of chunk Q32](figure/Q322.png) 

```r
Q.plots(data.alumnos$Q32c, Q32c)
```

```
## Error: object 'Q32c' not found
```

![plot of chunk Q32](figure/Q323.png) 

***
### Dimensión 3.3 - Estudiantes
Los resultados de esta dimensión son los siguientes:

```r
Q.plots(data.alumnos$Q33a, Q33a)
```

```
## Error: object 'Q33a' not found
```

![plot of chunk Q33](figure/Q331.png) 

```r
Q.plots(data.alumnos$Q33b, Q33b)
```

```
## Error: object 'Q33b' not found
```

![plot of chunk Q33](figure/Q332.png) 

```r
Q.plots(data.alumnos$Q33c, Q33c)
```

```
## Error: object 'Q33c' not found
```

![plot of chunk Q33](figure/Q333.png) 

```r
Q.plots(data.alumnos$Q33d, Q33d)
```

```
## Error: object 'Q33d' not found
```

![plot of chunk Q33](figure/Q334.png) 

```r
Q.plots(data.alumnos$Q33e, Q33e)
```

```
## Error: object 'Q33e' not found
```

![plot of chunk Q33](figure/Q335.png) 

```r
Q.plots(data.alumnos$Q33f, Q33f)
```

```
## Error: object 'Q33f' not found
```

![plot of chunk Q33](figure/Q336.png) 

***
### Dimensión 3.4 - Personal administrativo y de apoyo
Los resultados de esta dimensión son los siguientes:

```r
Q.plots(data.alumnos$Q34a, Q34a)
```

```
## Error: object 'Q34a' not found
```

![plot of chunk Q34](figure/Q341.png) 

```r
Q.plots(data.alumnos$Q34b, Q34b)
```

```
## Error: object 'Q34b' not found
```

![plot of chunk Q34](figure/Q342.png) 

```r
Q.plots(data.alumnos$Q34c, Q34c)
```

```
## Error: object 'Q34c' not found
```

![plot of chunk Q34](figure/Q343.png) 

```r
Q.plots(data.alumnos$Q34d, Q34d)
```

```
## Error: object 'Q34d' not found
```

![plot of chunk Q34](figure/Q344.png) 

***
### Dimensión 4.1 - Infraestructura, equipamientos e insumos
Los resultados de esta dimensión son los siguientes:

```r
Q.plots(data.alumnos$Q41a, Q41a)
```

```
## Error: object 'Q41a' not found
```

![plot of chunk Q41](figure/Q411.png) 

```r
Q.plots(data.alumnos$Q41b, Q41b)
```

```
## Error: object 'Q41b' not found
```

![plot of chunk Q41](figure/Q412.png) 

```r
Q.plots(data.alumnos$Q41c, Q41c)
```

```
## Error: object 'Q41c' not found
```

![plot of chunk Q41](figure/Q413.png) 

```r
Q.plots(data.alumnos$Q41d, Q41d)
```

```
## Error: object 'Q41d' not found
```

![plot of chunk Q41](figure/Q414.png) 

```r
Q.plots(data.alumnos$Q41e, Q41e)
```

```
## Error: object 'Q41e' not found
```

![plot of chunk Q41](figure/Q415.png) 

```r
Q.plots(data.alumnos$Q41f, Q41f)
```

```
## Error: object 'Q41f' not found
```

![plot of chunk Q41](figure/Q416.png) 

```r
Q.plots(data.alumnos$Q41g, Q41g)
```

```
## Error: object 'Q41g' not found
```

![plot of chunk Q41](figure/Q417.png) 

```r
Q.plots(data.alumnos$Q41h, Q41h)
```

```
## Error: object 'Q41h' not found
```

![plot of chunk Q41](figure/Q418.png) 

```r
Q.plots(data.alumnos$Q41i, Q41i)
```

```
## Error: object 'Q41i' not found
```

![plot of chunk Q41](figure/Q419.png) 

```r
Q.plots(data.alumnos$Q41j, Q41j)
```

```
## Error: object 'Q41j' not found
```

![plot of chunk Q41](figure/Q4110.png) 

```r
Q.plots(data.alumnos$Q41k, Q41k)
```

```
## Error: object 'Q41k' not found
```

![plot of chunk Q41](figure/Q4111.png) 

```r
Q.plots(data.alumnos$Q41l, Q41l)
```

```
## Error: object 'Q41l' not found
```

![plot of chunk Q41](figure/Q4112.png) 

```r
Q.plots(data.alumnos$Q41m, Q41m)
```

```
## Error: object 'Q41m' not found
```

![plot of chunk Q41](figure/Q4113.png) 

```r
Q.plots(data.alumnos$Q41n, Q41n)
```

```
## Error: object 'Q41n' not found
```

![plot of chunk Q41](figure/Q4114.png) 

```r
Q.plots(data.alumnos$Q41o, Q41o)
```

```
## Error: object 'Q41o' not found
```

![plot of chunk Q41](figure/Q4115.png) 

```r
Q.plots(data.alumnos$Q41p, Q41p)
```

```
## Error: object 'Q41p' not found
```

![plot of chunk Q41](figure/Q4116.png) 

```r
Q.plots(data.alumnos$Q41q, Q41q)
```

```
## Error: object 'Q41q' not found
```

![plot of chunk Q41](figure/Q4117.png) 

```r
Q.plots(data.alumnos$Q41r, Q41r)
```

```
## Error: object 'Q41r' not found
```

![plot of chunk Q41](figure/Q4118.png) 

```r
Q.plots(data.alumnos$Q41s, Q41s)
```

```
## Error: object 'Q41s' not found
```

![plot of chunk Q41](figure/Q4119.png) 

```r
Q.plots(data.alumnos$Q41t, Q41t)
```

```
## Error: object 'Q41t' not found
```

![plot of chunk Q41](figure/Q4120.png) 

```r
Q.plots(data.alumnos$Q41u, Q41u)
```

```
## Error: object 'Q41u' not found
```

![plot of chunk Q41](figure/Q4121.png) 

```r
Q.plots(data.alumnos$Q41v, Q41v)
```

```
## Error: object 'Q41v' not found
```

![plot of chunk Q41](figure/Q4122.png) 

```r
Q.plots(data.alumnos$Q41w, Q41w)
```

```
## Error: object 'Q41w' not found
```

![plot of chunk Q41](figure/Q4123.png) 

