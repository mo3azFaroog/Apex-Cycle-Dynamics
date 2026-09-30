# Apex Cycle Dynamics --- ذكاء المبيعات والعمليات

## 1. نظرة عامة على المشروع

**Apex Cycle Dynamics --- Sales & Operations Intelligence** هو مشروع
ذكاء أعمال متكامل تم بناؤه باستخدام Microsoft Power BI لتحويل بيانات
المبيعات والمنتجات والموزعين والموظفين والعملاء والمناطق إلى نظام تقارير
وتحليلات تفاعلي متعدد الصفحات.

المشروع يمثل تجربة عملية كاملة لدورة حياة مشروع Power BI، بداية من قاعدة
البيانات الخام وحتى لوحة المعلومات النهائية، ويغطي:

-   استخراج البيانات من SQL Server
-   تنظيف وتحويل البيانات باستخدام Power Query
-   بناء Data Model بأسلوب Star Schema
-   إنشاء العلاقات بين جداول الـ Fact والـ Dimensions
-   إنشاء Calculated Columns وDAX Measures
-   تصميم KPIs
-   إنشاء Slicers وفلاتر تفاعلية
-   تحليل Top-N
-   الجداول والمخططات والخرائط
-   تصميم صفحات Dashboard متعددة
-   التنقل بين الصفحات
-   تنسيق الواجهة وتجربة المستخدم
-   معالجة مشاكل Power BI أثناء التطوير
-   تحويل البيانات إلى معلومات قابلة للاستخدام الإداري

### سيناريو العمل

تم تقديم المشروع باسم **Apex Cycle Dynamics**، وهي شركة افتراضية تعمل في
مجال الدراجات والمنتجات المرتبطة بها وتبيع عبر العملاء والموزعين في
مناطق مختلفة.

> **ملاحظة مهمة حول البيانات:** اسم الشركة والسيناريو التحليلي هما Apex
> Cycle Dynamics، بينما مصدر البيانات الأساسي المستخدم في المشروع هو
> **AdventureWorksDW2025**. لذلك فإن الأرقام الظاهرة في التقرير هي أرقام
> من بيانات AdventureWorks التجريبية بعد نمذجتها وعرضها ضمن سيناريو Apex
> Cycle Dynamics، وليست أرقاماً مالية حقيقية لشركة Apex.

------------------------------------------------------------------------

# 2. أهداف المشروع

تم تصميم النظام للإجابة على أسئلة إدارية وتحليلية مثل:

1.  ما إجمالي المبيعات؟
2.  ما عدد الطلبات؟
3.  كيف تتغير المبيعات عبر الزمن؟
4.  ما متوسط قيمة الطلب؟
5.  ما المنتجات والفئات الأعلى مبيعاً؟
6.  ما الفئات الأعلى من حيث هامش الربح؟
7.  ما المنتجات الموجودة في الإنتاج وما المنتجات الخارجة من الإنتاج؟
8.  ما أداء الموزعين؟
9.  كم عدد الموزعين النشطين أو المعرضين للخطر؟
10. ما أداء الموظفين وأفضل مندوبي المبيعات؟
11. كيف يتوزع الموظفون حسب الإدارات والأعمار؟
12. ما المناطق والعملاء الأعلى أداءً؟
13. كيف يتغير اكتساب العملاء؟
14. كيف يمكن للإدارة الانتقال من المؤشرات العامة إلى التفاصيل؟

------------------------------------------------------------------------

# 3. التقنيات المستخدمة

  التقنية                  الاستخدام
  ------------------------ ---------------------------------------------------
  SQL Server               استعادة قاعدة البيانات والتعامل مع Data Warehouse
  AdventureWorksDW2025     مصدر البيانات الأساسي
  Power Query              تنظيف وتحويل وتجهيز البيانات
  Power BI Desktop         بناء النموذج والـ Dashboard
  DAX                      Measures وCalculated Logic وKPIs
  Map Visual / Bing Maps   التحليل الجغرافي
  Slicers                  التصفية التفاعلية
  Tables / Matrices        عرض التفاصيل

------------------------------------------------------------------------

# 4. مصدر البيانات

