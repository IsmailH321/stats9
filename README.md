Analysis of variance - 
a. Perform One-way ANOVA 
b. Perform Two-way ANOVA 

> library(dplyr)
> PATH <- "https://raw.githubusercontent.com/guru99-edu/R-Programming/master/poisons.csv"
> df <- read.csv(PATH) %>%

> library(dplyr)
> PATH <- "https://raw.githubusercontent.com/guru99-edu/R-Programming/master/poisons.csv"
> df <- read.csv(PATH) %>% select(-X) %>% mutate(poison = factor(poison, ordered = TRUE))
> glimpse(df)
Rows: 48
Columns: 3
$ time   <dbl> 0.31, 0.45, 0.46, 0.43, 0.36, 0.29, 0.40, 0.23, 0.22, 0.21, 0.18, 0.23, 0.82, 1.10, 0.88, 0.72, 0.92, 0.61, 0.49, 1.24, 0.30, 0.37, 0.38, 0.29, 0.43, 0.45, 0.63, 0.76, 0.4~
$ poison <ord> 1, 1, 1, 1, 2, 2, 2, 2, 3, 3, 3, 3, 1, 1, 1, 1, 2, 2, 2, 2, 3, 3, 3, 3, 1, 1, 1, 1, 2, 2, 2, 2, 3, 3, 3, 3, 1, 1, 1, 1, 2, 2, 2, 2, 3, 3, 3, 3
$ treat  <chr> "A", "A", "A", "A", "A", "A", "A", "A", "A", "A", "A", "A", "B", "B", "B", "B", "B", "B", "B", "B", "B", "B", "B", "B", "C", "C", "C", "C", "C", "C", "C", "C", "C", "C", "~
> levels(df$poison)
[1] "1" "2" "3"
> df %>% group_by(poison) %>% summarise(count_poison = n(),mean_time = mean(time, na.rm = TRUE),sd_time = sd(time, na.rm = TRUE))
# A tibble: 3 x 4
  poison count_poison mean_time sd_time
* <ord>         <int>     <dbl>   <dbl>
1 1                16     0.618  0.209 
2 2                16     0.544  0.289 
3 3                16     0.276  0.0623

Install package ggplot2

> utils:::menuInstallPkgs()
--- Please select a CRAN mirror for use in this session ---
trying URL 'https://cloud.r-project.org/bin/windows/contrib/4.0/ggplot2_3.3.3.zip'
Content type 'application/zip' length 4070994 bytes (3.9 MB)
downloaded 3.9 MB

package ‘ggplot2’ successfully unpacked and MD5 sums checked

The downloaded binary packages are in
        C:\Users\cheta\AppData\Local\Temp\RtmpYHALdg\downloaded_packages
> library(ggplot2)
Warning message:
package ‘ggplot2’ was built under R version 4.0.4 
> ggplot(df, aes(x = poison, y = time, fill = poison)) + geom_boxplot() + geom_jitter(shape = 15, color = "steelblue",position = position_jitter(0.21)) + theme_classic()


Box Plot Graphical Output

> anova_one_way <- aov(time~poison, data = df)
> summary(anova_one_way)
            Df Sum Sq Mean Sq F value   Pr(>F)    
poison       2  1.033  0.5165   11.79 7.66e-05 ***
Residuals   45  1.972  0.0438                     
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
> TukeyHSD(anova_one_way)
  Tukey multiple comparisons of means
    95% family-wise confidence level

Fit: aov(formula = time ~ poison, data = df)

$poison
         diff        lwr         upr     p adj
2-1 -0.073125 -0.2525046  0.10625464 0.5881654
3-1 -0.341250 -0.5206296 -0.16187036 0.0000971
3-2 -0.268125 -0.4475046 -0.08874536 0.0020924


Hypothesis in two-way ANOVA test:

> anova_two_way <- aov(time~poison + treat, data = df)
> summary(anova_two_way)
            Df Sum Sq Mean Sq F value  Pr(>F)    
poison       2 1.0330  0.5165   20.64 5.7e-07 ***
treat        3 0.9212  0.3071   12.27 6.7e-06 ***
Residuals   42 1.0509  0.0250                    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
