# Pandas, Pandas, Pandas


## Heroes of Pymoli - Background

![](Images/Fantasy.jpg)

After landing a job as Lead Analyst for an independent gaming company, you've been assigned the task of analyzing the data for their most recent fantasy game Heroes of Pymoli.

Like many others in its genre, the game is free-to-play, but players are encouraged to purchase optional items that enhance their playing experience. As a first task, the company would like you to generate a report that breaks down the game's purchasing data into meaningful insights.


## Objectives

The final report should include each of the following:

### Player Count
* Total Number of Players

```python
# Total Number of Players
player_count = len(purchase_data["SN"].unique())
player_count

# Create Summary DataFrame
player_count_table = pd.DataFrame({"Total Players": [player_count]})
player_count_table
```
![](Images/Player%20Count.png)

### Purchasing Analysis (Total)
* Number of Unique Items
* Average Purchase Price
* Total Number of Purchases
* Total Revenue

```python
![](Images/Purchasing%20Anaysis%20(Total).png)

### Gender Demographics
* Percentage and Count of Male Players
* Percentage and Count of Female Players
* Percentage and Count of Other / Non-Disclosed

```python
# Count & Percentage of Male Players
male_players = purchase_data.loc[purchase_data["Gender"] == "Male"]
male_count = len(male_players["SN"].unique())
male_percent = "{:.2f}%".format(male_count / player_count * 100)

# Count & Percentage of Female Players
female_players = purchase_data.loc[purchase_data["Gender"] == "Female"]
female_count = len(female_players["SN"].unique())
female_percent = "{:.2f}%".format(female_count / player_count * 100)

# Count & Percentage of Other / Non-Disclosed
other_players = purchase_data.loc[purchase_data["Gender"] == "Other / Non-Disclosed"]
other_count = len(other_players["SN"].unique())
other_percent = "{:.2f}%".format(other_count / player_count * 100)

# Create Summary DataFrame
gender_demographics_table = pd.DataFrame([{
    "Gender": "Male", "Total Count": male_count, 
    "Percentage of Players": male_percent}, 
    {"Gender": "Female", "Total Count": female_count, 
     "Percentage of Players": female_percent}, 
    {"Gender": "Other / Non-Disclosed", "Total Count": other_count, 
     "Percentage of Players": other_percent
    }], columns=["Gender", "Total Count", "Percentage of Players"])

gender_demographics_table = gender_demographics_table.set_index("Gender")
gender_demographics_table.index.name = None
gender_demographics_table
```
![](Images/Gender%20Demographics.png)

### Purchasing Analysis (Gender)
* The below each broken by gender
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
  * Average Purchase Total per Person by Gender
  
```python
# Purchase Count of Male Players
male_purchase_data = purchase_data.loc[purchase_data["Gender"] == "Male", :]
male_purchase_count = len(male_purchase_data)

# Average Purchase Price of Male Players
avg_male_purchase_price = purchase_data.loc[purchase_data["Gender"] == "Male", ["Price"]].mean()

# Total Purchase Value of Male Players
total_male_purchase_value = purchase_data.loc[purchase_data["Gender"] == "Male", ["Price"]].sum()

# Purchase Count of Female Players
female_purchase_data = purchase_data.loc[purchase_data["Gender"] == "Female", :]
female_purchase_count = len(female_purchase_data)

# Average Purchase Price of Female Players
avg_female_purchase_price = purchase_data.loc[purchase_data["Gender"] == "Female", ["Price"]].mean()

# Total Purchase Value of Female Players
total_female_purchase_value = purchase_data.loc[purchase_data["Gender"] == "Female", ["Price"]].sum()

# Purchase Count of Other / Non-Disclosed Players
other_purchase_data = purchase_data.loc[purchase_data["Gender"] == "Other / Non-Disclosed", :]
other_purchase_count = len(other_purchase_data)

# Average Purchase Price of Other / Non-Disclosed Players
avg_other_purchase_price = purchase_data.loc[purchase_data["Gender"] == "Other / Non-Disclosed", ["Price"]].mean()

# Total Purchase Value of Other / Non-Disclosed Players
total_other_purchase_value = purchase_data.loc[purchase_data["Gender"] == "Other / Non-Disclosed", ["Price"]].sum()

# Average Purchase Total per Person by Gender
avg_male_purchase_total_person = total_male_purchase_value / male_count
avg_female_purchase_total_person = total_female_purchase_value / female_count
avg_other_purchase_total_person = total_other_purchase_value / other_count