بدأ المشروع من ملف:

`AdventureWorksDW2025.bak`

تمت استعادة قاعدة البيانات داخل SQL Server، ثم الاتصال بها من Power BI.

الجداول الأساسية المستخدمة هي:

### Fact Table

-   `FactInternetSales`

### Dimension Tables

-   `DimDate`
-   `DimCustomer`
-   `DimProduct`
-   `DimProductSubcategory`
-   `DimProductCategory`
-   `DimSalesTerritory`

تم اختيار هذه الجداول لتكوين نموذج تحليلي عملي ومترابط بدلاً من تحميل كل
جداول الـ Data Warehouse دون حاجة.

------------------------------------------------------------------------

# 5. بناء Data Warehouse في SQL Server

## الخطوة الأولى --- Restore لقاعدة البيانات

تم التعامل مع ملف:

`AdventureWorksDW2025.bak`

من خلال SQL Server Management Studio.

الخطوات العامة:

1.  فتح SQL Server Management Studio.
2.  الاتصال بالـ SQL Server.
3.  فتح Databases.
4.  اختيار Restore Database.
5.  تحديد ملف الـ `.bak`.
6.  اختيار اسم قاعدة البيانات.
7.  تنفيذ Restore.
8.  التأكد من ظهور قاعدة البيانات.
9.  التحقق من وجود الجداول المطلوبة.

------------------------------------------------------------------------

# 6. اختيار الجداول

بعد استعادة قاعدة البيانات، تم تحديد الجداول المطلوبة للمشروع:

``` text
DimDate
DimCustomer
DimProduct
DimProductSubcategory
DimProductCategory
DimSalesTerritory
FactInternetSales
```

الفكرة الأساسية هي أن:

`FactInternetSales`

يمثل مركز النموذج ويحتوي على معاملات المبيعات، بينما الجداول الأخرى توفر
الخصائص التي يتم استخدامها في الفلترة والتحليل.

------------------------------------------------------------------------

# 7. الاتصال بين SQL Server وPower BI

تم استخدام Power BI Desktop للاتصال بقاعدة البيانات.

الخطوات:

1.  فتح Power BI Desktop.
2.  اختيار Get Data.
3.  اختيار SQL Server.
4.  إدخال اسم SQL Server.
5.  اختيار قاعدة `AdventureWorksDW2025`.
6.  تحديد الجداول المطلوبة.
7.  فتح البيانات في Power Query.
8.  مراجعة أنواع البيانات والحقول قبل بناء الـ Data Model.

------------------------------------------------------------------------

# 8. تنظيف البيانات باستخدام Power Query

مرحلة تنظيف البيانات كانت من أهم مراحل المشروع، وتم التعامل معها قبل
البدء في بناء الـ Dashboard.

## 8.1 Data Profiling

تمت مراجعة البيانات للبحث عن:

-   Nulls
-   Errors
-   Duplicates
-   أنواع بيانات خاطئة
-   قيم غير متناسقة
-   أعمدة غير ضرورية
-   أسماء غير واضحة
-   أكواد تحتاج إلى تحويلها إلى وصف مفهوم

الهدف هو منع البيانات غير الصحيحة من التأثير على الـ Measures والـ
Filters والـ Visuals.

------------------------------------------------------------------------

## 8.2 ضبط Data Types

تمت مراجعة أنواع البيانات.

أمثلة:

-   التواريخ → Date / DateTime
-   المفاتيح → Whole Number
-   الكميات → Whole Number
-   المبيعات → Decimal / Currency
-   النسب → Decimal
-   النصوص → Text

ضبط الـ Data Types مهم جداً لأنه يؤثر على:

-   DAX
-   Sorting
-   Time Intelligence
-   Aggregation
-   Slicers
-   Formatting

------------------------------------------------------------------------

## 8.3 إزالة الأعمدة غير الضرورية

تمت مراجعة الأعمدة وحذف ما لا يخدم أهداف التحليل عند الحاجة.

الفائدة:

-   تقليل حجم الـ Model
-   تقليل التشويش
-   تحسين قابلية الاستخدام
-   تسهيل اختيار الحقول داخل Power BI

