# Component Design


**1. Reading data:**
This is necessary in order to access the information.

**2. Processing data:**
All the data processing only needs to be run once and was run by the developers. The files will then be saved in the ./futurfish/data folder. This component prepares puts the data in the correct format and prepares it for modelling future vulnerability.

**3. Determining future vulnerability projections:**
This runs a simple model to link the data uploaded and processed in steps 1 and 2. It is necessary for use cases 2 and 3 in our functional specifications.

**4. Plotting and visualizing data:**
This allows us to share the results in an interactive format. This is necessary for all use cases.

**5. User interface:** 
This is where the user interacts with the data and gets answers to their questions.

-----

### 1. Reading data:
There are three datasets to import for this project:
* Stream flow data (.csv file): this data can be found at http://hydro.washington.edu/crcc/. The variables we use from this dataset are:
	* day from 1950 through 2099
	* daily average streamflow (cubic feet per second)
	* location
* Stream flow temperatures (.csv file): this data can be found at https://www.fs.fed.us/rm/boise/AWAE/projects/NorWeST.html#downloads. The variables in this dataset that we use are:
	* location
	* date
	* daily max and min
	* daily mean and standard deviation
* Salmon species habitat location (table in paper, which needs to be manually input as a .csv file). The variables in this dataset that we use are:
	* Salmon species
	* Spawning river

This component loads the .csv files using pandas.read_csv and build a pandas dataframe (for all three datasets)

-----

### 2. Processing data:
This is necessary to get our data in the right format. For example, calculating annual averages or other basic statistics.
- This function takes the data read in in step 1 and puts it in format usable by step 3.
- Name of the fuction: process_data
- inputs: the complete timeseries output from step 1
- outputs: basic statistics conducted upon the inputs. For example, minimum streamflow calculated at sites throughout the domain within the critical periods for each salmon species.

-----

### 3. Determining future vulnerability projections:
This component runs a simple model to predict salmon populations based on forecasted stream temperature and stream flow data:
* temp_mod(), stream_mod() and stream_mod_pink(): predicts salmon vulnerability based on stream temperature and flow rates. 
	Inputs: temperature (float) or flow rate (float), and salmon species factors (float). 
	Outputs: predicted salmon vulnerability (float) on scale of 1 to 5. 
* fishmod.py: takes full set of input data and outputs a .csv file with salmon vulnerability at each location in our dataset using the functions above and giving equal weight to stream flow and temperature vulnerability values. 
	Inputs: temperature and flow rate data at each location. 
	Outputs: .csv file with all salmon species, locations, vulnerability for each time period.  

-----

### 4. Plotting and visualizing data (plotting.py): 
This is used to display the results of our analysis. This is necessary for all use
cases.

#### Function
The visualization component takes numerical and categorical data from .csv file  provided by the
analysis/projection component and generate an interactive map that will allow the user to explore
the data. 

#### Name
This component resides in the Python module that will be named `plotting`.

#### Inputs
This component takes a data object produced by the projection component as
well as some options and parameters to be given through the user interface
component.

#### Outputs
This component outputs data to be interpreted visually using plotly and mapbox libraries

#### Operation
Operation of this component is opaque to the majority of users.  Visualizations will be
generated dynamically within the user interface component by triggering callbacks when the user
changes options or interacts with the visualizations themselves.

----

### 5. User interface:
This lets the user interact with the data and get answers to their questions. It is necessary
for all use cases.

#### Function
The user interface component is the primary method that users interact with the project.  It
provides and interactive map that provides both instruction and functionality.  The interactive
dashboard-like interface consists of widgets including:

 * Text for explanation/instruction
 * Radio buttons for selecting time period to display
 * Dropdown menu for selecting fish species to display
 * Main display of interactive map

#### Name
This component resides within three different component in a Python module that will be names: 
`futurefish_dash`,`dashboard`, and `interactions`.

#### Inputs
This component takes several inputs:

 * User input from interacting with widgets (using plotly and dash)
 * Data from the plotting and visualization component (interactive map)

#### Outputs
Operation of this component results in the serving of a web server that allows the user to
view and interact with the analysis in a web browser of their choosing.

#### Operation
This component provides a single entry point to the project for outside
users.  The user can setup and launch the interface locally.

----

### Further Details
The user interface is formatted as a locally hosted web app with the following sub-components:

 * #### Driver (futurefish_dash.py):
   Script that sets up and runs the HTML server

 * #### Layout Generator (dashboard.py):
   Sets up the initial layout and manages HTML

 * #### Interaction Layer (interactions.py):
   Processes datasets based on given inputs from the interactive user interface

 * #### Plotting Layer (plotting.py):
   Processes datasets based on given inputs from interaction

<p align="center">
 <img src="https://github.com/UWSEDs-aut17/uwseds-group-nw-climate-crew/blob/master/futurefish/resources/images/comp_design_fig.png">
</p> 