# Create Summary DataFrame
gender_purchasing_analysis_table = pd.DataFrame([{
    "Gender": "Female", "Purchase Count": female_purchase_count, 
    "Average Purchase Price": "${:.2f}".format(avg_female_purchase_price[0]), 
    "Total Purchase Value": "${:.2f}".format(total_female_purchase_value[0]), 
    "Avg Total Purchase per Person": "${:.2f}".format(avg_female_purchase_total_person[0])}, 
    {"Gender": "Male", "Purchase Count": male_purchase_count, 
     "Average Purchase Price": "${:.2f}".format(avg_male_purchase_price[0]), 
     "Total Purchase Value": "${:,.2f}".format(total_male_purchase_value[0]), 
     "Avg Total Purchase per Person": "${:.2f}".format(avg_male_purchase_total_person[0])}, 
    {"Gender": "Other / Non-Disclosed", "Purchase Count": other_purchase_count, 
     "Average Purchase Price": "${:.2f}".format(avg_other_purchase_price[0]), 
     "Total Purchase Value": "${:.2f}".format(total_other_purchase_value[0]), 
     "Avg Total Purchase per Person": "${:.2f}".format(avg_other_purchase_total_person[0])
    }], columns=["Gender", "Purchase Count", "Average Purchase Price", "Total Purchase Value", "Avg Total Purchase per Person"])

gender_purchasing_analysis_table = gender_purchasing_analysis_table.set_index("Gender")
gender_purchasing_analysis_table.index.name = None
gender_purchasing_analysis_table
```
![](Images/Purchasing%20Analysis%20(Gender).png)

### Age Demographics
* The below each broken into bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.)
  * Total Count
  * Percentage of Players
 
 ```python
 # Figure Out Minimum and Maximum Ages
# print(purchase_data["Age"].max())
# print(purchase_data["Age"].min())

# Establish Bins for Ages & Create Corresponding Names For Bins
age_bins = [0, 9, 14, 19, 24, 29, 34, 39, 46]
groups_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

# Place Data Series Into New Column Inside DataFrame
purchase_data["Age Group"] = pd.cut(purchase_data["Age"], bins=age_bins, labels=groups_names)
purchase_data

# Create a GroupBy Object Based Upon "Age Group"
age_group = purchase_data.groupby("Age Group")

# Count Total Players by Age Category
total_count_age = age_group["SN"].nunique()

# Calculate Percentages by Age Category 
percentage_by_age = round(total_count_age / player_count * 100,2)

# Create Summary DataFrame
age_demographics_table = pd.DataFrame({
    "Total Count": total_count_age, 
    "Percentage of Players": percentage_by_age
})

age_demographics_table["Percentage of Players"] = age_demographics_table["Percentage of Players"].map("{0:,.2f}%".format)
age_demographics_table.index.name = None
age_demographics_table
```
![](Images/Age%20Demographics.png)

### Purchasing Analysis (Age)
* The below each broken into bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.)
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
  * Average Purchase Total per Person by Age Group

```python
# Establish Bins for Ages & Create Corresponding Names For The Bins
bins = [0, 9, 14, 19, 24, 29, 34, 39, 46]
groups_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

# Place Data Series Into New Column Inside DataFrame
purchase_data["Age Group"] = pd.cut(purchase_data["Age"], bins=age_bins, labels=groups_names)

# Calculate "Purchase Count"
age_purchase_count = age_group["SN"].count()

# Calculate "Average Purchase Price"
avg_age_purchase_price = round(age_group["Price"].mean(),2)

# Calculate "Total Purchase Value"
total_age_purchase_value = round(age_group["Price"].sum(),2)

# Calculate "Avg Total Purchase per Person"
avg_total_age_purchase_person = round(total_age_purchase_value / total_count_age,2)

# Create Summary DataFrame
age_purchasing_analysis_table = pd.DataFrame({
    "Purchase Count": age_purchase_count, 
    "Average Purchase Price": avg_age_purchase_price,
    "Total Purchase Value": total_age_purchase_value,
    "Avg Total Purchase per Person": avg_total_age_purchase_person
})