------------------------------------------------------------------------

## 8.4 التعامل مع Nulls وErrors

تمت مراجعة القيم الفارغة والأخطاء.

حسب طبيعة العمود، يمكن أن يكون الإجراء:

-   حذف السجل إذا كان غير صالح
-   استبدال القيمة إذا كان هناك Default منطقي
-   الإبقاء على Blank إذا كان ذلك صحيحاً من ناحية الأعمال
-   تصحيح أو حذف Errors
-   التأكد من صحة مفاتيح العلاقات

------------------------------------------------------------------------

## 8.5 التعامل مع Duplicates

تمت مراجعة التكرارات، خصوصاً في جداول الـ Dimensions.

في الجانب الواحد من العلاقة يجب أن تكون المفاتيح مناسبة لتحديد السجلات
بشكل فريد.

------------------------------------------------------------------------

## 8.6 Rename وStandardization

تم الحرص على أن تكون أسماء الحقول واضحة قدر الإمكان.

الأسماء الواضحة تساعد في:

-   كتابة DAX
-   إعداد الـ Visuals
-   فهم الـ Model
-   تسهيل الصيانة لاحقاً

------------------------------------------------------------------------

## 8.7 Split وMerge وReplace

تم التعامل مع عمليات Power Query المهمة مثل:

-   Split Column
-   Merge Columns
-   Replace Values
-   Conditional Column
-   Custom Column

وذلك لتحويل البيانات الخام إلى بيانات مناسبة للتحليل.

------------------------------------------------------------------------

## 8.8 الفرق بين Merge وAppend

تمت دراسة هذه النقطة أثناء المشروع:

### Merge

يضيف أعمدة من Query آخر اعتماداً على مفتاح مشترك.

يشبه SQL JOIN.

### Append

يجمع صفوف Query مع Query آخر.

يشبه الجمع الرأسي للبيانات.

هذه النقطة مهمة جداً في ETL داخل Power BI.

------------------------------------------------------------------------

# 9. جدول التاريخ DimDate

تم استخدام `DimDate` كأساس للتحليل الزمني.

يسمح ذلك ببناء:

-   Year Slicer
-   Monthly Analysis
-   Sales Growth
-   YTD
-   Previous Year
-   YoY
-   التحليل حسب التاريخ

وجود Date Dimension واضحة مهم جداً لنجاح Time Intelligence في DAX.

------------------------------------------------------------------------

# 10. Data Model

تم بناء النموذج على أساس **Star Schema**.

الجداول الأساسية:

``` text
DimDate
DimCustomer
DimProduct
DimProductSubcategory
DimProductCategory
DimSalesTerritory
FactInternetSales
```

العلاقات الرئيسية:

``` text
DimDate              1 ─────── * FactInternetSales
DimCustomer          1 ─────── * FactInternetSales
DimProduct           1 ─────── * FactInternetSales
DimSalesTerritory    1 ─────── * FactInternetSales
```

أما تسلسل المنتجات:

``` text
Product Category
       ↓
Product Subcategory
       ↓
Product
       ↓
FactInternetSales
```

------------------------------------------------------------------------

# 11. علاقة التاريخ

تم استخدام تاريخ الطلب كالعلاقة الرئيسية في تحليل المبيعات:

`DimDate → FactInternetSales`

وكان **OrderDate** هو التاريخ النشط الأساسي للتحليل.

يمكن أن تحتوي قاعدة البيانات على تواريخ أخرى مثل Due Date وShip Date،
لكن التحليل الأساسي للمشروع اعتمد على Order Date.

------------------------------------------------------------------------

# 12. لماذا Star Schema؟

استخدام Star Schema يوفر:

-   DAX أبسط
-   Filter Propagation أوضح
-   فصل واضح بين Fact وDimensions
-   سهولة بناء التقارير
-   سهولة اكتشاف الأخطاء
-   قابلية أكبر للتوسع
-   نموذج مفهوم للمستخدم التجاري

بدلاً من دمج كل شيء في جدول واحد ضخم، تم الحفاظ على الفصل بين المعاملات
والوصف.

------------------------------------------------------------------------

