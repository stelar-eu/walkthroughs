<style>
r { color: Red }
o { color: Orange }
g { color: Green }
</style>

Food safety teams have long sat on mountains of incident data-from fresh-produce contamination spikes to repeat hazards in packaged goods. Today, the game is changing: instead of just reporting what happened, we're using time-series forecasting on product-hazard data to predict, and prevent the next outbreak. Think smarter inspection schedules, targeted interventions, and resources deployed exactly when and where they're needed. What was once a dusty archive becomes a real time crystal ball for prevention and preparedness.

> **Note**: Ensure the `stelar_client` library is installed via:
> ```python
>   pip install stelar_client --upgrade
> ```
> **Documentation**: <a href="https://stelar-client.readthedocs.io/en/latest/" target="_blank">ReadTheDocs</a>

## Dataset Selection
Browse to the <a name="button" class="btn btn-primary btn-sm " href="/stelar/console/v1/catalog" target="_blank">Data Catalog</a> and search for a dataset to use. 




In the left-hand panel, you may find facets to filter results with.


1. Filter By Tags - Check for **`food_incidents`** or **`food safety`**.

![Food Safety Forecasting]()

2. Filter By Organization - Find datasets provided by **`Agroknow`**

3. Select a dataset of your liking

<r>*Feel free to adjust or combine additional facets if you would like to narrow the list further.*</r>

You may use any of the following datasets for this task.

1. <a href="/stelar/console/v1/catalog/food-safety-incidents-products-and-hazards" target="_blank">Food Incidents, Products & Hazards</a> - Time series for food incidents
2. 
