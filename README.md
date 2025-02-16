# Data Portfolio: Excel to Power BI
![cover-photo](assets/images/kaggle_to_powerbi.gif)


## **1. Data Extraction from SQL Server**
To extract relevant YouTube data, we performed several **SQL transformations**:

- **Extracting YouTube Channel Names**  
  Used `SUBSTRING()` and `CHARINDEX()` to isolate channel names from emails.
  
  Step 1: List all rows for the selected columns needed
  ![Necessary-columns](assets/images/1_All_columns_needed.png)

  Step 2: We get Rid of the unecessary text after the "@" symbol and cast our strings
  ![Cast-String_name](assets/images/2_cast_string_channel_name.png)
  
  ### *Data Quality Check (Most Important Aspect)*
  - **Data Record Count Check** – Ensuring data has 100 records.
    ![Record-count](assets/images/3_Data_record_check.png)
    
  - **Column Count Check** – Verified the correct structure using `INFORMATION_SCHEMA`.
    ![Column-Count](assets/images/4_Column_count_check.png)
    
  - **Data Type Check** – Ensuring the right data type for names in our columns.
    ![Data-Type](assets/images/5_Data_type_check.png)
    
  - **Duplicate Removal Check** – We make sure there aren't any duplicate records.
    ![Duplicate-Check](assets/images/5_Duplicate_record_check.png)
  
  - ✅**Created a SQL View** that serves as the source for Power BI.
    ![SQL-View](assets/images/6_View_created.png)

---

## **2. Power BI Visualizations**
After data extraction, we imported the cleaned dataset into **Power BI** to create interactive dashboards.  

### *DAX Expressions*
This section includes key DAX expressions used in the Power BI reports to calculate metrics such as total views, engagement rates, and revenue estimates.

  - **Total Subscribers in Millions**
    ```DAX
    Total Subscribers (M) = 
    VAR million = 1000000
    VAR sumOfSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
    VAR totalSubscribers = DIVIDE(sumOfSubscribers,million)

    RETURN totalSubscribers
    ```

  - **Total Views in Billions**
    ```DAX
    Total Views (B) = 
    VAR billion = 1000000000
    VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
    VAR totalViews = ROUND(sumOfTotalViews / billion, 2)

    RETURN totalViews
    ```

  - **Total Videos Uploaded**
    ```DAX
    Total Videos = 
    VAR totalVideos = SUM(view_uk_youtubers_2024[total_videos])

    RETURN totalVideos
    ```


  - **Average Views Per Video in Millions(how many views each video generates)**
    ```DAX
    Average Views per Video (M) = 
    VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
    VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
    VAR  avgViewsPerVideo = DIVIDE(sumOfTotalViews,sumOfTotalVideos, BLANK())
    VAR finalAvgViewsPerVideo = DIVIDE(avgViewsPerVideo, 1000000, BLANK())

    RETURN finalAvgViewsPerVideo
    ``` 


  - **Subscriber Engagement Rate(How often subscribers engage with content)**
    ```DAX
    Subscriber Engagement Rate = 
    VAR sumOfTotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
    VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
    VAR subscriberEngRate = DIVIDE(sumOfTotalSubscribers, sumOfTotalVideos, BLANK())

    RETURN subscriberEngRate
    ```


  - **Views Per Subscriber**
    ```DAX
    Views Per Subscriber = 
    VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
    VAR sumOfTotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
    VAR viewsPerSubscriber = DIVIDE(sumOfTotalViews, sumOfTotalSubscribers, BLANK())

    RETURN viewsPerSubscriber
    ``` 


### *Key Visual type Designs*:
- **Table View**: Displays all available YouTube data.
  ![Table](assets/images/Table_View.png)
  
- **Treemap**: Shows **Top 20 YouTube Channels** by total views.
  ![Treemap](assets/images/Treemap_View.png)
  
- **Scorecard**: Highlights key statistics such as **Total Subs, Total Views, and Engagement Rate**.
  ![Scorecard](assets/images/Scorecard_View.png)
  
- **Horizontal Bar Chart**: Displays **Top 10 Channels by Subscribers**.
  ![Bar-Chart](assets/images/Horizontal__Bar_View.png)

🎨 **Dashboard Formatting & UX Improvements**:
- Renamed column headers for clarity and Adjusted X/Y-axis ranges.
  ![Format-Table](assets/images/Adjust_XY_Axis.png)
  
- Applied conditional formatting with YouTube-themed colors.
  ![Color-Scheme](assets/images/Color_Scheme_youtube.png)
  
- Enabled cross-filtering – clicking on a channel filters the entire dashboard.
  ![Dashboard-gif](assets/images/top_uk_youtubers_2024.gif)