# 13. Measures وتنظيم DAX

تم الاعتماد على Measures قابلة لإعادة الاستخدام بدلاً من كتابة نفس الحساب
داخل كل Visual.

كما تم العمل على فكرة تنظيم الـ Measures في:

-   Sales
-   Customers
-   KPIs
-   Operations

وتمت مناقشة استخدام **Measures Table** و**Display Folders**.

### ملاحظة مهمة

Power BI لا يوفر طريقة عادية لوضع الجداول نفسها داخل Display Folder مثل
نظام الملفات.

لذلك التنظيم الأفضل يكون عن طريق:

-   Measures Table
-   Display Folders للـ Measures/Fields
-   ترتيب الجداول داخل Model View
-   Naming Conventions

------------------------------------------------------------------------

# 14. أهم DAX Measures

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

``` dax
Total Orders =
DISTINCTCOUNT(FactInternetSales[SalesOrderNumber])
```

## Total Quantity

``` dax
Total Quantity =
SUM(FactInternetSales[OrderQuantity])
```

## Average Order Value

``` dax
Average Order Value =
DIVIDE([Total Sales], [Total Orders], 0)
```

## Sales YTD

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

هذه الـ Measures تشكل طبقة التحليل التي تعتمد عليها الـ Cards والـ
Charts والـ Tables والـ KPIs.

------------------------------------------------------------------------

# 15. Calculated Columns

تمت مناقشة واستخدام Calculated Columns للعمليات التي تحتاج إلى تصنيف كل
صف بشكل مستقل.

أمثلة:

-   Price Tier
-   Age Group
-   Customer Classification
-   Status
-   Business Labels
-   Row-level Categories

مثال منطقي لتصنيف السعر:

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

### الفرق بين Calculated Column وMeasure

**Calculated Column:** - يتم حسابها Row by Row - يتم تخزينها داخل الـ
Model - مناسبة للتصنيفات والـ Slicers

**Measure:** - يتم حسابها وقت الاستعلام - تتغير مع Filter Context -
مناسبة للـ KPIs والحسابات التجميعية

------------------------------------------------------------------------

# 16. تصميم الـ KPI Cards

تم تصميم مؤشرات رئيسية لكل صفحة.

أهم الـ KPIs الموجودة في المشروع:

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

تم وضع الـ KPI Cards في أعلى الصفحات لتقديم ملخص سريع قبل الدخول في
التفاصيل.

------------------------------------------------------------------------

# 17. الصفحة الرئيسية --- Home / Executive Overview

العنوان:

**Apex Cycle Dynamics --- Sales & Operations Intelligence**

هذه الصفحة هي مركز التنقل الرئيسي.

تحتوي على:

-   اسم المشروع
-   Year Selector
-   Total Sales
-   Total Orders
-   Active Resellers
-   Avg Profit Margin
-   Navigation Tiles

أقسام التنقل:

1.  Sales Overview
2.  Product Analysis
3.  Reseller Performance
4.  Employees
5.  Customers & Territory
6.  Financial Summary
7.  Call Center

كما يظهر في أسفل الصفحة:

`Last refreshed: 28 Jan 2014 | Powered by AdventureWorks data`

تم استخدام هوية بصرية مرتبطة بالدراجات.

------------------------------------------------------------------------

# 18. صفحة Sales Overview

### الهدف

تحليل أداء المبيعات حسب الوقت والفئة والمنتج والدولة.

### Slicers

-   Category
-   Channel
-   Year

### KPI Cards

-   Total Sales
-   Sales Growth %
-   Total All Orders
-   Average Order Value

### الرسومات

#### Sales by Month

Line Chart لعرض تغير المبيعات عبر الوقت.

#### Top 10 Products

جدول يعرض المنتجات الأعلى من حيث إجمالي المبيعات.

#### Total Sales by Category Name

مقارنة بين:

-   Bikes
-   Components
-   Clothing
-   Accessories

#### Total Sales by Country Region Name

تحليل المبيعات حسب الدولة/المنطقة.

#### Total Sales by Category Name

Donut/Pie Chart لعرض حصة كل فئة.

### أمثلة من القيم الظاهرة في الصورة

