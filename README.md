# E-commerce Data Warehouse and BI Solution

A business intelligence solution built on top of an e-commerce star-schema data warehouse (EDW). It includes an SSAS multidimensional cube, OLAP analysis in Excel, and interactive Power BI reports.

Completed for **Data Warehousing and Business Intelligence (IT3021) – Assignment 2**, Sri Lanka Institute of Information Technology (SLIIT), 2026. The data warehouse itself and its SSIS ETL were built in Assignment 1.

## Repository contents

| Path | Description |
| --- | --- |
| [`ssas/EcommerceCubeProject.slnx`](ssas/EcommerceCubeProject.slnx) | Visual Studio solution for the SSAS multidimensional project |
| [`excel/`](excel/) | Excel workbooks connected to the cube, used for the OLAP operation demos |
| [`power-bi/DWBI_Assignment_2.pbix`](power-bi/DWBI_Assignment_2.pbix) | Power BI report with the four required report pages |
| [`docs/IT23274648_DWBI_Assignment_2.pdf`](docs/IT23274648_DWBI_Assignment_2.pdf) | Full assignment report with step-by-step screenshots |

> **Note:** only the solution file is committed under `ssas/`. The project it references (`ssas/EcommerceCubeProject/EcommerceCubeProject.dwproj`, with the `.ds`, `.dsv`, `.cube` and `.dim` files) is not in the repository yet.

## Tools and technologies

- **SQL Server / SSMS** – hosts the EDW database
- **SSIS** – ETL from the source data into the star schema (Assignment 1)
- **SSDT (Visual Studio)** – SSAS project development
- **SSAS (Multidimensional)** – the OLAP cube
- **Microsoft Excel** – PivotTable analysis over the cube
- **Power BI Desktop** – interactive reports

## Data warehouse

The source is an e-commerce dataset of users, orders, order items, inventory items, products and distribution centers. For the ETL, the original single Excel dataset was split into three different source types. SSIS then loaded them into a star schema.

**Fact table: `FactOrders`** (one row per order item)

| Measure | Description |
| --- | --- |
| `SalePrice` | Sale value of the order item |
| `Cost` | Product cost |
| `Profit` | Sale price minus cost |
| `txn_process_time_hours` | Hours between transaction creation and completion (accumulating snapshot) |

**Dimension tables**

| Dimension | Contents |
| --- | --- |
| `DimUser` | Customer details: name, email, age, gender, address, city, state, country, traffic source. Tracked as **SCD Type 2** (`StartDate`, `EndDate`, `IsCurrent`) |
| `DimProduct` | Product name, category, department, brand, retail price, cost |
| `DimDistributionCenter` | Distribution center name and coordinates |
| `DimDate` | Full date, day, month, month name, quarter, year, day of week |

```
                 DimDistributionCenter
                          |
   DimDate ----------- FactOrders ----------- DimUser
                          |
                      DimProduct
```

## SSAS cube

- **Data source:** connects to the `EDW` SQL Server database using the service account.
- **Data source view:** includes all five tables, with relationships detected from the foreign keys.
- **Cube:** built with the Cube Wizard on the `Fact Orders` measure group. Measures are Sale Price, Cost, Profit, Txn Process Time Hours and Fact Orders Count.
- **Dimension attributes:**
  - Dim User: User Key, First Name, Gender, Country
  - Dim Product: Id, Pname, Category, Brand, Retail Price, Cost
  - Dim Distribution Center: Distribution Center Key, Name
  - Dim Date: Date Key, Full Date, Day, Month Name, Quarter, Year
- **Hierarchies:**
  - Product: `Category → Product Name`
  - Date: `Year → Quarter → Month Name → Day`
- **Deployment:** deployed and processed to a local SSAS instance as the `EcommerceCubeProject` database.

## OLAP operations (Excel)

Excel connects to the cube through **Data → Get Data → From Database → From Analysis Services**, using Windows Authentication. The workbooks in [`excel/`](excel/) demonstrate these operations:

| Operation | What it shows |
| --- | --- |
| Roll-up / Drill-down | Expanding and collapsing the Date hierarchy (Year ↔ Quarter ↔ Month ↔ Day) |
| Slice | Filtering on a single dimension, e.g. one product |
| Dice | Filtering on several dimensions at once with slicers, e.g. Product + Country + Year |
| Pivot | Swapping the Product and Date hierarchies between rows and columns |

## Power BI reports

The report imports the fact and dimension tables from the EDW SQL Server database.

1. **Matrix:** "Sales by Product and Year", with Total Sales and Order Count per product per year, plus subtotals and grand totals.
2. **Cascading slicers:** Category and Country/City slicers driving a map, Total Sales by category and by country charts, and KPI cards for Total Sales, Order Count and Profit.
3. **Drill-down:** a column chart of Total Sales that drills from Year → Quarter → Month → Day.
4. **Drill-through:** a category summary page (bar chart and matrix) that drills through to a detailed product-level table.

**Headline figures** (2019–2024): about **10.83M** total sales, **5.71M** profit and **182K** order items.

## How to open

**Prerequisites:** SQL Server with the `EDW` database restored, SQL Server Analysis Services in Multidimensional mode, Visual Studio with SSDT (the Analysis Services Projects extension), Excel and Power BI Desktop.

- **Cube:** open `ssas/EcommerceCubeProject.slnx` in Visual Studio. Point the data source at your SQL Server instance, set the deployment server under *Project → Properties → Deployment*, then deploy and process. This needs the `.dwproj` project files (see the note above).
- **Excel:** open a workbook in `excel/`, then update the connection under *Data → Queries & Connections* to your SSAS server and refresh.
- **Power BI:** open `power-bi/DWBI_Assignment_2.pbix`, go to *Transform data → Data source settings*, change the source to your SQL Server instance, then refresh.

## Documentation

The full write-up, including the ER diagram, step-by-step screenshots and explanations of each report, is in [`docs/IT23274648_DWBI_Assignment_2.pdf`](docs/IT23274648_DWBI_Assignment_2.pdf).
