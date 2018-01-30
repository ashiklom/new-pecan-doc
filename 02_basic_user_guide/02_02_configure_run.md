### Basic model configuration

This page is used for basic model configuration, including when your model will run and what input data it will use.

#### Choosing meteorology

Once a Machine, Model, and Site have been selected, PEcAn will take you to the Input selection page. From this page you will select what Plant Functional Type (PFT) you want to run at a site, the start and end dates of the run, and various Input selections. The most common of these across all models is the need to specify meteorological forcing data. The exact name of the menu item for meteorology will vary by model because all of the Input requirements are generated individually for each model based on the MODEL_TYPE table.  In general there are 3 possible cases for meteorology

* PEcAn already has driver files in its database
* PEcAn does not have drivers, but can generate them from publicly available data
* You need (or want) to upload your own drivers

The first two cases will appear automatically in the the pull down menu. For meteorological files that already exist you will see the date range that's available. By contrast, met that can be generated will appear as "Use <source>", where <source> is the origin of the data (e.g. "Use Ameriflux" will use the micromet from an Ameriflux eddy covariance tower, if one is present at the site).

If you want to upload your own met data this can be done in three ways. The standard way is to upload data in PEcAn's standard meteorological format (netCDF files, CF metadata). See [here](../../developers_guide/Adding-an-Input-Converter.md#met-data) for details about variables and units. From this standard, PEcAn can then convert the file to the model-specific format required by the model you have chosen. This approach is preferred because PEcAn will also be able to convert the file into the format required by any other model as well. The second option for adding met data is to add it in a model-specific format, which is often easiest if you've already been running your model at a site and are just switching to using PEcAn. The final way to add met data is to incorporate it into the overall meteorological processing workflow. This is preferred if you are working with a common meteorological data product that is not yet in PEcAn's workflow. Details are [here](../../developers_guide/Adding-an-Input-Converter.md#met-data), though at this stage you would also be strongly encouraged to contact the PEcAn development team.


##### Met workflow

In a nutshell, the PEcAn met workflow is designed to reduce the problem of converting *n* possible met inputs into *m* possible model formats, which requires *n x m* conversion functions as well as numerous custom functions for downscaling, gap filling, etc. Instead, PEcAn works with a single met standard, and thus requires *n* conversion functions, one for converting each data source into the PEcAn standard, and then *m* conversion functions for converting from that standard to what an individual model requires. For a new model joining the PEcAn system the burden in particularly low -- writing one conversion function provides access to *n* inputs. Similarly, PEcAn performs all other operations/manipulations (extracting a site, downscaling, gap filling, etc) within the PEcAn standard, which means these operations only need be implemented once.

Consider a generic met data product named MET for simplicity. PEcAn will use a function, download.MET, to pull data for the selected year from a public data source (e.g. Ameriflux, North American Regional Reanalysis, etc). Next, PEcAn will use a function, met2CF.MET, to convert the data into the PEcAn standard. If the data is already at the site scale it will then gapfill the data. If the data is a regional or global data product, PEcAn will then permute the data to allow easier site-level extraction, then it will extract data for the requested site and data range. Modules to address the temporal and spatial downscaling of meteorological data products, as well as their uncertainties, are in development but not yet part of the operational workflow. All of these functions are located within the data.atmosphere module.