-   Total Sales: حوالي `$109.809M`
-   Total Orders: حوالي `31K`
-   Average Order Value: حوالي `$21.19K`

وتظهر فئة Bikes كالفئة الأكبر في البيانات المعروضة.

------------------------------------------------------------------------

# 19. صفحة Product Analysis

### الهدف

تحليل المنتجات من ناحية:

-   الربحية
-   الأسعار
-   الإنتاج
-   دورة حياة المنتج
-   حالة المنتج

### Slicers

-   Category
-   Product Line
-   Status

### KPI Cards

-   In Production
-   Average Dealer Price
-   Avg Profit Margin %
-   Out of Production
-   Avg Lifecycle (Months)

### Visuals

#### Profit Margin % by Category

Bar Chart أفقي للمقارنة بين الفئات.

#### Products by Status

Donut Chart:

-   In Production
-   Out of Production

#### Products by Price Tier

تحليل:

-   High
-   Medium
-   Low
-   Premium
-   No price

#### Top 5 Products by Profit Margin

جدول يحتوي على:

-   Product Name
-   Product Category
-   Average Profit Margin

ومن المنتجات الظاهرة في الصورة:

-   All-Purpose Bike Stand
-   Fender Set - Mountain
-   HL Mountain Tire
-   HL Road Tire
-   ML Mountain Tire

------------------------------------------------------------------------

# 20. صفحة Reseller Performance

### الهدف

تحليل أداء الموزعين من ناحية:

-   Sales
-   Profitability
-   Activity
-   Risk
-   Partnership Duration

### Slicers

-   Reseller Name
-   Category
-   Year

### KPI Cards

-   Total Reseller Sales
-   Active Resellers
-   Reseller Profit Margin %
-   Resellers at Risk
-   Average Years as Partner

### Visuals

#### Top 10 Resellers by Sales

Bar Chart أفقي.

#### Resellers Status

Donut Chart:

-   Active
-   At Risk
-   Inactive

النسب الظاهرة في الصورة:

-   Active: حوالي `69.76%`
-   At Risk: حوالي `20.83%`
-   Inactive: حوالي `9.42%`

#### Reseller Detail Table

الأعمدة:

-   ResellerName
-   ProductCategoryName
-   Sum of AnnualSales
-   Reseller Status
-   Avg Profit Margin %

تم استخدام Status Icons:

-   Active → أخضر
-   At Risk → تحذير
-   Inactive → أحمر

كما تظهر في الصفحة قيمة:

-   Active Resellers: `489`
-   Resellers at Risk: `146`

------------------------------------------------------------------------

# 21. صفحة Employees

### الهدف

تحليل:

-   عدد الموظفين
-   الإدارات
-   الرواتب
-   الإجازات
-   الفئات العمرية
-   أداء مندوبي المبيعات

### Slicers

-   Department
-   Gender
-   Age Group

### KPI Cards

-   Total Employees
-   Avg Annual Salary
-   Total Vacation Cost
-   Top Salesperson

### Visuals

#### Employees by Department

Bar Chart لتوزيع الموظفين حسب الإدارة.

وتظهر Production كأكبر إدارة في الصورة.

#### Age Group Distribution

Donut Chart.

#### Leave Pattern

Donut Chart يتضمن:

-   Vacation
-   Sick Leave
-   Balanced

#### Top Salesperson

Bar Chart لأفضل الموظفين في المبيعات.

#### Top 5 Employees by Sales

جدول يحتوي على:

-   Full Name
-   Employee Department
-   Total Reseller Sales

ومن الأسماء الظاهرة:

-   Jae B Pak
-   Jillian Carson
-   Linda C Mitchell
-   Michael G Blythe

وتظهر **Linda C Mitchell** كـ Top Salesperson في السياق المعروض.

### أمثلة على KPIs الظاهرة

-   Total Employees: `296`
-   Avg Annual Salary: `$38.7K`
-   Total Vacation Cost: `$269.1K`

------------------------------------------------------------------------

# 22. صفحة Customers & Territory

### الهدف

تحليل:

-   العملاء
-   المبيعات الجغرافية
-   المناطق
-   دخل العملاء
-   اكتساب العملاء

