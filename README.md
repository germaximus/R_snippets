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
