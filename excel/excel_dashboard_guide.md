# Excel Zomato Delivery Analytics Guide
## Real Scraped Dataset Architecture & Analytical Formulas
**Workbook File:** `zomato_delivery_analytics_workbook.xlsx`  
**Master Orders:** `zomato_cleaned_orders_master.csv`  
**Real Restaurant Catalog:** `zomato_real_restaurants_catalog.csv`

---

## 1. Sheet Architecture

| Sheet Name | Content & Purpose | Structure |
|---|---|---|
| `Zomato_Orders_Master` | 1,800 transactional order records mapped to real scraped Zomato restaurants | Formatted Excel Table (`Ctrl + T`) with formulas, currency formatting (₹), and status flags |
| `Scraped_Zomato_Restaurants` | 106 live scraped restaurants from Zomato across Bengaluru, Mumbai, and Delhi NCR | Authentic restaurant IDs, real ratings, costs for two (₹), localities, promo tags, and URLs |
| `Cuisine_Performance` | Cuisine-level aggregation of GMV, AOV, Delivery Duration, and Take Rate | Summary Pivot Table & Margin Analysis |
| `Hub_Logistics` | 12 Metro Hub logistics breakdown | Distance vs Delay % vs Customer Rating |

---

## 2. Senior Analyst Excel Formulas Used

### A. Net Customer Payable (Unit Economics)
```excel
=MAX(60, [@[Gross_Order_Value_INR]] - [@[Discount_Amount_INR]] + [@[Delivery_Fee_INR]] + [@[Platform_Fee_INR]])
```
*Business Rule:* Enforces a platform basket floor of ₹60 while applying real scraped Zomato promo discounts and fees.

### B. SLA Delay Evaluation
```excel
=MAX(0, [@[Actual_Delivery_Mins]] - [@[Promised_SLA_Mins]])
```

### C. Zomato Platform Commission Revenue
```excel
=IF([@[Cost_For_Two_INR]] >= 1000, [@[Gross_Order_Value_INR]] * 0.22, [@[Gross_Order_Value_INR]] * 0.18)
```
*Business Rule:* Premium high-ticket dining brands pay a standard 22% tier, while QSR/mass categories pay 18%.

### D. Multi-Condition Summary Metrics for Executive Dashboard
```excel
' 1. Total Biryani GMV in Koramangala Hub:
=SUMIFS(Zomato_Orders_Master[Gross_Order_Value_INR], Zomato_Orders_Master[Zomato_Hub], "Koramangala", Zomato_Orders_Master[Primary_Cuisine], "Biryani")

' 2. Average Customer Rating for Orders Delivered Under 25 Minutes:
=AVERAGEIFS(Zomato_Orders_Master[Customer_Rating], Zomato_Orders_Master[Actual_Delivery_Mins], "<=25")

' 3. Count of Orders Receiving 50%+ Discount Promos:
=COUNTIF(Zomato_Orders_Master[Active_Zomato_Offer], "*50%*")
```

---

## 3. Pivot Tables & Executive Charts Setup
1. **Pivot Table 1: Cuisine GMV & AOV Matrix**
   - **Rows:** `Primary_Cuisine`
   - **Values:** Count of `Order_ID`, Sum of `Gross_Order_Value_INR`, Average of `Gross_Order_Value_INR`
   - **Chart:** Clustered Column Chart with Secondary Line Axis for AOV.
2. **Pivot Table 2: 12 Hub Logistics Comparison**
   - **Rows:** `Zomato_Hub`
   - **Values:** Average of `Distance_KM`, Average of `Actual_Delivery_Mins`, Average of `Customer_Rating`
   - **Conditional Formatting:** Green-Yellow-Red 3-color gradient on Delivery Duration.
