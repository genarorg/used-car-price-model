# What drives the price of a car?

Our client is a user car dealership that want to understand what makes a used car desirable to their customers. The company can benefit from a model that accurately estimates the sale value of a vehicle given its capabilities and characteristics. There are many factors that influence the price of a vehicle and we can use data analysis and machine learning to develop an understanding of how these factors play together. 

We will be working with a data sub-set of used car sales that contains information about the vehicle sold, along with some other information about the sale itself. We will study the data and use it to train a model that will help us answer the following questions:

1. What attributes or conditions are the main contributors to the sale price of a vehicle?
2. Given the data we have, can we predict the sale price of a car with a reasonable degree of confidence?

The detailed analysis can be found here:
1. [Business Understanding](1_business_undestanding.ipynb)
2. [Data Understanding](2_data_undestanding.ipynb)
3. [Data Preparation](3_data_preparation.ipynb)
3. [Modeling](4_modeling.ipynb)

## The Data

Upon an initial exploration of the data, we identified the following:

### Outliers

The columns `price`, `odometer` and `year` have some outliers. For price, we dropped any records where the price is **under** $1,000 USD. The remaining outliers were removed using IQR.

### Backfill and removal of unnecessary data

The following actions were taken:

* Backfill rows without `cylinders` with the average. The assumption is that this is actually an important feature, but the number of records without this is too high to remove.
* Backfill rows without `odometer` using the national average per year.
* Backfill `condition` from odometer and/or year. This is a broad assumption, but this is assumed to be an important feature so we will do out best to keep it.
* Backfill missing `manufacturer` from the `model` column. Use a dictionary of known brands.
* Backfill rows without `drive` with most popular drive.
* Backfill rows without `paint_color` with most popular color.
* Drop column `size`, since it cannot be inferred from any other column
* Drop column `VIN`, `state`, `region`.
* Drop rows without `fuel`, `transmission`, `title_status` and `year`. They have low null counts.
* Drop rows without `type`. This has a high null count, but no way to backfill.
* Drop rows withouth `manufacturer`.
* Drop the `model` column. Keeping this will produce a lot of categorical data and/or too many features to deal with.

## Initial Data Analysis

1. **Prices** and **year** are correlated. This makes sense, as newer cars are more expensive.
2. **Prices** and **odometer** are negatively correlated. This also makes sense, as cars with more miles are cheaper.
3. **Prices** and **cylinders** are not clearly correlated. I would have expected more cylinders to mean a higher price, but its not always the case.
4. **Prices** and **condition** are correlated, with newer cars being more expensive, but there is a lot of overlap.
5. **Prices** and **fuel** clearly correlated. Diesel cars are more expensive, followed by electric, gas and hybrid.
6. **Prices** and **title_status** are correlated, with clean and lien titles being more expensive.
7. **Prices** and **transmission** are correlated, with automatic cars being more expensive. However, there are lots of outlier due to missing data.
8. **Prices** and **drive** are correlated, with 4wd cars being more expensive, and almost equal to rwd cars.
9. **Prices** and **type** are correlated slightly, with pickups and trucks commanding higher prices
10. **Prices** and **paint_color** don't seem to be much correlated.