Once data is in the standard format and processed, it will be converted to the model-specific format using a met2model.MODEL function (located in that MODEL's module).

More detailed information on how PEcAn processes inputs can be found on our [Input Conversion] page.

#### Troubleshooting meteorological conversions

At the current moment (PEcAn 1.4.0), most of the issues below address possible errors that the Ameriflux meteorology workflow might report

##### Could not do gapfill ... The following variables have NA's

This error message means that there were gaps in the downloaded data, for whatever variables that were listed, which were larger than the current algorithm could fill. Particularly common is missing radiation or PAR data, as Ameriflux frequently converts nighttime data to NULL, and work is in progress to detect this based on solar geometry. Also common are incomplete years (first or last year of tower operations).

##### Could not get information about <site> . Is this an Ameriflux site? 

This message occurs when PEcAn believes that a site is part of Ameriflux (because it was listed on the Ameriflux or FLUXNET webpage and has a US-* site code), but no data is present on the Ameriflux server. The most common reasons for this is that you have selected a site that has not submitted data to Ameriflux yet (or that data hasn't been processed yet), or you have selected a year that's outside the tower's operational period. Visit [Ameriflux](http://ameriflux.lbl.gov/sites/site-list-and-pages/) and [FLUXNET](http://fluxnet.ornl.gov/site_status) for lists of available site years.

##### Could not download data for <site> for the year <YEAR>

This is similar to the previous error, but in this case PEcAn did find data for the site listed, but just not for the year requested. This can usually be fixed by just altering the years of the run to match those with available data.


#####  I could not find the requested var (or dimvar) in the file!

PEcAn could not find a required variable within the downloaded file. Most likely this is due to that variable not being measured at this site. The most common cause of failure is the absence of atmospheric pressure data (PRESS), but since most models have a low sensitivity to this variable we are working on methods to estimate this from other sources.

#### Choosing initial vegetation

On the Input Selection webpage, in addition to selecting PFTs, start & end dates, and meteorology, many models also require some way of specifying the initial conditions for the vegetation, which may range from setting the aboveground biomass and LAI up to detailed inventory-like data on species composition and stand structure.

At the moment, PEcAn has two cases for initial conditions and they only exist as required Inputs for the ED2 model:

If files already exist in the database, they can simply be selected from the menu. For ED2, there are 3 different veg files (site, pss, css) and it is important that you select a complete set, not mix and match.

If files don't exist they can be uploaded following the instructions on [How to insert new Input data](../developers_guide/How-to-insert-new-Input-data.html). 

Two additional options are in development:

* Model spin-up
* Automated workflows

##### Spin up

A number of ecosystem models are typically initialized by spinning up to steady state. At the moment PEcAn doesn't handle spin up automatically (e.g. looping met, checking for stability), but there are various ways to achieve a spin-up within the system. 

**Option 1:** If there are model-specific settings in a model's settings/config file, then that file can be accessed by clicking on the **Edit model config** check box. If this box is selected then PEcAn will pause the site run workflow after it has generated your model config file, but before it runs the model, and give you an opportunity to edit the file by hand, allowing you to change any model-specific spin up settings (e.g met recycling, spin up length)

**Option 2:** Set start_year very early and set the met drivers to be a long time series (e.g. PalEON, something custom uploaded to Inputs)

**Option 3:** In the MODEL_TYPE table, add your model's restart format as an optional input, modify the model specific write.config function to use that restart, and then load a previous spin-up to the Inputs table

Beyond these options, we hope to eventually develop more general, model-agnostic tools for spin up. In particular, we have started to explore the accelerated spin-up and semi-analytical techniques being developed by Yiqi Luo's lab

##### Veg workflow

As with meteorology, PEcAn is working to develop a model-agnostic workflow for converting various sources of vegetation data to common standards, developing common processing tools, and then writing out to model-specific formats. This process is in a much early stage than the meteorology workflow, as we are still researching what the options are for standard formats, but ultimately aims to be much more broad in scope, considering not just plot inventory data but also historical documentation, paleoecological proxies, satellite remote sensing (e.g. LANDSAT), airborne hyperspectral imagery, and active remote sensing (Lidar, Radar).

At the moment, what is functional is a model-specific workflow for the ED2 model that can query the USFS Forest Inventory and Analysis and then construct initial condition files for ED2. This tool works with an internal copy of the FIA that is uploaded to a postGRES database along side BETY, however for space reasons this database does not ship with the PEcAn VM. To turn this feature on:

1. [Download and Install the FIA database](../../developers_guide/Installing-PEcAn-Data.Rmd)
2. For web-base runs, specify the database settings in the [config.php](https://github.com/PecanProject/pecan/blob/master/web/config.example.php)
3. For R-based runs, specify the database settings in the [pecan.xml](../advanced_users_guide/PEcAn-Configuration.md#database-access)

More detailed information on how PEcAn processes inputs can be found on our [Input Conversion] page.

#### Selecting a soils product

Many models have requirements for soils information, which may include: site-specific soil texture and depth information; soil biogeochemical initial conditions (e.g. soil carbon and nitrogen pools); soil moisture initial conditions; and soil thermal initial conditions.

As with [[Choosing initial vegetation]], we eventually hope to develop data standards, soils workflows, and spin-up tools, but at the moment model requirements need to be met by [inserting Input data](../developers_guide/How-to-insert-new-Input-data.html) into the database or using files that have already been uploaded.

##### Soil texture, depth, and physical parameters

A PEcAn-standard netCDF file format exists for soil texture, depth, and physical parameters, using PEcAn standard names that are largely a direct extention of the CF standard. A table of standard names and units can be listed using `PEcAn.data.land::soil.units()` with no arguements.

```{r,echo=FALSE}
knitr::kable(PEcAn.data.land::soil.units())
```

More detailed information on how PEcAn processes inputs can be found on our [Input Conversion] page.

#### Other model inputs

Finally, any other model-specific inputs (e.g. N deposition, land use history, etc), should be met by [inserting Input data](../developers_guide/How-to-insert-new-Input-data.html) into the database or using files that have already been uploaded.
