# E-commerce Data Warehouse and BI Solution

A business intelligence solution built on an e-commerce star-schema data warehouse (EcommerceDW). Completed for the Data Warehousing and Business Intelligence module (IT3021), April 2026.

## Contents
- `ssas/`: SSAS multidimensional cube project (Cube_EDW), with data source, data source view, dimensions, and hierarchies
- `excel/`: Excel workbooks used for OLAP pivot analysis
- `power-bi/`: Power BI report with matrix, cascading slicers, drill-down, and drill-through pages
- `docs/`: assignment report

## Star schema
- Fact table: FactOrders (SalePrice, Cost, Profit, transaction processing time)
- Dimensions: DimUser (SCD Type 2), DimProduct, DimDistributionCenter, DimDate

## Tools
SQL Server, SSIS, SSAS (SSDT), Excel, Power BI Desktop

## How to open
- Cube: open `ssas/EcommerceCubeProject.slnx` in Visual Studio with SSDT. You need a local SQL Server with the EDW database restored.
- Power BI: open `power-bi/DWBI_Assignment_2.pbix` in Power BI Desktop and update the data source to your local server.
