# CDO Processing

The raw data used on this project comes in grib format. This format contains the information about the dimensions and the variables. This files can be transformed into nc format, which can be readed by the xarray libreary into a dataset.

Before reading them in python, a previous processing is made, because working with the grib format is much more efficient than doing in with the dataset or dataframe. To do this processing we use the CDO (Climate Data Operator) commands on a linux subsystem terminal. 

Here we detail the processing made to the data. 

## Process

The raw data comes in grib files with the selected variables. We get 4 month interval data with hourly data with 3h resolution. 
We got files from 01-01-2021 until 31-05-2025.

First, we made the daily means in order to reduce 8 times the amount of data.

```
cdo daymean hourly_data.grib dayly_data.grib
```

With the dayly means, we merge all the grib files into one.

```
cdo mergetime dayly_data_1.grib dayly_data_2.grib ... dayly_data_n.grib dayly_data_2021-2025.grib
```

Now, there are some variables that have different spatial resolution, instad of 0.25x0.25 they have 0.5x0.5. In order to make it homogeneous we are going to make an interpolation of the values and make all the variables have the same grid.

To make this, first we obtain the grid description from a grib file with only the small grid.

```
cdo griddes file_with_small_grid.grib grid_0.25.txt
```

We need to filter now the variables with small and with big grid. Get the parameter ID of these variables and split the grib file with these two sets of variables.

```
cdo selparam,*small_grid_params dayly_data_2021-2025.grib small_grid_variables.grib
cdo selparam,*big_grid_params dayly_data_2021-2025.grib big_grid_variables.grib
```

Now we need to interpolate the big grid file.

```
cdo remapbil,grid_0.25.txt big_grid_variables.grib big_grid_variables_on_small_grid.grib
```

In order to do the merge we need to set the grid in both files so they can have the same type.

```
cdo setgrid,grid_0.25.txt small_grid_variables.grib small_grid_variables_fixed.grib 
cdo setgrid,grid_0.25.txt big_grid_variables_on_small_grid.grib big_grid_variables_on_small_grid_fixed.grib
```

Now we can merge both files.

```
cdo merge small_grid_variables_fixed.grib big_grid_variables_on_small_grid_fixed.grib dayly_data_2021-2025_merged.grib
```

At this point we can change the file format to nc.

```
cdo -f nc copy dayly_data_2021-2025_merged.grib dayly_data_2021-2025_merged.nc
```

As the data downloaded does not have a lon size divisible by 5 in order to do a griboxmean we need to make the spatial grid with a proper dimension.

```
cdo setindexbox,0,75,0,45 dayly_data_2021-2025_merged.nc dayly_data_2021-2025_merged_cut.nc 
```

And finally we can make the gridboxmean of 5 to reduce the spatial dimension

```
cdo gridboxmean,5,5 dayly_data_2021-2025_merged_cut.nc era5_2021-2025_daymean_merged_gridboxmean5x5_selected.nc
```

This file is the one that we are going to use in the 03_Data_Processing notebook.










