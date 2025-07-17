# Narrative
*Sentinel‑2’s twin polar‑orbiting satellites deliver 10–60 m multispectral imagery every five days, capturing 13 spectral bands from visible to shortwave infrared. In northern Austria, outdated field boundaries hinder irrigation planning, yield forecasting and subsidy management. Field Segmentation ingests B2, B3, B4 and B8 reflectance bands to output field polygons—homogeneous agricultural parcels visible in imagery. These polygons mask images for pixel extraction, supporting crop classification, yield prediction  and cloud‑removal via data imputation. By reducing time series and aggregating pixel values per field, segmentation boosts quality and efficiency across STELAR use cases.*


# Scenario Overview

In the following scenario, we will utilize the **Field Segmentation** tool to segment crop fields in northern Austria using Sentinel-2 imagery. 

### Involved Tools from STELAR Toolkit

-  <a class="btn btn-outline-primary" href="/stelar/console/v1/tool/field-segmentation" target="_blank">Field Segmentation</a> - A tool that segments crop fields over Sentinel-2 imagery. It outputs field polygons, which can be used for various agricultural applications such as crop classification and yield prediction.


### Involed Features from STELAR KLMS Platform
1. **Data Catalog**: Browse and select datasets relevant to food safety incidents.
2. **Workflow Monitoring**: Monitor the execution of workflows and tasks.
3. **Lineage Tracking**: Track the lineage of artifacts to understand data flow and transformations.
4. **Data Analysis**: Perform data analysis using Python and Jupyter Notebooks.


### Walkthrough Steps Overview

<div style="text-align: center;">
    Dataset Selection --> Tool Execution --> Execution Monitoring --> Output Artifacts Tracking
</div>

### Data Pipeline Overview

<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/stelar-eu/walkthroughs/refs/heads/main/Scenario%204%20-%20Segmenting%20Crop%20Fields%20for%20the%20Austrian%20North/images/field_segmentation.drawio.png" alt="Data Pipeline Overview" style="width:700px;">
    <p><strong>Figure 1</strong>: Overview of the data pipeline for segmenting crop fields</p>
<br>

---

# Dataset Selection
Browse to the <a name="button" class="btn btn-primary btn-pill py-1 px-3" href="/stelar/console/v1/catalog" target="_blank">Data Catalog</a> and search for a dataset to use. 

<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/stelar-eu/walkthroughs/refs/heads/main/Scenario%201%20-%20Forecasting%20Food%20Safety%20Incidents/images/catalog.png" alt="Data Catalog" style="width:650px;">
    <p><strong>Figure 1</strong>: Screenshot of the Data Catalog interface.</p>
</div>
  
In the left-hand panel, you may find facets to filter results with.


- **Filter By Tags** - Check for **`Pesticides`** or **`Precision Farming`**.
<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/stelar-eu/walkthroughs/refs/heads/main/Scenario%201%20-%20Forecasting%20Food%20Safety%20Incidents/images/tags.png" alt="Data Catalog" style="width:250px;">
    <p><strong>Figure 2</strong>: Tags Facet option in the Data Catalog.</p>
</div>

- **Filter By Organization** - Find datasets provided by **`ABACO Group`**
<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/stelar-eu/walkthroughs/refs/heads/main/Scenario%201%20-%20Forecasting%20Food%20Safety%20Incidents/images/orgs.png" alt="Data Catalog" style="width:250px;">
    <p><strong>Figure 3</strong>: Owner Organization Facet option in the Data Catalog.</p>
</div>

- Select a dataset of your liking. **Compare datasets** by clicking the icon next to the dataset name to add it the **comparison list**.
<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/stelar-eu/walkthroughs/refs/heads/main/Scenario%201%20-%20Forecasting%20Food%20Safety%20Incidents/images/compare.png" alt="Compare Datasets" style="height: 190px;"> 
</div>

<br>

**We suggest using any of the following datasets for this scenario.**