### Slicers

-   Year
-   Territory / Region
-   Gender

### KPI Cards

-   Total Customers
-   Total Internet Sales
-   Total Orders
-   Sales Growth Display
-   Top Territory

### Visuals

#### Sales by Country

Map Visual لتحليل المبيعات جغرافياً.

#### Top 10 Customers with the Highest Income

Bar Chart لأعلى العملاء من حيث Yearly Income.

#### Customer Acquisition Trend

تحليل اتجاه اكتساب العملاء عبر الزمن.

### أمثلة ظاهرة في التقرير

-   Total Customers: `101`
-   Top Territory: `United States`

------------------------------------------------------------------------

# 23. Financial Summary وCall Center

الصفحة الرئيسية تحتوي أيضاً على أقسام للتنقل إلى:

-   Financial Summary
-   Call Center

هذه الأقسام تمثل توسعاً في اتجاه **Sales & Operations Intelligence** بحيث
لا يقتصر المشروع على المبيعات فقط.

وبذلك تم تصميم الـ Navigation من البداية ليكون قابلاً للتوسع.

------------------------------------------------------------------------

# 24. Navigation

تم بناء Navigation يبدأ من الصفحة الرئيسية.

الهيكل:

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

كما تم وضع Home Button داخل الصفحات التفصيلية للعودة إلى الصفحة
الرئيسية.

------------------------------------------------------------------------

# 25. Slicers والفلاتر

تم وضع الـ Slicers في الجزء العلوي من الصفحات.

ومنها:

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

كما تم استخدام Clear Filter Control لمساعدة المستخدم على العودة إلى
الحالة الافتراضية.

------------------------------------------------------------------------

# 26. Top-N Analysis

تم استخدام Top-N بشكل متكرر.

أمثلة:

-   Top 10 Products
-   Top 10 Resellers
-   Top 5 Products by Profit Margin
-   Top 5 Employees by Sales
-   Top 10 Customers by Income

الهدف هو إعطاء الإدارة صورة سريعة عن العناصر الأعلى أداءً دون الحاجة لفحص
آلاف الصفوف.

------------------------------------------------------------------------

# 27. التصميم البصري

تم استخدام هوية بصرية موحدة في المشروع.

### أهم عناصر التصميم

-   لون Terracotta / Brown
-   خلفية فاتحة
-   Rounded Containers
-   Shadow Effects
-   عناوين واضحة
-   KPI Cards بيضاء
-   Icons
-   Navigation Buttons
-   Page Headers
-   Home Icon
-   Cards في أعلى الصفحة
-   Section Titles

تم استخدام Icons مرتبطة بالدراجات والمنتجات والعملاء والموظفين
والموزعين.

الهدف كان أن يبدو المشروع كمنتج BI متكامل وليس مجرد مجموعة Visuals
منفصلة.

------------------------------------------------------------------------

# 28. المشاكل التي تم حلها أثناء تطوير المشروع

## مشكلة Map Visual

في مرحلة بناء الخريطة لم يظهر الحقل المتوقع مثل Size.

تمت مراجعة نوع الـ Map Visual وحقول الـ Field Well المناسبة له بدلاً من
افتراض أن جميع أنواع الخرائط لها نفس الحقول.

------------------------------------------------------------------------

## مشكلة Tooltip

عندما لم تظهر بيانات Tooltip كما هو متوقع، تمت مراجعة:

-   Tooltip field
-   Visual formatting
-   Filter context
-   Measure result

------------------------------------------------------------------------

## مشكلة Card فوق Shapes

في بعض مراحل التصميم كان الـ Card يظهر خلف Shape.

تم استخدام:

-   Bring to Front
-   Send to Back
-   Selection Pane

لضبط ترتيب الطبقات.

------------------------------------------------------------------------

## مشكلة Category Label في Card

عندما لم يظهر Category Label، تمت مراجعة إعدادات الـ Card الخاصة بـ:

-   Category Label
-   Callout Value
-   Title
-   Alignment
-   Font Size

------------------------------------------------------------------------

## Full Screen

