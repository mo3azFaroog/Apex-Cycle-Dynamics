# Apex Cycle Dynamics --- Sales & Operations Intelligence

## 1. Project Overview

**Apex Cycle Dynamics --- Sales & Operations Intelligence** is a
multi-page Microsoft Power BI business intelligence solution designed to
turn transactional sales, product, reseller, employee, and customer data
into an interactive management dashboard.

The project was developed as a practical end-to-end Power BI portfolio
project, covering:

-   Data extraction from SQL Server
-   Data cleaning and transformation with Power Query
-   Dimensional data modeling / star schema design
-   Relationship management between fact and dimension tables
-   Calculated columns and DAX measures
-   KPI design
-   Interactive slicers, Top-N analysis, tables, charts, maps, and
    navigation
-   Multi-page report design and visual formatting
-   Dashboard usability and troubleshooting
-   Business-oriented interpretation of sales and operations data

### Business scenario

The report presents the company as **Apex Cycle Dynamics**, a
bicycle-focused business that sells products through customers and
resellers across multiple territories.

> **Important data note:** The analytical scenario and branding are Apex
> Cycle Dynamics, while the underlying warehouse used for the project is
> based on **Microsoft AdventureWorksDW2025** data. The displayed
> business figures therefore represent the sample warehouse data modeled
> through the Apex scenario rather than audited real-world company
> figures.

------------------------------------------------------------------------

## 2. Project Objectives

The main objectives were to build a dashboard that allows management to:

1.  Monitor overall sales performance.
2.  Track order volume and average order value.
3.  Understand product performance and profitability.
4.  Monitor product production status.
5.  Evaluate reseller performance and reseller risk.
6.  Analyze employee distribution and sales performance.
7.  Understand customer and territory performance.
8.  Analyze sales by time, category, country, and territory.
9.  Identify high-value products, customers, resellers, and employees.
10. Provide an intuitive navigation experience from a management landing
    page into detailed analytical pages.

------------------------------------------------------------------------

# 3. Technology Stack

  -----------------------------------------------------------------------
  Technology                          Purpose
  ----------------------------------- -----------------------------------
  SQL Server                          Restore and query the
                                      AdventureWorksDW2025 warehouse

  AdventureWorksDW2025                Main sample data warehouse

  Power Query                         Data extraction, profiling,
                                      cleaning, transformation, and
                                      preparation

  Power BI Desktop                    Data modeling, DAX, visuals, report
                                      development

  DAX                                 Measures, calculated logic, KPIs,
                                      time intelligence, and analytical
                                      calculations

  Bing Maps / map visual              Geographic sales analysis

  Power BI slicers                    Interactive filtering

  Power BI tables/matrices            Detailed record-level analysis
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 4. Source Data

The project started from the **AdventureWorksDW2025.bak** SQL Server
backup.

The database was restored into SQL Server and then connected to Power
BI.

The core analytical model used the following tables:

### Fact table

-   `FactInternetSales`

### Dimension tables

-   `DimDate`
-   `DimCustomer`
-   `DimProduct`
-   `DimProductSubcategory`
-   `DimProductCategory`
-   `DimSalesTerritory`

The model follows a dimensional / star-schema approach in which the
sales fact table stores transactional sales records while dimension
tables provide descriptive attributes for filtering and analysis.

------------------------------------------------------------------------

# 5. Data Architecture

## 5.1 Star Schema

The central table is:

`FactInternetSales`

It is surrounded by dimensions that describe:

-   Date
-   Customer
-   Product
-   Product Subcategory
-   Product Category
-   Sales Territory

Conceptually:

``` text
                         DimDate
                            |
                            |
DimCustomer ---- FactInternetSales ---- DimProduct
                            |
                            |
                    DimSalesTerritory

DimProduct
    |
    v
DimProductSubcategory
    |
    v
DimProductCategory
```

The product hierarchy is:

``` text
Product Category
       ↓
Product Subcategory
       ↓
Product
```

This structure supports category → subcategory → product analysis.

------------------------------------------------------------------------

# 6. SQL Server Preparation

## Step 1 --- Restore AdventureWorksDW2025

The `.bak` backup was restored in SQL Server Management Studio.

General workflow:

