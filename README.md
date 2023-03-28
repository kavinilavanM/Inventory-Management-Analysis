# Inventory management data analysis

Inventory management data analysis refers to the process of using data to understand and optimize inventory levels, supply chain efficiency, and customer demand. This can involve analyzing data on inventory levels, sales trends, lead times, supplier performance, and customer behavior.

Inventory analysis is essential for businesses to manage their inventory effectively and efficiently. By analyzing inventory levels, demand patterns, and supply chain performance, businesses can identify potential risks and challenges and take proactive measures to prevent stockouts or overstocking.Effective inventory analysis also helps businesses to optimize their supply chain operations. By identifying inefficiencies and bottlenecks in the supply chain, businesses can make improvements to increase efficiency, reduce costs, and improve customer satisfaction.Furthermore, inventory analysis can help businesses to increase profitability by minimizing inventory holding costs, reducing stockouts, and optimizing product pricing and promotions based on demand patterns.

### 1. ABC analysis
ABC analysis is a method that helps businesses categorize their inventory based on the Pareto principle, also known as the 80/20 rule. This principle states that roughly 80% of the effects come from 20% of the causes. In the context of inventory management, it means that a small percentage of the products in your inventory account for a large percentage of your sales and profits.

By performing an ABC analysis, businesses can identify their most valuable products (A-items) and allocate more resources to managing them, such as optimizing their inventory levels, monitoring their demand closely, and improving their supply chain processes. In contrast, they can also identify their least valuable products (C-items) and consider reducing their inventory levels or discontinuing them altogether.

To calculate the annual usage value per product, you can use the following formula:

*(Number of items sold) x (Cost per item) = Annual usage value per product*

Once you have calculated the annual usage value for each product in your inventory, you can sort them in descending order and assign them to the appropriate category based on their cumulative usage value. The top 20% of the products with the highest cumulative usage value will be classified as A-items, the next 30% will be classified as B-items, and the remaining 50% will be classified as C-items.

### 2. HML analysis
HML analysis is a simple method of inventory analysis that can be useful in identifying inventory items that require special attention due to their high cost, potential profit margin, or demand. By sorting inventory into categories based on price per unit, businesses can prioritize their efforts and resources towards managing the inventory that is most critical to their operations.
For example, high-priced items may require extra security measures or more frequent inventory checks to prevent theft or loss. Meanwhile, low-priced items may not require as much attention since they have less impact on overall inventory value and profitability.
H: Inventory with a high price per unit.
M: Inventory with a medium price per unit.
L: Inventory with a low price per unit.
HML analysis can also be combined with other inventory analysis methods, such as ABC analysis, to provide a more comprehensive understanding of inventory performance and needs. By using multiple methods, businesses can identify patterns and trends in their inventory and make more informed decisions about inventory management strategies.

### 3. VED analysis
VED analysis helps businesses prioritize their inventory based on their importance and impact on the overall operations. By categorizing inventory as vital, essential, or desirable, businesses can allocate resources and manage inventory levels more effectively. For example, a business may want to focus on keeping its vital inventory in stock at all times, while only ordering desirable inventory when necessary to avoid overstocking. Additionally, VED analysis can help businesses identify potential risks and areas for improvement in their inventory management, such as identifying items that may need to be reordered more frequently to maintain adequate stock levels.
V: Vital inventory: products needed constantly for business operations
E: Essential inventory: important products in minimal quantities
D: Desirable inventory: optional products for the business

### 4. SDE analysis
SDE analysis is a method of categorizing inventory based on its availability and lead time:
Scarce inventory is imported and has a longer lead time, making it harder to acquire. This inventory is critical to the business and its scarcity can lead to delays and disruptions in operations.
Difficult inventory is still important to the business, but it requires more effort and time to secure. This inventory has a lead time of over 14 days but less than 6 months.
Easily available inventory is readily accessible and can be quickly obtained. This inventory is usually less critical to the business and can be easily replaced or substituted.

