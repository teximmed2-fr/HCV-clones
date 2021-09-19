Heatmaps were generated with R Studio for Windows 10.

The following information is provided:

Versions of used Software and packages.
Links for download of software versions and packages.
R Scripts for Heatmap generation.
Excel files that were used as input data.


RStudio Version 1.2.5033 
link for download:
https://www.npackd.org/p/rstudio/1.2.5033

scource code download here:
https://github.com/rstudio/rstudio/releases/tag/v1.2.5033


R version 3.6.3 (2020-02-29)
link for download:
https://cran.r-project.org/bin/windows/base/old/3.6.3/


Install neccessary packages as listed in the scripts below.

install package ‘readxl’ version 1.3.1
https://github.com/tidyverse/readxl/releases/tag/v1.3.1

Install package ‘ComplexHeatmap’ version 2.2.0 
https://www.bioconductor.org/packages/release/bioc/html/ComplexHeatmap.html
download link: https://anaconda.org/bioconda/bioconductor-complexheatmap/2.0.0/download/noarch/bioconductor-complexheatmap-2.0.0-r36_1.tar.bz2

Install package‘uwot’ version 0.1.8
https://github.com/jlmelville/uwot/releases/tag/v0.1.8

install package‘dplyr’ version 0.8.5
https://github.com/tidyverse/dplyr

install package ‘ggplot2’ version 3.3.0
https://github.com/tidyverse/ggplot2

The following R code generates a heatmap from data contained in excel files.

R Script for heatmap with clustering and dendrograms from excel file 45 or 46-1

# libraries
library(readxl)
library(ComplexHeatmap)

# path
path<-"C:/Users/yourpath/45.xlsx"
# your path = your location / path for the input excel file

# dataframe
df<-read_excel(path)

#df to matrix
matrix<-as.matrix(df)

Heatmap(scale(df[,-c(1)]),
        column_order = NULL,
        right_annotation=rowAnnotation(type=df$type,
                                       col=list(type=c("HCV"="#D33D1D",
                                                           "Flu"="#ECE124", "Tfh"="#1AD353","Th1"="#1A6DD3")),simple_anno_size = unit(2, "mm")),
        
        column_names_gp = gpar(fontsize = 8),
        show_row_names = TRUE, 
        show_row_dend = TRUE,
        show_column_dend = FALSE,
        cluster_rows = TRUE,cluster_columns = TRUE,
        name=" ")


R Script for umap from excel file 45 or 46-1

# libraries
library(ggplot2)
library(uwot)
library(dplyr)
library(readxl)
#library(ggforce)


# path
path<-"C:/Users/yourpath/46-1.xlsx"

# dataframe
df<-read_excel(path)
df<-na.omit(df)

# seed
set.seed(666)

#umap dataframe
umap(df,n_neighbors=10,scale=TRUE)%>%
  as.data.frame()%>%
  #modify dataframe
  mutate(type=df$type)%>%
  ggplot(aes(x=V1,y=V2,color=type))+
  geom_point()+
  stat_ellipse()+
  theme_bw()+
  scale_color_manual(values = c("HCV"="#D33D1D",
                                "Flu"="#ECE124", "Tfh"="#1AD353","Th1"="#1A6DD3"))
                       