1.  Open SQL Server Management Studio.
2.  Connect to the SQL Server instance.
3.  Open **Databases**.
4.  Select **Restore Database**.
5.  Select the `AdventureWorksDW2025.bak` backup.
6.  Configure the destination database.
7.  Complete the restore.
8.  Verify that the database and required tables are available.

## Step 2 --- Validate the source tables

Before loading the model into Power BI, the required dimensions and fact
table were identified.

The main tables selected were:

``` text
DimDate
DimCustomer
DimProduct
DimProductSubcategory
DimProductCategory
DimSalesTerritory
FactInternetSales
```

The project deliberately started with a manageable core model instead of
loading every warehouse table.

------------------------------------------------------------------------

# 7. Power BI Data Connection

The SQL Server database was connected to Power BI Desktop.

General workflow:

1.  Open Power BI Desktop.
2.  Select **Get Data**.
3.  Choose **SQL Server**.
4.  Enter the SQL Server instance.
5.  Select `AdventureWorksDW2025`.
6.  Select the required tables.
7.  Load the data into Power Query / Power BI.
8.  Review data types and table structure before modeling.

------------------------------------------------------------------------

# 8. Data Cleaning and Transformation

Data preparation was treated as a separate stage before building
visuals.

The main Power Query workflow covered the following operations.

## 8.1 Data profiling

The data was reviewed for:

-   Null values
-   Errors
-   Duplicate records
-   Incorrect data types
-   Inconsistent values
-   Unnecessary columns
-   Naming issues
-   Columns containing codes instead of readable descriptions

Data profiling is important because incorrect source data can propagate
into measures, filters, slicers, and visuals.

------------------------------------------------------------------------

## 8.2 Data types

Columns were checked and assigned appropriate types.

Examples:

-   Date fields → Date / DateTime
-   Keys → Whole Number
-   Quantities → Whole Number
-   Sales amounts → Decimal / Currency
-   Percentages → Decimal
-   Text attributes → Text

Correct data types are essential for:

-   DAX calculations
-   Sorting
-   Time intelligence
-   Aggregation
-   Slicers
-   Numeric formatting

------------------------------------------------------------------------

## 8.3 Removing unnecessary columns

Columns that were not required for the business questions were excluded
where appropriate.

This reduces:

-   Model size
-   Visual clutter
-   Refresh overhead
-   Confusing fields in the Fields pane

The goal was to keep the analytical model focused on business-relevant
fields.

------------------------------------------------------------------------

## 8.4 Handling nulls and errors

Null and error values were inspected before calculations were created.

Typical actions included:

-   Removing records when a missing value made the record unusable
-   Replacing missing values where a meaningful business default existed
-   Keeping blanks when blank was semantically correct
-   Removing or correcting error rows
-   Validating key fields used for relationships

------------------------------------------------------------------------

## 8.5 Duplicates

Duplicate records were reviewed, especially where uniqueness was
expected.

Dimension keys need to behave as unique identifiers on the one-side of
relationships.

------------------------------------------------------------------------

## 8.6 Rename and standardize fields

Field names were kept understandable for report development.

Readable field names make:

-   DAX easier to write
-   Visual configuration easier
-   Business users more comfortable with the report
-   The data model easier to maintain

------------------------------------------------------------------------

## 8.7 Split and merge columns

Power Query operations discussed and used as part of the transformation
workflow included:

-   Split Column
-   Merge Columns
-   Replace Values
-   Conditional Columns
-   Custom Columns

These transformations are useful for converting raw source attributes
into report-ready dimensions.

------------------------------------------------------------------------

## 8.8 Merge vs Append

A key Power Query concept addressed during the project:

### Merge

Combines columns from related queries based on a matching key.

Equivalent conceptually to a SQL join.

### Append

Stacks rows from two or more queries with compatible structures.

Equivalent conceptually to combining datasets vertically.

This distinction was included in the data preparation process because it
is fundamental to building reliable Power BI models.

------------------------------------------------------------------------

## 8.9 Date table

A dedicated date dimension was used through `DimDate`.

The date dimension provides the foundation for:

-   Year slicers
-   Monthly sales analysis
-   Time-based filtering
-   Sales growth calculations
-   Year-to-date analysis
-   Previous-year comparisons
-   Year-over-year analysis

------------------------------------------------------------------------

# 9. Data Model and Relationships

The model uses a star-schema design.

The main relationship pattern is:

