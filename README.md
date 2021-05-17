# R_snippets
Useful code snippets

<details><summary><b>Testing DNA indices</b></summary>

```R
library(DNABarcodes)
library(magrittr)
library(rstudioapi)
setwd(dirname(getActiveDocumentContext()$path))

# Provide a set of barcodes (from a file or manually)
table <- read.table(file = "barcodes.txt", header = TRUE, sep = "\t", stringsAsFactors = FALSE, fill = TRUE)

# select a subset to test, for instance "CELseq2"
barcodes <- table[["CELseq2"]][!is.na(table[["CELseq2"]])]
barcodes <- table[["Riboseq"]]

barcodes <- table$Riboseq_PCR_8nt %>% .[. != ""]

# Hamming distance and error tolerance
analyse.barcodes(barcodes)
```

</details>