age_purchasing_analysis_table["Average Purchase Price"] = age_purchasing_analysis_table["Average Purchase Price"].map("${0:,.2f}".format)
age_purchasing_analysis_table["Total Purchase Value"] = age_purchasing_analysis_table["Total Purchase Value"].map("${0:,.2f}".format)
age_purchasing_analysis_table["Avg Total Purchase per Person"] = age_purchasing_analysis_table["Avg Total Purchase per Person"].map("${0:,.2f}".format)
age_purchasing_analysis_table
```
![](Images/Purchasing%20Analysis%20(Age).png)

### Top Spenders
* Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
  * SN
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value

```python
# Identify the Top 5 Spenders in the Game by Total Purchase Value & GroupBy "SN"
top_spenders = purchase_data.groupby("SN")

# Calculate "Purchase Count"
spender_purchase_count = top_spenders["Purchase ID"].count()

# Calculate "Average Purchase Price"
average_spender_purchase_price = round(top_spenders["Price"].mean(),2)

# Calculate "Total Purchase Value"
total_spender_purchase_value = top_spenders["Price"].sum()

# Create Summary DataFrame
top_spenders_table = pd.DataFrame({ 
    "Purchase Count": spender_purchase_count,
    "Average Purchase Price": average_spender_purchase_price,
    "Total Purchase Value": total_spender_purchase_value
})

sort_top_spenders = top_spenders_table.sort_values(["Total Purchase Value"], ascending=False).head()
sort_top_spenders["Average Purchase Price"] = sort_top_spenders["Average Purchase Price"].astype(float).map("${:,.2f}".format)
sort_top_spenders["Total Purchase Value"] = sort_top_spenders["Total Purchase Value"].astype(float).map("${:,.2f}".format)
sort_top_spenders
```
![](Images/Top%20Spenders.png)

### Most Popular Items
* Identify the 5 most popular items by purchase count, then list (in a table):
  * Item ID
  * Item Name
  * Purchase Count
  * Item Price
  * Total Purchase Value
  
 ```python
 # Identify the 5 Most Popular Items by Creating New DataFrame
popular_items_list = purchase_data[["Item ID", "Item Name", "Price"]]

# GroupBy "Item ID" & "Item Name"
popular_items = popular_items_list.groupby(["Item ID","Item Name"])

# Calculate "Purchase Count"
item_purchase_count = popular_items["Price"].count()

# Calculate "Item Price"
item_price = popular_items["Price"].sum()

# Calculate "Total Purchase Value" 
item_purchase_value = item_price / item_purchase_count

# Create Summary DataFrame
most_popular_items = pd.DataFrame({
    "Purchase Count": item_purchase_count, 
    "Item Price": item_purchase_value,
    "Total Purchase Value": item_price
})

popular_items_formatted = most_popular_items.sort_values(["Purchase Count"], ascending=False).head()
popular_items_formatted["Item Price"] = popular_items_formatted["Item Price"].astype(float).map("${:,.2f}".format)
popular_items_formatted["Total Purchase Value"] = popular_items_formatted["Total Purchase Value"].astype(float).map("${:,.2f}".format)
popular_items_formatted
```
![](Images/Most%20Popular%20Items.png)

### Most Profitable Items
* Identify the 5 most profitable items by total purchase value, then list (in a table):
  * Item ID
  * Item Name
  * Purchase Count
  * Item Price
  * Total Purchase Value

```python
popular_items_formatted = most_popular_items.sort_values(["Total Purchase Value"], ascending=False).head()
popular_items_formatted["Item Price"] = popular_items_formatted["Item Price"].astype(float).map("${:,.2f}".format)
popular_items_formatted["Total Purchase Value"] = popular_items_formatted["Total Purchase Value"].astype(float).map("${:,.2f}".format)
popular_items_formatted
```
![](Images/Most%20Profitable%20Items.png)

## Observable Trends

  * Female players and non-disclosed players spent more on average at 6% and 11% higher than their male counterparts respectively even though males outnumber them by at least 5-1.
  * A substantial 3/4 of players are between 15 and 29 years old with their average spend at $2.99. However, that is 6% lower than the remaining 1/4 of the players between the ages of <10 to 14, and 30 to 40+ years old at $3.16. The age demographic that has the highest average purchases per person are the 35 to 39 year olds with $3.60 that make up almost a 20% difference than 15 - 29 year olds who total 76.8% of players, meaning they have the highest purchasing power.
  * The top 5 spenders spent on average $3.45 per purchase with a total purchase value of $13.32.
  * Oathbreaker (Last Hope of the Breaking Storm), Fiery Glass Crusader and Nirvana are the most profitable items and are also 3 of the top 4 most popular items.
