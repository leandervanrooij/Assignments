
# Graded Assignment 2
Leander van Rooij u846850

# Packages

# Assignment 1

First load the `tidyverse` package:


```R
library(tidyverse)
```

##  Assignment 1a

Read the data file _graded_assignment_2.csv_ from disk:


```R
data1 <- read.csv2("_graded_assignment2.csv")
head(data1)
```


<table>
<thead><tr><th scope=col>ID</th><th scope=col>Group</th><th scope=col>FKG</th><th scope=col>DKG</th><th scope=col>Gender</th><th scope=col>Elderly</th><th scope=col>Age</th></tr></thead>
<tbody>
	<tr><td>1     </td><td>17    </td><td>0     </td><td>0     </td><td>male  </td><td>65+   </td><td>80-84 </td></tr>
	<tr><td>2     </td><td>34    </td><td>0     </td><td>0     </td><td>female</td><td>65+   </td><td>65-69 </td></tr>
	<tr><td>3     </td><td>31    </td><td>1     </td><td>0     </td><td>female</td><td>65-   </td><td>50-54 </td></tr>
	<tr><td>4     </td><td>22    </td><td>1     </td><td>0     </td><td>female</td><td>65-   </td><td>5-sep </td></tr>
	<tr><td>5     </td><td>35    </td><td>0     </td><td>0     </td><td>female</td><td>65+   </td><td>70-74 </td></tr>
	<tr><td>6     </td><td>19    </td><td>1     </td><td>1     </td><td>male  </td><td>65+   </td><td>90-94 </td></tr>
</tbody>
</table>



## Assignment 1b

Add column "Health_status" with containing values _1_ (Healthy) and _0_ (Unhealthy).
A person is Healthy, when `FKG` equals 0 and `DKG` equals 0.

So

| FKG | DKG | Health_status   |
|-----|-----|--------------   |
| 0   | 0   | 1               |
| 1   | 0   | 0               |
| 0   | 1   | 0               |
| 1   | 1   | 0               |

Hint: you can use `ifelse()`:


```R
data2 <- data1 
data2$Health_status<-ifelse(data2$FKG +data2$DKG == 0,1,0)
head(data2)
  
```


<table>
<thead><tr><th scope=col>ID</th><th scope=col>Group</th><th scope=col>FKG</th><th scope=col>DKG</th><th scope=col>Gender</th><th scope=col>Elderly</th><th scope=col>Age</th><th scope=col>Health_status</th></tr></thead>
<tbody>
	<tr><td>1     </td><td>17    </td><td>0     </td><td>0     </td><td>male  </td><td>65+   </td><td>80-84 </td><td>1     </td></tr>
	<tr><td>2     </td><td>34    </td><td>0     </td><td>0     </td><td>female</td><td>65+   </td><td>65-69 </td><td>1     </td></tr>
	<tr><td>3     </td><td>31    </td><td>1     </td><td>0     </td><td>female</td><td>65-   </td><td>50-54 </td><td>0     </td></tr>
	<tr><td>4     </td><td>22    </td><td>1     </td><td>0     </td><td>female</td><td>65-   </td><td>5-sep </td><td>0     </td></tr>
	<tr><td>5     </td><td>35    </td><td>0     </td><td>0     </td><td>female</td><td>65+   </td><td>70-74 </td><td>1     </td></tr>
	<tr><td>6     </td><td>19    </td><td>1     </td><td>1     </td><td>male  </td><td>65+   </td><td>90-94 </td><td>0     </td></tr>
</tbody>
</table>



In the next datacamp course you will learn more about the package `dplyr`. For now we just give you some code. You can run the following script.

First, we want to make "Health_status" a factor instead of a character


```R
data2 <- data2 %>%
  mutate(Health_status = as.numeric(Health_status))
str(data2)
```

    'data.frame':	10000 obs. of  8 variables:
     $ ID           : int  1 2 3 4 5 6 7 8 9 10 ...
     $ Group        : int  17 34 31 22 35 19 1 30 29 8 ...
     $ FKG          : int  0 0 1 1 0 1 0 1 0 1 ...
     $ DKG          : int  0 0 0 0 0 1 1 1 0 1 ...
     $ Gender       : Factor w/ 2 levels "female","male": 2 1 1 1 1 2 2 1 1 2 ...
     $ Elderly      : Factor w/ 2 levels "65+","65-": 1 1 2 2 1 1 2 2 2 2 ...
     $ Age          : Factor w/ 20 levels "0-4","15-19",..: 16 13 10 9 14 18 1 8 7 6 ...
     $ Health_status: num  1 1 0 0 1 0 0 0 1 0 ...


Then, we want to count the number of healthy and unhealthy males and females


```R
data3 <- data2 %>%
  group_by(Gender, Health_status) %>%
 summarise(Count_observations = n()) %>%
  mutate(Health_status=as.factor(Health_status))

data3
```


<table>
<thead><tr><th scope=col>Gender</th><th scope=col>Health_status</th><th scope=col>Count_observations</th></tr></thead>
<tbody>
	<tr><td>female</td><td>0     </td><td>3723  </td></tr>
	<tr><td>female</td><td>1     </td><td>1258  </td></tr>
	<tr><td>male  </td><td>0     </td><td>3734  </td></tr>
	<tr><td>male  </td><td>1     </td><td>1285  </td></tr>
</tbody>
</table>



## Assignment 2

Recreate with the dataframe "data3" the following barchart ![](../Sourcedata/barchart.png)

* Hint: see for changing the [legends and colors](http://www.cookbook-r.com/Graphs/Legends_(ggplot2)/)
* Hint: the "green" color is in fact the color "greenyellow"
* Hint: see for the [themes](http://ggplot2.tidyverse.org/reference/ggtheme.html): 




```R
ggplot(data3,aes(x = Gender, y= Count_observations, fill = Health_status)) +
geom_bar(position = "dodge", stat = "identity") +

scale_fill_manual(values=c("red","greenyellow"))
```




![png](output_11_1.png)


End notebook