تمت مراجعة طريقة عرض التقرير في وضع العرض الكامل / Presentation View
لتسهيل مراجعة الـ Dashboard.

------------------------------------------------------------------------

## تنظيم الجداول

تمت مناقشة طريقة وضع عدة Tables داخل Folder.

الحل العملي:

-   إنشاء Measures Table
-   استخدام Display Folders للـ Measures
-   ترتيب الجداول في Model View
-   استخدام Naming Conventions واضحة

------------------------------------------------------------------------

# 29. Best Practices التي تم تطبيقها

## فصل Fact وDimensions

`FactInternetSales` يحتوي على معاملات المبيعات.

والـ Dimensions تصف البيانات.

## تجنب العلاقات المعقدة

تم الاعتماد على One-to-Many حيثما كان ذلك مناسباً.

## استخدام Measures

تم إنشاء الحسابات المهمة كـ Measures قابلة لإعادة الاستخدام.

## استخدام Dimensions للفلاتر

Year وCategory وProduct وCustomer وTerritory وغيرها تعتمد على حقول الـ
Dimensions.

## توحيد التصميم

تم الحفاظ على نفس الهوية البصرية عبر جميع الصفحات.

------------------------------------------------------------------------

# 30. دورة تنفيذ المشروع بالكامل

``` text
AdventureWorksDW2025.bak
        ↓
Restore في SQL Server
        ↓
اختيار الجداول
        ↓
الاتصال من Power BI
        ↓
Power Query
        ↓
Data Profiling
        ↓
تنظيف البيانات
        ↓
ضبط Data Types
        ↓
إزالة الأعمدة غير الضرورية
        ↓
التعامل مع Null / Errors / Duplicates
        ↓
Transformations
        ↓
تجهيز DimDate
        ↓
Data Model
        ↓
Relationships
        ↓
Calculated Columns
        ↓
DAX Measures
        ↓
KPIs
        ↓
Slicers & Filters
        ↓
Charts / Tables / Maps
        ↓
Navigation
        ↓
Formatting & Theme
        ↓
Testing & Troubleshooting
        ↓
Sales & Operations Intelligence Dashboard
```

------------------------------------------------------------------------

# 31. Quality Assurance

## Data

تمت مراجعة:

-   Row Counts
-   Data Types
-   Errors
-   Relationship Keys
-   Date Filtering

## Model

تمت مراجعة:

-   Cardinality
-   Relationship Direction
-   Active Date Relationship
-   Ambiguous Paths

## DAX

تمت مراجعة:

-   نتائج Measures
-   التعامل مع القسمة على صفر باستخدام DIVIDE
-   Time Intelligence
-   Filter Context

## Visuals

تمت مراجعة:

-   Titles
-   Currency Formatting
-   Percentage Formatting
-   Tables
-   Top-N
-   Slicers
-   Visual Interactions

## Navigation

تمت مراجعة:

-   Home Buttons
-   Navigation Tiles
-   Clear Filters
-   Page Consistency

------------------------------------------------------------------------

# 32. الأسئلة التجارية التي يستطيع المشروع الإجابة عليها

## Sales

-   كم إجمالي المبيعات؟
-   كم عدد الطلبات؟
-   كيف تتغير المبيعات؟
-   ما متوسط قيمة الطلب؟
-   ما الفئات الأعلى مبيعاً؟

## Products

-   ما المنتجات الأعلى مبيعاً؟
-   ما الفئات الأعلى ربحية؟
-   ما المنتجات التي ما زالت في الإنتاج؟
-   ما المنتجات الخارجة من الإنتاج؟
-   كيف تتوزع المنتجات حسب Price Tier؟

## Resellers

-   من أعلى الموزعين في المبيعات؟
-   كم عدد الموزعين النشطين؟
-   كم عدد الموزعين المعرضين للخطر؟
-   ما هامش ربح الموزعين؟
-   ما متوسط مدة الشراكة؟

## Employees

-   ما حجم كل إدارة؟
-   ما متوسط الراتب؟
-   من أفضل مندوبي المبيعات؟
-   كيف يتوزع الموظفون حسب العمر؟
-   كيف يبدو نمط الإجازات؟

