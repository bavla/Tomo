# Bibliographic data analysis

```
> wdir <- "C:/Users/batagelj/work/Tomo"
> setwd(wdir)
> AId <- factor(trimws(T$AVTOR))
> aId <- levels(AId)
> WId <- factor(trimws(T$LIST.ID))
> wId <- levels(WId)
> T <- read.csv("AI.txt",colClasses=c("character"))
> Encoding(T$ime) <- "UTF-8"
```