1. <a href="/stelar/console/v1/catalog/pesticides-input-dataset" target="_blank">Pesticides Input Data</a> - A dataset containing a database of pesticides from the Italian Ministry of Health, including information on active ingredients, toxicity, and environmental impact. Additionally it features a file annotating the pesticides used by the farmer for this indicative scenario.

---


# Tool Execution 

> **Note**: Ensure the **STELAR Python SDK Client** is installed via:
> ```
>   pip install stelar_client --upgrade
> ```
>
> **Documentation**: <a href="https://stelar-client.readthedocs.io/en/latest/" target="_blank">ReadTheDocs</a>



Download the Python Note by clicking the button below, Open it using any *Python IDE* or *Jupyter Notebook editor*.

<div style="text-align: center;">
    <a name="button" class="btn btn-primary btn-blue btn-pill py-1 px-3" href="https://github.com/stelar-eu/walkthroughs/blob/main/Scenario%204%20-%20Segmenting%20Crop%20Fields%20for%20the%20Austrian%20North/scenario4.ipynb" target="_blank" download>
    <svg  xmlns="http://www.w3.org/2000/svg"  width="24"  height="24"  viewBox="0 0 24 24"  fill="none"  stroke="currentColor"  stroke-width="2"  stroke-linecap="round"  stroke-linejoin="round"  style="margin-right: 10px;"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><path d="M12 9h-7a2 2 0 0 0 -2 2v4a2 2 0 0 0 2 2h3" /><path d="M12 15h7a2 2 0 0 0 2 -2v-4a2 2 0 0 0 -2 -2h-3" /><path d="M8 9v-4a2 2 0 0 1 2 -2h4a2 2 0 0 1 2 2v5a2 2 0 0 1 -2 2h-4a2 2 0 0 0 -2 2v5a2 2 0 0 0 2 2h4a2 2 0 0 0 2 -2v-4" /><path d="M11 6l0 .01" /><path d="M13 18l0 .01" /></svg>
    Download Python Notebook</a>  
</div>

## Notebook Overview

The notebook contains the following sections:

1.**Import Libraries**: Import necessary libraries such as the `stelar_client` for accessing STELAR API and `time` for handling events and delays.
```python
from stelar.client import Client, Dataset, Process, TaskSpec
import time
```

2.**Initialize STELAR Client**: Set up a STELAR client. Replace your credentials in the client initialization.
```python
client = Client(
    base_url="https://sandbox.stelar.gr",
    username="your_username",
    password="your_password"
)
```

3.**Select Dataset(s)**: The notebook offers a block containing declarations for instatiating local variables for the dataset(s) you selected from the Data Catalog. 
```python
dataset_n = c.datasets["pesticides-input-dataset"]
print(f"Selected dataset: {dataset_n.id} | {dataset_n.title}")
```


4.**Start a new Workflow Process**: Create a new workflow process to execute the task.
```python
process = client.processes.create(
    name="process-jsmith-pesticide-selection",
    title="Pesticide Selection Process for John Smith",
    organization = c.organizations["stelar-klms"],
)
```

5.**Define the Task Specification**: Create a task specification for the tool.

A **STELAR Task** is a dedicated execution unit for a tool, which is always defined within the context of a **workflow process**. The task specification includes the tool to be used, the inputs required, the outputs expected, and any parameters that need to be set.


<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/stelar-eu/walkthroughs/refs/heads/main/Scenario%201%20-%20Forecasting%20Food%20Safety%20Incidents/images/task.png" alt="Task Context" style="width:650px;">
    <p><strong>Figure 4</strong>: STELAR Task abstract illustarion (Receives Inputs & Parameters, produces Outputs & Metrics)</p>
</div>
  
<br>

STELAR Client **abstracts the the process** of defining a **task specification**. The provided `TaskSpec` class allows you to **define the task**, its inputs, outputs, and parameters in a structured way.

