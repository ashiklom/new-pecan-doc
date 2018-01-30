# Introduction

**Ecosystem science, policy, and management informed by the best available data and models**

```{r, echo=FALSE,out.height= "90%", out.width="90%"}
  knitr::include_graphics(rep("figures/PecanLogo.png"))
```

**Our Mission:**


**Develop and promote accessible tools for reproducible ecosystem modeling and forecasting**


[PEcAn Website](http://pecanproject.github.io/)

[Public Chat Room](https://gitter.im/PecanProject/pecan)

[Github Repository](https://github.com/PecanProject/pecan)

## PEcAn in a nutshell

PEcAn provides an interface to a variety of ecosystem models and attempts to standardize and automate the processes of model parameterization, execution, and analysis. First, you choose an ecosystem model, then the time and location of interest (a site), the plant community (or crop) that you are interested in simulating, and a source of atmospheric data from the BETY database (LeBauer et al, 2010). These are set in a "settings" file, commonly named `pecan.xml` which can be edited manually if desired. From here, PEcAn will take over and set up and execute the selected model using your settings. The key is that PEcAn uses models as-is, and all of the translation steps are done within PEcAn so no modifications are required of the model itself. Once the model is finished it will allow you to create graphs with the results of the simulation as well as download the results. It is also possible to see all past experiments and simulations.
