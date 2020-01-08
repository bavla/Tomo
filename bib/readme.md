# Bibliographic data analysis

## Files

Tomo created the following files:

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

## Using a list as a dictionary

```
> L <- list()
> L
list()
> i <- c("a","b","c")
> v <- c(1,5,7)
> L[i] <- v
> L
$a
[1] 1

$b
[1] 5

$c
[1] 7
> j <- c("a","c")
> u <- c(8,3)
> L[j] <- u
> L
$a
[1] 8

$b
[1] 5

$c
[1] 3

>
```

## Converting the data into Pajek files

```
> wdir <- "C:/Users/batagelj/work/Tomo"
> setwd(wdir)
> library(stringr)
> source("https://raw.githubusercontent.com/bavla/Rnet/master/R/Pajek.R")

> X <- read.csv("WA.txt",colClasses=c("character"))
> AId <- factor(trimws(X$AVTOR))
> aId <- levels(AId)
> WId <- factor(trimws(X$LIST.ID))
> wId <- levels(WId)
> T <- read.csv("AI.txt",colClasses=c("character"))
> Encoding(T$ime) <- "UTF-8"

> Ime <- list()
> Ime[T$koda] <- str_to_title(trimws(T$ime))
> Anames <- as.character(Ime[aId])
> Anames[1:6]
[1] "Gyergyek Ludvik"   "Grad Janez"        "Hadži Dušan"       "Koroušić Blaženko" "Vodopivec Franc"  
[6] "Bohte Zvonimir"
> levels(AId) <- Anames
> uv2net(WId,AId,Net="WA.net",twomode=TRUE)
```
I read the net file into EmEditor in Windows-1250 encoding and saved it in UTF-8 with BOM encoding.

```
> Z <- read.csv("AF.txt",colClasses=c("character"))
> str(Z)
'data.frame':   2031 obs. of  2 variables:
 $ MSTID: chr  "00027" "00032" "00035" "00061" ...
 $ POD1 : chr  "2.06.00" "1.07.02" "1.04.02" "2.04.02" ...
> mZ0 <- substr(Z$POD1,1,1)
> mZ0[1:6]
[1] "2" "1" "1" "2" "2" "2"
> mZ1 <- substr(Z$POD1,1,4)
> mZ1[1:6]
[1] "2.06" "1.07" "1.04" "2.04" "2.04" "2.06"
> iz <- factor(Z$MSTID,levels=aId)
> iz[1:10]
 [1] 00027 00032 00035 00061 00142 00172 00206 00225 00357 00365
4947 Levels: 00027 00032 00035 00061 00142 00158 00172 00204 00206 00213 00219 00225 00357 00365 ... A99389027
> length(iz)
[1] 2031
> mz0 <- factor(mZ0)
> mz0[1:10]
 [1] 2 1 1 2 2 2 1 N 4 3
Levels: 1 2 3 4 5 6 7 N
```




```
> ANa <- list()
> ANa[aId] <- 0
> length(ANa)
[1] 4947
> iz <- factor(Z$MSTID,levels=aId)
> mZ0 <- substr(Z$POD1,1,1)
> mz0 <- factor(mZ0)
> mz0[1:9]
[1] 2 1 1 2 2 2 1 N 4
Levels: 1 2 3 4 5 6 7 N
> jz0 <- as.integer(mz0)
> jz0[1:9]
[1] 2 1 1 2 2 2 1 8 4
> iz[1:9]
[1] 00027 00032 00035 00061 00142 00172 00206 00225 00357
4947 Levels: 00027 00032 00035 00061 00142 00158 00172 00204 00206 00213 00219 00225 00357 00365 ... A99389027
> jz <- as.integer(iz)
> jz[1:9]
[1]  1  2  3  4  5  7  9 12 13
```

