# fun-data-wrangling
# RDD Visualization Script

Creates a Scalable Vector Graphic (SVG) demonstrating a Regression Discontinuity Design (RDD) with a clear discontinuity at x=0.

## Dependencies
- ggplot2
- svglite

## Usage
```r
#### Creating a Scalable Vector Graphic just for fun ##
#         Author: Kasturi 
#   Date created: 31/10/2025

library(ggplot2)

set.seed(7)
n <- 300
x <- runif(n, -1, 1)

# Define a true discontinuity at 0
y <- ifelse(x < 0,
            -0.2 + 0.1*x,       # left side: gentle slope, low intercept
            0.5 + 0.6*x)        # right side: higher intercept & steeper slope
y <- y + rnorm(n, 0, 0.1)       # add noise

df <- data.frame(x, y)

p <- ggplot(df, aes(x, y)) +
  geom_point(size = 1.2, color = "gray40") +
  geom_smooth(
    data = df[df$x < 0, ],
    method = "lm", formula = y ~ x, se = FALSE,
    fullrange = FALSE,
    color = "mediumpurple4",
    linewidth = 1
  ) +
  geom_smooth(
    data = df[df$x >= 0, ],
    method = "lm", formula = y ~ x, se = FALSE,
    fullrange = FALSE,
    color = "mediumpurple4",
    linewidth = 1
  ) +
  geom_vline(xintercept = 0, linetype = "dashed") +
  labs(
    x = "Running Variable",
    y = "Outcome"
  ) +
  theme_bw(base_size = 14) +
  theme(
    panel.grid.major = element_blank(),
    panel.grid.minor = element_blank()
  )

p   # shows in Plots pane

# Save as SVG
ggsave("rdd_example.svg", p, width = 8, height = 5.2, units = "in",
       device = svglite::svglite)
```


**Author:** Kasturi Kandalam  
**Date:** 31/10/2025
