# Practica-1

# PRIMERA PREGUNTA

Las bases de datos que presentaremos a continuación se establecerán en torno a la educación en Perú: los colegios de Lima Metropolitana que ingresarán a la modalidad semipresencial, todos los colegios de esta región, los índices de educación en Latinoamérica, y los funcionarios encargados de la educación. 

*Primera base de datos*

Nuestra primera base de datos, que denominaremos "colsemipresencial", se extraerá con scraping en función "htmltab". Esta base se refiere a los colegios en Lima Metropolitana que realizarán una modalidad semi presencial. Las variables que contiene, en orden, son las siguientes: nombre del colegio, distrito en el que se encuentra, y a qué gestión pertenece.

```{r}
library(htmltab)
link1 = "https://gestion.pe/peru/minedu-como-puedo-saber-si-en-el-colegio-de-mi-hijo-tambien-habra-clases-semipresenciales-minedu-clases-semipresenciales-colegios-autorizados-lima-nnda-nnlt-noticia/"
path1 = "/html/body/div[1]/div[6]/div[2]/section/div[2]/div[3]/div[2]/section/table"
colsemi = htmltab(link1, path1)
library(rio)
export(colsemi, "colsemipresencial.csv") 
```

*Segunda base de datos*

La segunda base de datos, denominada "collima", también se extraerá en función "htmltab". Como se precisó, esta contendrá los datos de los colegios que existen en Lima Metropolitana. Sus variables son cuatro: nombre del colegio, año de creación, qué género admite en sus aulas, y el distrito en el que se encuentra.

```{r}
link2 = "https://es.wikipedia.org/wiki/Anexo:Colegios_del_Per%C3%BA"
path2 = "/html/body/div[3]/div[3]/div[5]/div[1]/table[6]"
colegios = htmltab(link2, path2)
colegios$`InformaciÃ³n general >> #`=NULL
colegios$`InformaciÃ³n general >> AfiliaciÃ³n religiosa`=NULL
names(colegios)
names(colegios)=c("Colegio", "Año de creación", "Género", "Distrito")
export(colegios, "collima.csv") 
```

*Tercera base de datos*

La tercera base de datos, igualmente en función "html", se denomina "indlatino". Se refiere a los índices de educación de los países en Latinoamérica y contiene ocho variables. En orden, se refieren a lo siguiente: 1) país latinoamericano; 2) promedio de educación por años; 3) expectativa de educación por años; 4) alfabatización (% mayor a 15 años); 5) primaria completa; 6) secundaria completa; 7) terciaria completa; y 8) terciaria en desarrollo.

```{r}
link3 = "https://es.wikipedia.org/wiki/Anexo:Indicadores_de_educaci%C3%B3n_en_Latinoam%C3%A9rica"
path = "/html/body/div[3]/div[3]/div[5]/div[1]/table[1]"
indice = htmltab(link3, path3)
names(indice)
names(indice)=c("pais", "prom_edu", "exp_edu", "alfab", "prim_com", "sec_com", "ter_com", "ter_desa")
export(indice, "indlatino.csv")
```

*Cuarta base de datos*

La última base de datos tendrá la función "rvest". En este caso, contiene información sobre los funcionarios del Ministerio de Educación, ente encargado de la educaión en el Perú. Esta base de datos está compuesta por dos variables -nombre y cargo del funcionario- y se denomina "minedu".

```{r}
url="https://www.gob.pe/institucion/minedu/funcionarios"
minpub=read_html(url)

css_nombre="h3.h.is-6.link.link--read-more-inline.is-bold"
nombre_html <- html_nodes(minpub,css_nombre)
nombre_texto <- html_text(nombre_html)
head(nombre_texto)

css_cargo="h3.font-light.mb-6"
cargo_html <- html_nodes(minpub,css_cargo)
cargo_texto <- html_text(cargo_html)
head(cargo_texto)

data1 <- data.frame(NOMBRE = nombre_texto, CARGO = cargo_texto)

export(data1, "minedu.csv")
```
