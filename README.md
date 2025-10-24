# Indian Kids Screen Time Dashboard

This project is a 3-page interactive Power BI dashboard that analyzes the screen time habits of children in India. It explores key trends, device preferences, and the powerful correlation between screen time and health impacts.

This dashboard was built from a raw dataset and includes all data cleaning, transformation, and visualization steps.

## Dashboard Overview (Page 1)

The first page serves as a high-level executive summary, answering the biggest questions at a glance.

<img width="1323" height="743" alt="image" src="https://github.com/user-attachments/assets/0e1bba57-b542-43ba-a6be-b8d2a0c25eb8" />

### Key Features:
* **KPI Cards:** Total participants, overall average screen time, top user age group, and the most common device.
* **Health Status Breakdown:** A bar chart showing the count of health impacts (Mental, Physical, etc.) across the dataset.
* **Device Share:** A donut chart showing the market share of Smartphones, TVs, Laptops, and Tablets.
* **Top Users Analysis:** A clustered bar chart comparing average screen time by age group and gender.

## Age & Health Deep Dive (Page 2)

The second page is a deep dive that tells a powerful story about *how* screen time habits change with age and what the consequences are.

<img width="1325" height="746" alt="image" src="https://github.com/user-attachments/assets/5568d4ec-c8a6-4aa8-83cb-8ee5f79421b4" />

### Key Features:
* **Age vs. Screen Time:** A line chart showing that average screen time **jumps up** after age 10.
* **Age vs. Content Quality:** An area chart showing that as screen time increases, the educational-to-recreational ratio **plummets**.
* **Impact Analysis:** Bar charts that clearly show:
    * Kids who exceed the recommended limit have significantly higher average screen time.
    * Kids who report health impacts like "Poor Sleep" have the highest average screen time.

## Key Insights & Conclusion (Page 3)

The final page summarizes the entire report into four main takeaways, presented in a clean 2x2 grid.

<img width="1321" height="741" alt="image" src="https://github.com/user-attachments/assets/d7a7e0fc-6cb8-420e-a36c-dee89962bf0a" />

### The 4 Key Insights:
1.  **Excessive Time is the Norm:** The average screen time is **4.37 hours**, with **85% of kids** exceeding the 2-hour recommended limit.
2.  **Quality Plummets with Age:** As kids get older, screen time not only increases, but its quality becomes almost purely recreational.
3.  **Portable Devices are the Driver:** Smartphones are the #1 device (47%). Portable devices (74% of all use) are strongly linked to higher screen time.
4.  **Clear Link to Health Impacts:** A clear correlation exists where kids with reported health issues (like Poor Sleep) have a significantly higher average screen time.

## Tools Used

* **Power BI Desktop:** Used for all data modeling, analysis, and visualization.
* **Power Query (M):** Used for all data cleaning and transformation.
* **DAX:** Used to create custom measures for KPI cards and filters.

## Data Cleaning & Transformation

Before visualization, the data was heavily processed in Jupyter Notebook & Power Query to make it usable:

* **Handled Missing Values:** Replaced `null` values in `Health_Impacts` with "No Health Impacts" for clarity.
* **Corrected Data Types:**
    * Changed `Age` from text to a **Whole Number** (this was critical for sorting the line charts correctly).
    * Changed `Educational_to_Entertainment_Ratio` from text to a **Decimal Number** to allow for averaging.
* **Feature Engineering (New Columns):**
    * **Age Bands:** Created (Pre-Teens, Teenagers, Late Teens) from the `Age` column.
    * **Device Type:** Created (Portable, Fixed) from the `Primary_Device` column.
    * **Screen_Size:** Created (`<30"` or `>=30"`) based on the device type.
    * **Health Status:** Grouped specific health impacts into (Mental, Physical, Mental-Physical).

## How to View This Project

1.  **Download** the `.pbix` file from this repository [Folder_name: Final_Dashboarb].
2.  **Open** the file using Power BI Desktop. 