Problem
```
> ANa[jz] <- jz0
Error in ANa[jz] <- jz0 : NAs are not allowed in subscripted assignments
> which(is.na(jz))
  [1] 1462 1463 1465 1467 1469 1470 1474 1477 1478 1480 1487 1492 1494 1499 1500 1504 1505 1507 1509 1515 1519
 [22] 1524 1530 1534 1537 1545 1548 1554 1561 1567 1576 1582 1584 1593 1597 1598 1601 1605 1609 1615 1622 1623
 [43] 1628 1630 1636 1638 1649 1653 1654 1656 1658 1674 1679 1681 1682 1683 1687 1701 1706 1708 1709 1714 1722
 [64] 1728 1731 1732 1736 1737 1739 1740 1743 1747 1756 1774 1778 1781 1786 1792 1793 1796 1799 1801 1804 1806
 [85] 1808 1822 1828 1834 1835 1839 1843 1845 1847 1849 1851 1855 1862 1866 1872 1874 1875 1877 1878 1879 1885
[106] 1889 1890 1897 1903 1904 1906 1909 1912 1915 1919 1927 1932 1935 1937 1942 1943 1949 1956 1964 1965 1967
[127] 1971 1974 1976 1977 1978 1979 1986 1987 1988 1994 1998 2017 2019 2031
> iz[1460:1468]
[1] 92561 15530 <NA>  <NA>  12066 <NA>  36412 <NA>  34602
4947 Levels: 00027 00032 00035 00061 00142 00158 00172 00204 00206 00213 00219 00225 00357 00365 ... A99389027
> ina <- which(is.na(jz))
> Z$MSTID[ina]
  [1] "05685" "51120" "36752" "26242" "51879" "01588" "06556" "51210" "50009" "28106" "34676" "39561" "13789"
 [14] "38511" "50629" "39820" "53056" "39519" "32236" "30821" "52672" "39367" "39237" "29948" "13843" "27781"
 [27] "40461" "53163" "14607" "36908" "37581" "52892" "13854" "23390" "13844" "36889" "20977" "33506" "50140"
 [40] "39334" "50668" "50456" "00823" "52528" "19320" "14367" "03019" "39106" "37764" "16202" "33823" "33577"
 [53] "16085" "52808" "36720" "03832" "34679" "31738" "03435" "00726" "30781" "51980" "25001" "37774" "34563"
 [66] "03025" "30835" "36547" "51094" "19548" "50815" "35838" "37235" "51122" "51123" "31196" "35304" "11777"
 [79] "50721" "29014" "32247" "12229" "21465" "16127" "39538" "51213" "34628" "51964" "50816" "52806" "50673"
 [92] "11641" "51878" "37257" "35918" "31353" "52804" "24058" "09859" "36225" "28914" "52701" "30388" "51979"
[105] "52490" "37128" "01082" "36551" "53165" "51376" "51126" "28222" "37808" "30101" "23906" "19601" "26173"
[118] "37964" "15519" "38859" "08495" "51477" "34799" "38510" "51377" "21814" "21120" "52334" "36937" "50720"
[131] "00727" "28636" "40449" "52453" "34173" "28294" "51877" "38860" "52810" "28723"
```
In the file AF.txt there are some authors that do not appear in WA.txt.

```
> nina <- which(!is.na(jz))
> jza <- jz[nina]
> jz0a <- jz0[nina]
> ANa[jza] <- jz0a
> C0 <- as.integer(ANa)
> C0[1:20]
 [1] 2 1 1 2 2 1 2 1 1 1 1 8 4 3 3 1 3 3 2 1
> vector2clu(C0,"C0.clu") 

> mz1 <- factor(mZ1)
> mz1[1:10]
 [1] 2.06 1.07 1.04 2.04 2.04 2.06 1.03 NA.N 4.03 3.06
68 Levels: 1.00 1.01 1.02 1.03 1.04 1.05 1.06 1.07 1.08 1.09 2.01 2.02 2.03 2.04 2.05 2.06 2.07 2.08 ... NA.N
> jz1 <- as.integer(mz1)
> jz1[1:20]
 [1] 16  8  5 14 14 16  4 68 45 38 36  3 36 37 11 17 20 37  4 44
> jz1a <- jz1[nina]
> ANa[jza] <- jz1a
> C1 <- as.integer(ANa)
> C1[1:20]
 [1] 16  8  5 14 14  2 16  2  4  2  2 68 45 38 36  3 36 37 11  2
> vector2clu(C1,"C1.clu")

> mz2 <- factor(Z$POD1)
> jz2 <- as.integer(mz2)
> jz2a <- jz2[nina]
> ANa[jza] <- jz2a
> C2 <- as.integer(ANa)
> C2[1:20]
 [1]  62  34  24  56  54   5  62   2  18   5   2 179 133 116 114  10 114 115  41   4
> vector2clu(C2,"C2.clu")

> t2 <- table(C2)
> t2o <- rev(sort(t2))
> t2o[1:20]
C2
   0  111    7    2  114  116  113    3  179    6  118   73  156  110    5    4  115  158    8   28 
3056   77   71   67   65   64   62   61   58   55   52   51   43   43   41   37   35   34   34   32 
> L0 <- levels(mz0)
> L0
[1] "1" "2" "3" "4" "5" "6" "7" "N"
> for(i in 1:length(L0)) cat(i,'-"',L0[i],'"; ',sep=""); cat("\n") 
1-"1"; 2-"2"; 3-"3"; 4-"4"; 5-"5"; 6-"6"; 7-"7"; 8-"N"; 
> L1 <- levels(mz1)
> for(i in 1:length(L1)) cat(i,'-"',L1[i],'"; ',sep=""); cat("\n") 
1-"1.00"; 2-"1.01"; 3-"1.02"; 4-"1.03"; 5-"1.04"; ... ; 67-"7.02"; 68-"NA.N"; 
> L2 <- levels(mz2)
> for(i in 1:length(L2)) cat(i,'-"',L2[i],'"; ',sep=""); cat("\n") 
1-"1.00.00"; 2-"1.01.00"; 3-"1.01.01"; 4-"1.01.02"; 5-"1.01.03"; ... ; 178-"7.02.00"; 179-"NA.NA.NA"; 
```
 