### *What we have discovered* :
 With further analysis into the dashboard we have determined that the top three youtube channels with the most subscribers are NoCopyrightSounds with 33.6M subscribers, DanTDM with 28.6M, and Dan Rhodes with 26.5M. With these channels we are going to use the Avg views per video (in millions) from each of the three channels to further our analysis for our client.
 
---

## **3. Excel Data Analysis**
To further analyze **business insights**, we imported Power BI results into Excel.

**Metrics Computed:**
- **Conversion Rate**: Measures how many viewers purchase a product after watching a video. In this case we are going to say 2% of viewers purchased the product
- **Campaign Cost**: Amount spent per influencer. We are going to say $50,000 for each influencer
- **Potential Product Sales per Video**: How many products we can sell knowing how much views they average per video. (Avg. views per video) × (Conversion rate).
- **Potential Revenue per Video**:How much money we can make (Product Sales) × (Product Cost).
- **Net Profit**: (Potential Revenue) - (Campaign Cost).
  ![excel-table](assets/images/1_excel_table_wo_formatting.png)

### *Calculation Breakdown for the top three subscribed channels*
    1. NoCopyrightSounds
       - Average views per video = 6.92 million
       - Product cost = $5
       - Potential units sold per video = 6.92 million x 2% conversion rate = 138,400 units sold
       - Potential revenue per video = 138,400 x $5 = $692,000
       - Campaign cost (one-time fee) = $50,000
       - Net profit = $692,000 - $50,000 = $642,000

    2. DanTDM
      - Average views per video = 5.34 million
      - Product cost = $5
      - Potential units sold per video = 5.34 million x 2% conversion rate = 106,800 units sold
      - Potential revenue per video = 106,800 x $5 = 534,000
      - Campaign cost (one-time fee) = $50,000
      - Net profit = $534,000 - $50,000 = $484,000

    3. Dan Rhodes
      - Average views per video = 11.15 million
      - Product cost = $5
      - Potential units sold per video = 11.15 million x 2% conversion rate = 223,000 units sold
      - Potential revenue per video = 223,000 x $5 = $1,115,000
      - Campaign cost (one-time fee) = $50,000
      - Net profit = $1,115,000 - $50,000 = $1,065,000


✅ **Excel Formatting Enhancements**:
- Shortcuts for efficient formatting (e.g., `ALT + H + M + C` to center text).
- Conditional formatting applied to highlight revenue trends.
  ![table-wth-formatting](assets/images/2_excel_table_wth_formatting.png)
  

---

## **4. SQL & Excel Cross-Validation**
📊 Final validation step:  Now the same outputs we determined in Excel will now be determined using SQL

### *SQL Query*
```SQL
/*

1. Define the variables
2. Create a CTE (round the average views per video column, potential units sold, potential revenue per video, net_profit)
3. Select columns that are appropriate for the analysis
4. Filter the results by youtube channels with highest subscriber bases
5. Order by net_profit (from highest to lowest)


*/

--1.
DECLARE @conversionRate FLOAT = 0.02;		-- The conversion rate at 2%
DECLARE @productCost MONEY = 5.0;			-- The product cost at $5
DECLARE @campaignCost MONEY = 50000.0;	-- The campaign cost at $50,000


--2 and 3.
WITH ChannelData AS (
	SELECT 
		channel_name,
		total_views,
		total_videos,
		ROUND((CAST(total_views AS FLOAT) / total_videos), -4) AS rounded_avg_views_per_video
	FROM 
		view_uk_youtubers_2024
)

SELECT 
	channel_name,
	rounded_avg_views_per_video,
	(rounded_avg_views_per_video * @conversionRate) AS potential_units_sold_per_video,
	(rounded_avg_views_per_video * @conversionRate * @productCost) AS potential_revenue_per_video,
	(rounded_avg_views_per_video * @conversionRate * @productCost) - @campaignCost AS net_profit
FROM 
	ChannelData

--4.
WHERE 
	channel_name IN ('NoCopyrightSounds', 'DanTDM', 'Dan Rhodes')

--5.
ORDER BY net_profit DESC
```

### *Output*
![SQL-Output](assets/images/SQL_validation.png)


- Compared **Excel outputs with SQL results** for consistency.



---


## **Conclusion**
This project provided a **data-driven approach** to analyzing YouTube channels. By integrating **SSMS, Power BI, and Excel**, we gained actionable insights into subscriber engagement and revenue potential.

### **🚀 Technologies Used**
- SQL Server (SSMS)  
- Power BI  
- Microsoft Excel  

---

## **Author**
📌 Developed by [Musa Ceesay]  

---