``` text
DimDate              1 ─────── * FactInternetSales
DimCustomer          1 ─────── * FactInternetSales
DimProduct           1 ─────── * FactInternetSales
DimSalesTerritory    1 ─────── * FactInternetSales
```

The product hierarchy is connected as:

``` text
DimProductCategory
        1
        |
        *
DimProductSubcategory
        1
        |
        *
DimProduct
        1
        |
        *
FactInternetSales
```

### Date relationship

`DimDate` was connected to `FactInternetSales` using the sales date key.

The **OrderDate** relationship was treated as the primary active date
relationship for the core sales analysis.

Other date concepts such as due date and ship date can exist in the
warehouse, but the main report analysis was centered on order date.

------------------------------------------------------------------------

# 10. Why the Star Schema Was Used

The star schema provides:

-   Simpler DAX
-   Better filter propagation
-   Clear fact/dimension separation
-   Easier report development
-   Better scalability
-   Easier troubleshooting
-   More intuitive business analysis

Instead of joining everything into one very wide table, the model keeps
descriptive attributes in dimensions and transactional measurements in
the fact table.

------------------------------------------------------------------------

# 11. Measures and DAX Layer

A major part of the project was separating reusable calculations from
raw columns.

A dedicated measures table / organized measure structure was considered
to keep the model clean.

Display folders were also discussed for organizing measures into logical
groups such as:

-   Sales
-   Customers
-   KPIs
-   Operations

> Power BI does not provide a normal folder hierarchy for physically
> grouping model tables. Display folders are primarily used to organize
> fields/measures inside tables. A dedicated Measures table is therefore
> a practical way to keep calculations organized.

------------------------------------------------------------------------

# 12. Core DAX Measures

The following are representative core measures used / designed for the
project.

## Total Sales

``` dax
Total Sales =
SUM(FactInternetSales[SalesAmount])
```

## Total Cost

``` dax
Total Cost =
SUM(FactInternetSales[TotalProductCost])
```

## Total Profit

``` dax
Total Profit =
[Total Sales] - [Total Cost]
```

## Profit Margin %

``` dax
Profit Margin % =
DIVIDE([Total Profit], [Total Sales], 0)
```

## Total Orders

Depending on the chosen business definition, orders can be calculated
using a distinct order identifier:

``` dax
Total Orders =
DISTINCTCOUNT(FactInternetSales[SalesOrderNumber])
```

## Quantity Sold

``` dax
Total Quantity =
SUM(FactInternetSales[OrderQuantity])
```

## Average Order Value

``` dax
Average Order Value =
DIVIDE([Total Sales], [Total Orders], 0)
```

## Year-to-Date Sales

``` dax
Sales YTD =
TOTALYTD(
    [Total Sales],
    DimDate[FullDateAlternateKey]
)
```

## Previous Year Sales

``` dax
Sales Previous Year =
CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR(DimDate[FullDateAlternateKey])
)
```

## Sales Growth %

``` dax
Sales Growth % =
DIVIDE(
    [Total Sales] - [Sales Previous Year],
    [Sales Previous Year],
    0
)
```

These measures form the analytical layer used by cards, charts, tables,
KPIs, and page-level analysis.

------------------------------------------------------------------------

# 13. Calculated Columns

Calculated columns were considered for row-level business logic that
needs to exist at the record level.

Examples of suitable calculated-column logic include:

-   Product price tier
-   Age group
-   Customer classification
-   Status categories
-   Business-friendly labels
-   Derived row-level classifications

A price-tier classification can conceptually be built as:

``` dax
Price Tier =
SWITCH(
    TRUE(),
    [Price] >= HighThreshold, "High",
    [Price] >= MediumThreshold, "Medium",
    [Price] >= LowThreshold, "Low",
    "No price"
)
```

The exact thresholds should be aligned with the project's final
model/business rules.

### Calculated column vs measure

A key modeling distinction:

**Calculated column** - Calculated row by row - Stored in the model -
Useful for categories and slicers

**Measure** - Calculated at query time - Responds dynamically to filter
context - Best for KPIs and aggregations

------------------------------------------------------------------------

# 14. KPI Design

The dashboard was designed around management-friendly KPIs.

Examples visible in the final report include:

-   Total Sales
-   Total Orders
-   Active Resellers
-   Average Profit Margin
-   Average Order Value
-   In Production
-   Out of Production
-   Average Dealer Price
-   Average Lifecycle
-   Total Reseller Sales
-   Reseller Profit Margin
-   Resellers at Risk
-   Average Years as Partner
-   Total Employees
-   Average Annual Salary
-   Total Vacation Cost
-   Top Salesperson
-   Total Customers
-   Total Internet Sales
-   Sales Growth Display
-   Top Territory

The KPI cards were deliberately placed near the top of each analytical
page so that users can understand the current state before moving into
detailed charts.

------------------------------------------------------------------------

# 15. Final Dashboard Architecture

The final report contains a landing page plus multiple analytical pages.

## Page 1 --- Home / Executive Overview

Title:

**Apex Cycle Dynamics --- Sales & Operations Intelligence**

The home page functions as the management navigation hub.

It contains:

-   Company/project title
-   Year selector
-   Total Sales
-   Total Orders
-   Active Resellers
-   Average Profit Margin
-   Navigation tiles/buttons

Navigation sections:

1.  Sales Overview
2.  Product Analysis
3.  Reseller Performance
4.  Employees
5.  Customers & Territory
6.  Financial Summary
7.  Call Center

A footer identifies the source context:

`Last refreshed: 28 Jan 2014 | Powered by AdventureWorks data`

The home page uses a branded bicycle / cycling visual identity.

------------------------------------------------------------------------

# 16. Page 2 --- Sales Overview

### Main purpose

Provide a high-level view of sales performance across time, products,
categories, and geography.

### Slicers

-   Category
-   Channel
-   Year

### KPI cards

-   Total Sales
-   Sales Growth %
-   Total All Orders
-   Average Order Value

### Visuals

#### Sales by Month

A line chart showing sales movement over time.

#### Top 10 Products

A table showing product names and total sales.

#### Total Sales by Category Name

A column chart comparing:

-   Bikes
-   Components
-   Clothing
-   Accessories

#### Total Sales by Country Region Name

A column chart showing sales by country/region.

#### Total Sales by Category Name

A donut/pie chart showing category share.

### Visible dashboard example values

The screenshots show approximately:

-   Total Sales: `$109.809M`
-   Total Orders: `31K`
-   Average Order Value: `$21.19K`
-   Bikes account for the dominant category share in the displayed
    sample.

The exact displayed values depend on the current filter context.

------------------------------------------------------------------------

# 17. Page 3 --- Product Analysis

### Main purpose

Understand product portfolio performance, profitability, production
status, pricing, and lifecycle.

### Slicers

-   Category
-   Product Line
-   Status

### KPI cards

-   In Production
-   Average Dealer Price
-   Average Profit Margin %
-   Out of Production
-   Average Lifecycle (Months)

### Visuals

#### Profit Margin % by Category

Horizontal bar chart comparing product categories.

#### Products by Status

Donut chart separating:

-   In Production
-   Out of Production

#### Products by Price Tier

Bar chart showing:

-   High
-   Medium
-   Low
-   Premium
-   No price

#### Top 5 Products by Profit Margin

Detailed table containing:

-   Product Name
-   Product Category
-   Average Profit Margin

The screenshot shows accessory products such as:

-   All-Purpose Bike Stand
-   Fender Set - Mountain
-   HL Mountain Tire
-   HL Road Tire
-   ML Mountain Tire

as examples in the top-profit-margin table.

------------------------------------------------------------------------

# 18. Page 4 --- Reseller Performance

### Main purpose

Evaluate reseller revenue, activity, profitability, partnership
duration, and risk.

### Slicers

-   Reseller Name
-   Category
-   Year

### KPI cards

-   Total Reseller Sales
-   Active Resellers
-   Reseller Profit Margin %
-   Resellers at Risk
-   Average Years as Partner

### Visuals

#### Top 10 Resellers by Sales

Horizontal bar chart.

#### Resellers Status

Donut chart containing:

-   Active
-   At Risk
-   Inactive

The displayed screenshot shows approximately:

-   Active: 69.76%
-   At Risk: 20.83%
-   Inactive: 9.42%

#### Reseller Detail Table

Columns include:

-   ResellerName
-   ProductCategoryName
-   Sum of AnnualSales
-   Reseller Status
-   Avg Profit Margin %

Status icons were used to make the table more readable.

Examples include:

-   Active → green status indicator
-   At Risk → warning indicator
-   Inactive → red status indicator