## Customers & Territory

-   ما المناطق الأعلى مبيعاً؟
-   من العملاء الأعلى دخلاً؟
-   كيف يتغير اكتساب العملاء؟
-   ما العلاقة بين خصائص العملاء والمبيعات؟

------------------------------------------------------------------------

# 33. قيمة المشروع في الـ Portfolio

هذا المشروع يثبت مهارات عملية في:

-   SQL Server
-   Data Warehousing
-   Power Query
-   ETL
-   Data Cleaning
-   Dimensional Modeling
-   Star Schema
-   Data Relationships
-   DAX
-   Time Intelligence
-   KPI Development
-   Data Visualization
-   Business Analysis
-   Geographic Analysis
-   Dashboard UX
-   Interactive Reporting
-   Power BI Troubleshooting

ويمكن استخدامه كـ Portfolio Project للتقديم على وظائف مثل:

-   Data Analyst
-   BI Analyst
-   Power BI Developer
-   Junior BI Developer
-   Reporting Analyst
-   Business Intelligence Intern / Junior
-   IT / Technical Support مع إظهار مهارات تحليل البيانات

------------------------------------------------------------------------

# 34. هيكل المشروع المقترح

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

# 35. ملخص النتائج الظاهرة في Dashboard

حسب الصور المرفقة، تظهر أمثلة من النتائج الحالية:

-   Total Sales: حوالي **\$109.809M**
-   Total Orders: حوالي **31K**
-   Active Resellers: **489**
-   In Production: **197**
-   Out of Production: **200**
-   In Production: حوالي **49.62%**
-   Out of Production: حوالي **50.38%**
-   Resellers at Risk: **146**
-   Active Resellers: **489**
-   Active Resellers: حوالي **69.76%**
-   At Risk: حوالي **20.83%**
-   Inactive: حوالي **9.42%**
-   Total Employees: **296**
-   Average Annual Salary: **\$38.7K**
-   Total Vacation Cost: **\$269.1K**
-   Total Customers: **101**
-   Top Territory: **United States**

هذه القيم مرتبطة بالـ Filter Context الحالي، وقد تتغير عند تغيير الـ
Slicers.

------------------------------------------------------------------------

# 36. الخلاصة النهائية

مشروع **Apex Cycle Dynamics --- Sales & Operations Intelligence** لم يكن
مجرد Dashboard، بل كان دورة BI كاملة:

**Source → Clean → Model → Calculate → Analyze → Visualize → Navigate →
Validate**

بدأ المشروع من AdventureWorksDW2025، ثم:

1.  تم تجهيز SQL Server.
2.  تم اختيار الجداول الأساسية.
3.  تم ربط SQL Server مع Power BI.
4.  تم تنظيف البيانات في Power Query.
5.  تم تجهيز الـ Date Dimension.
6.  تم بناء Star Schema.
7.  تم إنشاء العلاقات.
8.  تم إنشاء Calculated Columns.
9.  تم بناء DAX Measures.
10. تم تصميم KPIs.
11. تم إنشاء Slicers.
12. تم بناء Charts وTables وMaps.
13. تم بناء صفحات التحليل.
14. تم إنشاء Navigation.
15. تم توحيد التصميم.
16. تم حل مشاكل Power BI المختلفة.
17. تم اختبار التقرير.
18. تم الوصول إلى Dashboard متكامل متعدد الصفحات.

النتيجة النهائية هي **Sales & Operations Intelligence Solution** قابلة
للعرض كـ Portfolio Project وتوضح القدرة على التعامل مع دورة حياة مشروع
Power BI من البيانات الخام وحتى المنتج التحليلي النهائي.

------------------------------------------------------------------------

## معلومات المشروع

**Project:** Apex Cycle Dynamics\
**Solution:** Sales & Operations Intelligence\
**Platform:** Microsoft Power BI\
**Source:** AdventureWorksDW2025\
**Main Fact:** FactInternetSales\
**Dimensions:** DimDate, DimCustomer, DimProduct, DimProductSubcategory,
DimProductCategory, DimSalesTerritory\
**Report Type:** Interactive Multi-Page BI Dashboard