### 5. Safety stock analysis
Safety stock analysis is a method of calculating the amount of additional inventory that should be held in reserve to prevent stockouts. This analysis takes into account the lead time for replenishing inventory, the expected demand during that lead time, and the desired level of customer service.

## Requirements and Problem Statements for this Projects 

1. ABC Analysis
2. Inventory Turn Over Ratio 
3. Safety Stock Level 
4. Reorder Level
5. Stock Status
6. VED Analysis 

## DAX Formula

### ABC Analysis 

Annual Sales Quantity 2020 = CALCULATE(
    SUM('Past Orders'[Order Quantity]),
    FILTER('Past Orders',
    'Past Orders'[SKU ID] ='Stock'[SKU ID] &&
    'Past Orders'[Date].[Year] = 2020))
    
Revenue Share % = 100*'Stock'[Annual Revenue]/SUM('Stock'[Annual Revenue])+0

Cumulative Shares = CALCULATE(
    SUM('Stock'[Revenue Share %]),
    FILTER(Stock,
    Stock[Revenue Share %] >=EARLIER(Stock[Revenue Share %])))
    
ABC Analysis = IF(Stock[Cumulative Shares]<=70,"A [High]", IF(Stock[Cumulative Shares]<=90,"B [Medium]","C [Less]"))

ABC Rank = RANK.EQ(Stock[Cumulative Shares],Stock[Cumulative Shares],ASC)

![Parito Example of ABC Analysis](https://github.com/kavinilavanM/Inventory-Management-Analysis/blob/main/Parito%20example-%20ABC%20Analysis.png)

### Inventory Turn Over Ratio

A ratio that measures the number of times inventory is sold and replaced over a period of time. It's calculated as Cost of Goods Sold (COGS) divided by Average Inventory.

Value in WH = Stock[Current Stock Quantity]*Stock[Unit Price]

Inventory TurnOver Ratio = SUM(Stock[Annual Revenue])/SUM(Stock[Value in WH])


![INVENTORY TURNOVER RATIO](https://github.com/kavinilavanM/Inventory-Management-Analysis/blob/main/Inventory%20turn%20over%20ratio.png)

### Safety Stock Level 

Peak Weakly Demands = CALCULATE(
    MAX('Weekly Demond Sheet'[Weekly Demonds]),
    Filter('Weekly Demond Sheet',
    'Weekly Demond Sheet'[SKU ID]=Stock[SKU ID]))
    
 Average Weekly Demonds = CALCULATE(
    AVERAGE('Weekly Demond Sheet'[Weekly Demonds]),
    FILTER('Weekly Demond Sheet',
    'Weekly Demond Sheet'[SKU ID]=Stock[SKU ID]))
    
 Safety Stock = (Stock[Peak Weakly Demands] * Stock[Maximum Lead Time (days)]/7)-(Stock[Average Weekly Demonds] * Stock[Average Lead Time (days)]/7)
 


#### Reorder Level 

ReOrder Points = Stock[Safety Stock] +(Stock[Average Weekly Demonds] * Stock[Average Lead Time (days)]/7)

#### Stock Status 

Stock Status = IF(Stock[Current Stock Quantity] = 0, "Out of Stock",IF(Stock[Need for Order]= "YES","Below Reorder Points","In Stock"))

![](https://github.com/kavinilavanM/Inventory-Management-Analysis/blob/main/Stock%20Status.png)

#### VED Analysis

CV Rank = RANK.EQ(Stock[coefficient of variation],Stock[coefficient of variation],1)

XYZ ? = if(Stock[CV Rank]<=0.2*MAX(Stock[CV Rank]),"X [uniform Demond]", if(Stock[CV Rank]<=0.5*MAX(Stock[CV Rank]),"Y [Variable Demond]",
" Z [Uncertain Demonds]"))

#### Dashboard Preview 
![Dashboard](https://github.com/kavinilavanM/Inventory-Management-Analysis/blob/main/Screenshot%202023-03-28%20074259.png)