------------------------------------------------------------------------

# 19. Page 5 --- Employees

### Main purpose

Analyze workforce composition, salaries, leave patterns, departments,
and sales contribution.

### Slicers

-   Department
-   Gender
-   Age Group

### KPI cards

-   Total Employees
-   Average Annual Salary
-   Total Vacation Cost
-   Top Salesperson

### Visuals

#### Employees by Department

Bar chart showing workforce distribution.

The screenshot shows Production as the largest department.

#### Age Group Distribution

Donut chart showing age-group composition.

#### Leave Pattern

Donut chart showing:

-   Vacation
-   Sick Leave
-   Balanced

#### Top Salesperson

Bar chart ranking sales employees by sales contribution.

#### Top 5 Employees by Sales

Detailed table containing:

-   Full Name
-   Employee Department
-   Total Reseller Sales

Examples visible in the report include:

-   Jae B Pak
-   Jillian Carson
-   Linda C Mitchell
-   Michael G Blythe

The page also identifies **Linda C Mitchell** as the displayed Top
Salesperson in the current sample context.

------------------------------------------------------------------------

# 20. Page 6 --- Customers & Territory

### Main purpose

Understand customer distribution, geographic sales, customer income, and
acquisition patterns.

### Slicers

-   Year
-   Territory / Region
-   Gender

### KPI cards

-   Total Customers
-   Total Internet Sales
-   Total Orders
-   Sales Growth Display
-   Top Territory

### Visuals

#### Sales by Country

A geographic map visual was used to display the geographic distribution
of sales.

The map used location/category information and Bing/Microsoft map
rendering.

#### Top 10 Customers with the Highest Income

Horizontal bar chart ranked by yearly income.

#### Customer Acquisition Trend

A time-based visual showing new-customer activity over time.

### Geographic analysis

The page provides a visual way to move from country/territory-level
performance into individual customer analysis.

------------------------------------------------------------------------

# 21. Financial Summary and Call Center

The Home page also includes dedicated navigation tiles for:

-   Financial Summary
-   Call Center

These areas were included in the overall information architecture so the
report can be expanded into broader operations intelligence without
changing the navigation concept.

The project structure therefore goes beyond pure sales reporting and
establishes a foundation for a broader operational BI solution.

------------------------------------------------------------------------

# 22. Report Navigation

Navigation was designed around the Home page.

The home page uses visual navigation tiles to move between analytical
sections.

A **Home** button was also placed on detailed pages to return users to
the landing page.

This creates a simple flow:

``` text
Home
 ├── Sales Overview
 ├── Product Analysis
 ├── Reseller Performance
 ├── Employees
 ├── Customers & Territory
 ├── Financial Summary
 └── Call Center
```

------------------------------------------------------------------------

# 23. Filters and Slicers

Slicers were used consistently at the top of the analytical pages.

They allow users to dynamically filter:

-   Year
-   Category
-   Channel
-   Product Line
-   Status
-   Reseller
-   Department
-   Gender
-   Age Group
-   Territory / Region

A clear-filter control was also used in the report design to help users
return to an unfiltered view.

------------------------------------------------------------------------

# 24. Top-N Analysis

Top-N analysis was used repeatedly to turn large datasets into
actionable summaries.

Examples:

-   Top 10 Products
-   Top 10 Resellers
-   Top 5 Products by Profit Margin
-   Top 5 Employees by Sales
-   Top 10 Customers by Income

This approach makes the report useful for management without requiring
users to inspect thousands of records.

------------------------------------------------------------------------

# 25. Visual Design System

The report uses a consistent visual identity.

### Main design characteristics

-   Warm brown / terracotta accent color
-   Light neutral background
-   Rounded containers
-   Shadow effects
-   Dark headings
-   White KPI cards
-   Consistent typography
-   Icon-based navigation
-   Consistent page headers
-   Home navigation icon
-   KPI cards across the top
-   Section titles above visuals

The cycling theme is reinforced with bicycle and cycling-related icons.

The objective was to make the report look like a cohesive business
product rather than a collection of unrelated Power BI visuals.

------------------------------------------------------------------------

# 26. UI / Power BI Troubleshooting Covered During Development

Several Power BI interface and visual configuration problems were
addressed during development.

## 26.1 Map visual fields

The map visual did not initially expose the expected field such as
**Size**.

