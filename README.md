# Useful code snippets for sequencing data analysis   

<details><summary><b>Testing DNA indices</b></summary>

```R
library(DNABarcodes)
library(magrittr)
library(rstudioapi)
setwd(dirname(getActiveDocumentContext()$path))

# Provide a set of barcodes (from a file or manually)
table <- read.table(file = "barcodes.txt", header = TRUE, sep = "\t", stringsAsFactors = FALSE, fill = TRUE)

# select a subset to test
barcodes <- table[["Riboseq"]]

# Hamming distance and error tolerance
analyse.barcodes(barcodes)
```
</details>

<details><summary><b>REVIGO plot</b></summary>

```R
# sample data
revigo.names <- c("term_ID","description","frequency_%","plot_X","plot_Y","GO.size","log10.pvalue","uniqueness","dispensability")
revigo.data <- rbind(c("GO:0000003","reproduction",8.579,-5.509,-2.930,2.908,-3.063,1.000,0.000),
c("GO:0006412","translation",2.874,-1.503,6.329,2.435,-4.066,0.866,0.000),
c("GO:0009792","embryo development ending in birth or egg hatching",3.181,3.140,-5.933,2.479,-6.863,0.951,0.000),
c("GO:0015031","protein transport",4.878,6.092,1.887,2.664,-3.483,0.808,0.000),
c("GO:0030968","endoplasmic reticulum unfolded protein response",0.594,-1.983,-5.742,1.756,-2.696,0.930,0.008),
c("GO:0006915","apoptotic process",0.954,-5.950,3.560,1.959,-1.467,0.992,0.008),
c("GO:0006457","protein folding",1.007,-6.545,0.100,1.982,-2.322,0.992,0.008),
c("GO:0007005","mitochondrion organization",1.559,6.391,-3.303,2.170,-1.493,0.959,0.008),
c("GO:0006123","mitochondrial electron transport, cytochrome c to oxygen",0.106,-0.345,-1.477,1.041,-1.081,0.960,0.088),
c("GO:0040020","regulation of meiotic nuclear division",0.159,-3.239,0.983,1.204,-1.026,0.982,0.097),
c("GO:0052696","flavonoid glucuronidation",1.865,1.546,7.292,2.246,-1.245,0.888,0.121),
c("GO:0006898","receptor-mediated endocytosis",0.233,5.977,3.713,1.362,-1.744,0.893,0.296),
c("GO:0009408","response to heat",0.626,-2.580,-5.413,1.778,-1.161,0.956,0.404),
c("GO:1902600","proton transmembrane transport",0.817,5.266,2.381,1.892,-3.329,0.866,0.440),
c("GO:0002119","nematode larval development",3.627,2.574,-6.061,2.535,-3.401,0.951,0.450),
c("GO:0000398","mRNA splicing, via spliceosome",1.326,-2.798,5.936,2.100,-1.216,0.906,0.520),
c("GO:0019915","lipid storage",0.361,6.765,1.496,1.544,-1.142,0.847,0.524),
c("GO:0045039","protein insertion into mitochondrial inner membrane",0.074,6.173,0.180,0.903,-1.438,0.815,0.560),
c("GO:0009813","flavonoid biosynthetic process",1.865,0.490,6.972,2.246,-1.245,0.858,0.672))
# format
revigo.df <- data.frame(revigo.data, stringsAsFactors = F)
colnames(revigo.df) <- revigo.names
revigo.df <- revigo.df[(revigo.df$plot_X != "null" & revigo.df$plot_Y != "null"), ]
revigo.df[,3:9] <- lapply(revigo.df[,3:9], as.numeric)
revigo.df <- revigo.df[rev(order(revigo.df$log10.pvalue)),]
# plot
p1 <- ggplot(data = revigo.df) +
  geom_point( aes(plot_X, plot_Y, size = GO.size, fill = log10.pvalue), shape = 21, colour = "black", stroke = 0.25, alpha = 0.75, show.legend = T) +
  scale_fill_gradientn(colours = c("blue", "cyan", "green", "yellow", "red"), values = c(0,0.2,0.4,0.6,0.8,1.0), trans = 'reverse') +
  scale_size_area(max_size = 12) +
  xlim(min(revigo.df$plot_X)-(max(revigo.df$plot_X) - min(revigo.df$plot_X))/10, max(revigo.df$plot_X)+(max(revigo.df$plot_X) - min(revigo.df$plot_X))/10) +
  ylim(min(revigo.df$plot_Y)-(max(revigo.df$plot_Y) - min(revigo.df$plot_Y))/10, max(revigo.df$plot_Y)+(max(revigo.df$plot_Y) - min(revigo.df$plot_Y))/10) +
  theme_bw() +
  theme(panel.grid = element_blank(), axis.text = element_blank(), axis.title = element_blank())

ggsave(plot = p1, "./figures/revigo-plot.pdf",  device="pdf", units='in', width = 6, height = 5) 

```
</details>
