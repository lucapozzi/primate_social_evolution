## Bayes Traits Model Selection

In this document I describe the basic code used to identify the model best supported by the data using the reversible-jump (RJ) procedure in the software ["BayesTraits"](http://www.evolution.rdg.ac.uk/BayesTraitsV3.0.1/BayesTraitsV3.0.1.html).

Load require packages
```r
require(phytools)
require(geiger)
require(btw)

#Define the path where to find BayesTraits
.BayesTraitsPath <- "~/bin/BayesTraitsV2"
```
Load input files
```r
#load tree file
tree <- read.nexus("input_tree.tree")

#load matrix social system primates
data_input <- read.table("input_social_systems.txt",row.names = 1)
```

Commands to run a Reversible jump hyper-prior model analyses. Priors can be changed by using the command "rj" or "rjhp". We tested multiple combinations of priors as fully described in the manuscript. The function "Discrete" is used to run Multistate analyses when data are categorical traits.
```r
# Priors reverse jump for model selection: “rjhp gamma 0 10 0 10”
multirj <- Discrete(tree, primate.socsyst, "Bayesian", rjhp = "gamma 0 10 0 10", bi = 2500000, it = 10000000, sa = 1000)

# Priors reverse jump for model selection: rj = "uniform -100 100" 
multirj <- Discrete(tree, primate.socsyst, "Bayesian", rj = "uniform -100 100", bi = 2500000, it = 10000000, sa = 1000)

# Priors reverse jump for model selection: rjhp = "exp 0 100"
multirj <- Discrete(tree, primate.socsyst, "Bayesian", rjhp = "exp 0 100", bi = 2500000, it = 10000000, sa = 1000)

# Priors reverse jump for model selection: rjhp = "exp 0 10"
multirj <- Discrete(tree, primate.socsyst, "Bayesian", rjhp = "exp 0 10", bi = 2500000, it = 10000000, sa = 1000)

# Priors reverse jump for model selection: rjhp = "exp 0 1"
multirj <- Discrete(tree, primate.socsyst, "Bayesian", rjhp = "exp 0 10", bi = 2500000, it = 10000000, sa = 1000)
```

The function rjmodels can be used to get a quick summary of the models sampled in the reversible jump analysis.
```r
rjout <- rjmodels(multirj)
```