The solution was to identify the correct map visual and map fields
according to the visual's current field well, rather than assuming every
map visual exposes identical configuration options.

------------------------------------------------------------------------

## 26.2 Tooltip configuration

Tooltip behavior was investigated when expected information did not
appear.

The principle used was:

-   Verify the field is placed in the visual's Tooltip bucket.
-   Check the visual's formatting options.
-   Confirm that the measure returns a value under the current filter
    context.

------------------------------------------------------------------------

## 26.3 Card visual and shapes

A card was sometimes hidden behind shapes.

The solution was to use the layer/order controls:

-   Bring to front
-   Send to back
-   Selection pane

This ensured KPI values remained visible above background shapes.

------------------------------------------------------------------------

## 26.4 Card category label

The category label was not appearing as expected.

The card formatting panel was used to control:

-   Category label
-   Callout value
-   Title
-   Alignment
-   Font size

This was important for keeping KPI cards consistent.

------------------------------------------------------------------------

## 26.5 Full-screen report view

Power BI report view and presentation/full-screen behavior were also
addressed so the dashboard could be reviewed as a presentation rather
than only as an editing canvas.

------------------------------------------------------------------------

## 26.6 Table organization

A common modeling question was how to group multiple model tables into a
folder.

Power BI does not simply place entire tables into ordinary display
folders.

The practical solution discussed was:

-   Use a dedicated Measures table.
-   Use Display Folders for measures/fields.
-   Keep model tables logically arranged in Model view.
-   Use naming conventions to make the model easier to navigate.

------------------------------------------------------------------------

# 27. Data Modeling Best Practices Applied

The project followed several BI best practices:

### 1. Separate facts from dimensions

`FactInternetSales` contains transactional information.

Dimensions describe the business entities.

### 2. Avoid unnecessary many-to-many relationships

The model was designed around one-to-many dimension-to-fact
relationships.

### 3. Keep calculations reusable

Important KPIs were implemented as measures instead of duplicating logic
across visuals.

### 4. Use dimensions for filtering

Year, category, customer, product, territory, and similar slicers are
driven from dimension attributes.

### 5. Keep the report visually consistent

The same design language was used across pages.

------------------------------------------------------------------------

# 28. End-to-End Development Workflow

The complete project workflow can be summarized as:

``` text
AdventureWorksDW2025.bak
        ↓
SQL Server Restore
        ↓
Source Table Selection
        ↓
Power BI SQL Connection
        ↓
Power Query
        ↓
Data Profiling
        ↓
Data Cleaning
        ↓
Data Type Validation
        ↓
Column Selection / Renaming
        ↓
Null / Error / Duplicate Handling
        ↓
Transformations
        ↓
Date Preparation
        ↓
Data Model
        ↓
Relationships
        ↓
Calculated Columns
        ↓
DAX Measures
        ↓
KPI Definitions
        ↓
Slicers & Filters
        ↓
Charts / Tables / Maps
        ↓
Page Navigation
        ↓
Formatting & Theme
        ↓
Testing / Troubleshooting
        ↓
Final Sales & Operations Intelligence Dashboard
```

------------------------------------------------------------------------

# 29. Quality Assurance

Before finalizing the dashboard, the following areas should be
validated:

## Data

-   Correct row counts
-   Correct data types
-   No unexpected errors
-   No broken relationship keys
-   Correct date filtering

## Model

-   Correct relationship direction
-   Correct cardinality
-   Correct active date relationship
-   No unnecessary ambiguous paths

## DAX

-   Measures return expected values
-   Division-by-zero handled using `DIVIDE`
-   Time intelligence uses a proper date dimension
-   Filter context produces expected results

## Visuals

-   Titles match the analysis
-   Units are correctly formatted
-   Currency is consistent
-   Percentages use percentage formatting
-   Tables are readable
-   Top-N filters behave correctly
-   Slicers interact correctly

## Navigation

-   Home buttons work
-   Navigation tiles lead to the correct pages
-   Clear-filter behavior works
-   Page layout remains consistent

------------------------------------------------------------------------

# 30. Business Questions Answered by the Dashboard

The solution is designed to answer questions such as:

### Sales

-   How much are we selling?
-   How many orders are being generated?
-   How is sales changing over time?
-   What is the average order value?
-   Which categories drive sales?

### Products

