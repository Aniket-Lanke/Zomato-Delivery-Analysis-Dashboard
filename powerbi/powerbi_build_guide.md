# Step-by-Step Power BI Canvas Build Guide
## Project: Zomato Food Delivery Operations & Revenue Intelligence

Follow these exact steps in **Power BI Desktop** to build the enterprise dashboard:

---

### Step 1: Load Data into Power BI
1. Open **Power BI Desktop**.
2. Click **Get Data** -> **Text/CSV**.
3. Select `data/zomato_cleaned_orders_master.csv` -> Click **Transform Data** (Power Query).
4. Verify data types:
   - `Order_Date` -> Date
   - `Gross_Order_Value_INR`, `Platform_Net_Profit_INR`, `Customer_Rating` -> Decimal Number
   - `Kitchen_Prep_Mins`, `Rider_Transit_Mins`, `Actual_Delivery_Mins` -> Whole Number
5. Click **Close & Apply**.
6. Repeat for `data/zomato_real_restaurants_catalog.csv` (Dim Table).

---

### Step 2: Establish the Data Model (Star Schema)
1. Switch to the **Model View** (left pane).
2. Drag `Restaurant_ID` from `zomato_real_restaurants_catalog` to `Restaurant_ID` in `zomato_cleaned_orders_master`.
3. Set cardinality to **1 to Many (1:*)** with single direction filtering.

---

### Step 3: Create DAX Measures Table
1. On the Home ribbon, click **Enter Data**.
2. Name the table `_Measures` and click **Load**.
3. Create the 12 DAX measures listed in `powerbi/powerbi_dax_and_data_model.md`:
   - `Gross Merchandise Value (INR)`
   - `Total Orders`
   - `Average Order Value (AOV)`
   - `Platform Take Rate %`
   - `On-Time SLA Rate %`
   - `Average CSAT Rating`

---

### Step 4: Canvas Layout & Theme
1. **Background:** Dark theme (`#0F172A`) or clean light canvas.
2. **Accent Colors:** Zomato Red (`#E23744`), Emerald Green (`#10B981`), Amber (`#F59E0B`).

#### Top KPI Cards (Row 1):
- **Card 1:** `Gross Merchandise Value (INR)` (Display unit: Millions or Thousands, Prefix: `₹`).
- **Card 2:** `Total Orders` (1,800).
- **Card 3:** `Average Order Value (AOV)` (`₹777`).
- **Card 4:** `On-Time SLA Rate %` (`89.2%`).
- **Card 5:** `Platform Take Rate %` (`14.19%`).
- **Card 6:** `Average CSAT Rating` (`4.58 ★`).

#### Visual 1 (Center-Left):
- **Type:** Clustered Column Chart.
- **X-Axis:** `Primary_Cuisine`.
- **Y-Axis:** `Gross Merchandise Value (INR)`.
- **Tooltip:** `Average Order Value (AOV)`, `Total Orders`.

#### Visual 2 (Center-Right):
- **Type:** Scatter Chart / Bubble Plot.
- **X-Axis:** `Actual_Delivery_Mins`.
- **Y-Axis:** `Customer_Rating`.
- **Details:** `Restaurant_Name`.
- **Legend:** `Delivery_Status` (On-Time, Slight Delay, Major Delay).

#### Visual 3 (Bottom):
- **Type:** Table / Matrix Visual.
- **Columns:** `Restaurant_Name`, `City`, `Zomato_Hub`, `Cost_For_Two_INR`, `Zomato_Delivery_Rating`, `Active_Zomato_Offer`.
- **Formatting:** Data bars on GMV, Conditional formatting on ratings.
