# Bibliographic data analysis


`WA.txt`
```
"AVTOR", "LIST.ID"
"07083", "10337881"
"15530", "10337881"
"07083", "12005977"
"15530", "12005977"
...
```

`AI.txt`
```
"koda", "ime"
"00027", "GYERGYEK LUDVIK"
"00032", "Grad Janez"
"00035", "Hadži Dušan"
"00061", "Koroušić Blaženko"
"00142", "Vodopivec Franc"
...
```

FN.txt
```
1.00.00,Naravoslovje
1.01.00,Matematika
1.01.01,Analiza
1.01.02,Topologija
1.01.03,Numerična in računalniška matematika
1.01.04,Algebra
1.01.05,Teorija grafov
...
```

AF.txt
```
"MSTID",POD1
"00027",2.06.00
"00032",1.07.02
"00035",1.04.02
"00061",2.04.02
"00142",2.04.00
"00172",2.06.00
...
```




```
> wdir <- "C:/Users/batagelj/work/Tomo"
> setwd(wdir)
> T <- read.csv("WA.txt",colClasses=c("character"))
> AId <- factor(trimws(T$AVTOR))
> aId <- levels(AId)
> WId <- factor(trimws(T$LIST.ID))
> wId <- levels(WId)
> T <- read.csv("AI.txt",colClasses=c("character"))
> Encoding(T$ime) <- "UTF-8"
```