-   Which products generate the most sales?
-   Which categories have stronger margins?
-   Which products are in production?
-   Which products are out of production?
-   How are products distributed across price tiers?

### Resellers

-   Which resellers generate the most revenue?
-   How many resellers are active?
-   How many are at risk?
-   What is reseller profitability?
-   How long have partners been working with the company?

### Employees

-   How large is each department?
-   What is the average annual salary?
-   Which employees contribute the most sales?
-   What does the leave pattern look like?
-   What is the workforce age distribution?

### Customers and Territory

-   Which territories generate the most sales?
-   Who are the highest-income customers?
-   How is customer acquisition changing?
-   How do customer attributes relate to sales?

------------------------------------------------------------------------

# 31. Portfolio Value

This project demonstrates practical skills in:

-   SQL Server
-   Data warehousing
-   Power Query
-   ETL concepts
-   Data cleaning
-   Dimensional modeling
-   Star schema design
-   Data relationships
-   DAX
-   Time intelligence
-   KPI development
-   Data visualization
-   Business analysis
-   Geographic analysis
-   Dashboard UX
-   Interactive reporting
-   Power BI troubleshooting

It is therefore suitable as a portfolio project for roles such as:

-   Data Analyst
-   BI Analyst
-   Power BI Developer
-   Junior BI Developer
-   Reporting Analyst
-   Business Intelligence Intern / Junior
-   IT / Technical Support professional demonstrating data-analysis
    capability

------------------------------------------------------------------------

# 32. Suggested Repository Structure

``` text
Apex-Cycle-Dynamics/
│
├── README.md
├── README_AR.md
│
├── screenshots/
│   ├── home.png
│   ├── sales-overview.png
│   ├── product-analysis.png
│   ├── reseller-performance.png
│   ├── employees.png
│   └── customers-territory.png
│
├── data/
│   └── source-data-description.md
│
├── sql/
│   └── database-notes.md
│
├── powerbi/
│   └── Apex-Cycle-Dynamics.pbix
│
└── documentation/
    ├── data-model.md
    └── dax-measures.md
```

------------------------------------------------------------------------

# 33. Project Completion Summary

The Apex Cycle Dynamics project progressed from raw warehouse data to a
complete interactive BI product.

The most important achievement was not simply creating charts. The
project demonstrated the complete BI lifecycle:

**Source → Clean → Model → Calculate → Analyze → Visualize → Navigate →
Validate**

The final result is a multi-page Sales & Operations Intelligence
solution that connects data engineering concepts, analytical modeling,
DAX, and business-focused dashboard design into one portfolio-ready
Power BI project.

------------------------------------------------------------------------

## 34. Key Project Facts from the Final Dashboard

The supplied dashboard screenshots show the following example outputs
under the displayed filter contexts:

-   Total Sales: approximately **\$109.809M**
-   Total Orders: approximately **31K**
-   Active Resellers: **489**
-   Product analysis shows approximately **197 In Production** and **200
    Out of Production**
-   Product status is displayed at approximately **49.62% In
    Production** and **50.38% Out of Production**
-   Reseller analysis shows **489 Active Resellers** and approximately
    **146 Resellers at Risk**
-   Reseller status distribution shown: approximately **69.76% Active**,
    **20.83% At Risk**, **9.42% Inactive**
-   Employee page shows **296 Total Employees**
-   Employee page shows **\$38.7K Average Annual Salary**
-   Employee page shows **\$269.1K Total Vacation Cost**
-   Customers & Territory page shows **101 Total Customers**
-   The displayed top territory is **United States**
-   The report uses AdventureWorks sample data behind the Apex Cycle
    Dynamics business presentation

These values are context-dependent and may change when slicers or
filters are changed.

------------------------------------------------------------------------

# 35. Final Notes

This README documents the project as an end-to-end Power BI case study.

For a production implementation, the next stage would be to replace the
AdventureWorks sample warehouse with Apex Cycle Dynamics' actual
operational data while preserving the same analytical architecture,
governance principles, KPI layer, and dashboard design.

**Project:** Apex Cycle Dynamics\
**Solution:** Sales & Operations Intelligence\
**Platform:** Microsoft Power BI\
**Source:** AdventureWorksDW2025 sample warehouse\
**Primary model:** FactInternetSales + dimensional tables\
**Report type:** Interactive multi-page BI dashboard
