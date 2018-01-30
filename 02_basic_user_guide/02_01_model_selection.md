### Site and model selection

This page is used to select the model to run and the site at which you would like to run that model.

**NOTE:** If this page does not load for you, it may be related to a known Google Maps API key issue. See [issue #1269][issue-1269] for a possible solution.

#### Selecting a model

1. On the **Select Host** webpage use the Host pull-down menu to select the server you want to run on. Note that at the moment only the LOCAL server will run through the web-based workflow.

2. Next, select the model you want to run under the Model pull-down menu

3. If the model you want to run is not listed login to BETY and navigate to Runs > Models.

4. If there are already entries for the model you want to run then most likely the PEcAn modules for that model have been implemented but PEcAn is unaware that the model has been installed on your server. Information on adding a model executable to the PEcAn database can be found [here](../../developers_guide/Adding-an-Ecosystem-Model.md#model). After this is done, your model should appear on the PEcAn **Select Host** page after your refresh the page.

5. If there a no entries for the model you want to run then most likely the PEcAn modules need to be implemented. See the instructions at Adding an Ecosystem Model

6. If selecting your model causes your site to disappear from the Google Map then the site exists but there are no drivers for that site registered in the database. See the instruction [here](Choose-a-site.md#my-site-shows-up-when-i-dont-have-any-model-selected-but-disappears-once-i-select-the-model-i-want-to-run) for more info on diagnosing what drivers are missing.


[issue-1269]: https://github.com/PecanProject/pecan/issues/1269

#### Selecting a site

1. **Find the site on the map** The simplest way of determining if a site exists in PEcAn is through the Google Map interface of the web-based workflow. You'll want to make sure that the Host is set to All Sites and the Model is set to All Models. 

2. **Find the site in BETY** If the site is not on the map there's a chance that it still in PEcAn but just lacking geographic information. To begin you'll want to login to your local version of the BETY database. If you're on the PEcAn VM this will be at localhost:3280/bety or localhost:6480/bety depending on whether you downloaded the 32 or 64 bit version.  Within BETY navigate to Data > Sites and use the Search window on the page to try and locate your site. If you **DO** find your site you will want to click Edit and add geographic information so that the site will show up on the map. It is also worth noting that the site ID number shows up in the URL for the Show or Edit pages. This ID is frequently useful to know, for example if you have to set up a PEcAn settings file by hand. If you did not find you site you will want to follow the instructions below for adding a site

Note that it's also possible to select a new site from this interface by right clicking (see "Adding a new site" -- TODO: link).