```python
t = TaskSpec(tool="tool-abc")

# Define inputs
t.i(
    file_a = str(dataset_n.resources[0].id),  # Use the first resource of the dataset
    file_b = str(dataset_n.resources[1].id),  # Use the second resource of the dataset
)

# Define task-local aliases for datasets
t.d(alias="d0", dataset=dataset_n)  # Use the dataset object directly

# Define outputs
t.o(
    output_file =  {
        # Specify the output file name and location
        "url": "s3://klms-bucket/experiments/pesticides/matched_pesticide_products.csv",
        # Specify the destination dataset for this resource
        "dataset": "d0"
        # Specify the resource metadata to be published after task completion
        "resource":{
            "name": "Matched Pesticide Products",
            "format": "xlsx",
            "description": "Matched pesticide products based on user requirements and pesticide database.",
        }
    }
)
t.p(
    threshold = 0.8,  # Example parameter for the task
    iterations = 10
)
```


6.**Execute the Task**: Execute the task using the STELAR client within a workflow process.

```python
process = c.processes["process-jsmith-pesticide-selection"]
# The task is invoked in the process.
task_obj = process.run(t)
```

---

# Execution Monitoring
Once the **task starts executing**, you can **monitor** its progress and status using the **STELAR UI Console**.

1.**Browse** over to the <a name="button" class="btn btn-primary btn-pill py-1 px-3" href="/stelar/console/v1/processes" target="_blank">Processes</a> page and find your workflow process by its name or ID. If you used the provided notebook, the process title should be something like  `process-jsmith-food-safety-forecasting`.
<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/stelar-eu/walkthroughs/refs/heads/main/Scenario%201%20-%20Forecasting%20Food%20Safety%20Incidents/images/process.png" alt="Workflow Process" style="width:700px;">
    <p><strong>Figure 5</strong>: Screenshot of the workflow process page, offering access to tasks, comparison and graph generation features</p>
</div>
  

2.**Open the Task** which is last executed in the process. The task name depends on the goal of the task, but it should be something like `Pesticide Matching for jsmith`.


3.**Monitor the Task Status**: The task status will be displayed in the task details page. You can see if the task is running, completed, or failed. The page also provides information for 

- **Inputs**: Input artifacts used by the task.
- **Parameters**: Parameters used for the task execution.
- **Outputs**: Output artifacts produced by the task.
- **Metrics**: Metrics generated by the task.
- **Real-time Logs**: View the real-time logs of the task execution as it runs within the STELAR platform.


<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/stelar-eu/walkthroughs/refs/heads/main/Scenario%201%20-%20Forecasting%20Food%20Safety%20Incidents/images/task_page.png" alt="Task Details Page" style="width:700px;">
    <p><strong>Figure 6</strong>: Screenshot of the task monitoring page, offering access to task metadata and real time logs</p>
</div>

---

# Output Artifacts Tracking
Once the task is **completed succesfully**, you may track the output artifacts generated by the task and the provenenance of them.

**If you used the provided notebook**, the output artifacts **will be stored in the dataset you created** for  storing the results.

Browse to the <a name="button" class="btn btn-primary btn-pill py-1 px-3" href="/stelar/console/v1/catalog" target="_blank">Data Catalog</a> and find the **dataset you created containing your username in the title**. Inside the dataset, you will find its resources. By clicking on the 'Action' button you **track the lineage of the resource**, which will show you the roadmap of the resource, including the task that generated it.s


<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/stelar-eu/walkthroughs/refs/heads/main/Scenario%201%20-%20Forecasting%20Food%20Safety%20Incidents/images/tracking.png" alt="Resource Lineage Tracking" style="width:700px;">
    <p><strong>Figure 7</strong>: Resources registered under a dataset. Lineage tracking is possible on a per resource basis.</p>
</div>

By clicking on the **resource name** you have the option to **preview the resource** or **download it**. While we opted to support the majority of common file formats, you may find that some special format or quite larger files may not be supported for preview.

<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/stelar-eu/walkthroughs/refs/heads/main/Scenario%201%20-%20Forecasting%20Food%20Safety%20Incidents/images/resource_preview.png" alt="Resource Preview" style="width:700px;">
    <p><strong>Figure 8</strong>: Resource preview for a tabular CSV resource.</p>
</div>