## Converting the data into Pajek files / essential code

```
> wdir <- "C:/Users/batagelj/work/Tomo"
> setwd(wdir)
> library(stringr)
> source("https://raw.githubusercontent.com/bavla/Rnet/master/R/Pajek.R")

> X <- read.csv("WA.txt",colClasses=c("character"))
> AId <- factor(trimws(X$AVTOR))
> aId <- levels(AId)
> WId <- factor(trimws(X$LIST.ID))
> wId <- levels(WId)
> T <- read.csv("AI.txt",colClasses=c("character"))
> Encoding(T$ime) <- "UTF-8"

> Ime <- list()
> Ime[T$koda] <- str_to_title(trimws(T$ime))
> Anames <- as.character(Ime[aId])
> levels(AId) <- Anames
> uv2net(WId,AId,Net="WA.net",twomode=TRUE)

> Z <- read.csv("AF.txt",colClasses=c("character"))
> mZ0 <- substr(Z$POD1,1,1)
> mZ1 <- substr(Z$POD1,1,4)
> iz <- factor(Z$MSTID,levels=aId)

> ANa <- list()
> ANa[aId] <- 0
> mz0 <- factor(mZ0)
> jz0 <- as.integer(mz0)
> jz <- as.integer(iz)

> nina <- which(!is.na(jz))
> jza <- jz[nina]
> jz0a <- jz0[nina]
> ANa[jza] <- jz0a
> C0 <- as.integer(ANa)
> vector2clu(C0,"C0.clu") 

> mz1 <- factor(mZ1)
> jz1 <- as.integer(mz1)
> jz1a <- jz1[nina]
> ANa[jza] <- jz1a
> C1 <- as.integer(ANa)
> vector2clu(C1,"C1.clu")

> mz2 <- factor(Z$POD1)
> jz2 <- as.integer(mz2)
> jz2a <- jz2[nina]
> ANa[jza] <- jz2a
> C2 <- as.integer(ANa)
> vector2clu(C2,"C2.clu")

> t2o <- rev(sort(table(C2)))
> t2o[1:20]
C2
   0  111    7    2  114  116  113    3  179    6  118   73  156  110    5    4  115  158    8   28 
3056   77   71   67   65   64   62   61   58   55   52   51   43   43   41   37   35   34   34   32 
> L0 <- levels(mz0)
> for(i in 1:length(L0)) cat(i,'-"',L0[i],'"; ',sep=""); cat("\n") 
1-"1"; 2-"2"; 3-"3"; 4-"4"; 5-"5"; 6-"6"; 7-"7"; 8-"N"; 
> L1 <- levels(mz1)
> for(i in 1:length(L1)) cat(i,'-"',L1[i],'"; ',sep=""); cat("\n") 
1-"1.00"; 2-"1.01"; 3-"1.02"; 4-"1.03"; 5-"1.04"; ... ; 67-"7.02"; 68-"NA.N"; 
> L2 <- levels(mz2)
> for(i in 1:length(L2)) cat(i,'-"',L2[i],'"; ',sep=""); cat("\n") 
1-"1.00.00"; 2-"1.01.00"; 3-"1.01.01"; 4-"1.01.02"; 5-"1.01.03"; ... ; 178-"7.02.00"; 179-"NA.NA.NA"; 
```
 


