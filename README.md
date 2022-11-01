# Power BI - Pareto's Graph

Step by Step on How to Create Pareto's Graphic with Power BI

## 1st - Create a New Table based on the Main Table:
*(This is used to create a clean-table with only the values necessary for the graph)*

- Modeling > New Table *(Write DAX Expression)*
 
 ![image](https://user-images.githubusercontent.com/16196820/199260867-3c91174b-6814-4c61-856d-38b3e221fc5d.png)

- Use the following DAX Code:

````dax
Pareto = SUMMARIZE(
        'Sell Report Table',                   // Main Table
        'Sell Report Table'[Countries],        // Data from Region or Product
        "Product", 	                       // New Field
        SUM('Sell Report Table'[Impact $ (K)]) // Data from Value (Generally $)
        )
````

## 2nd - Create a new Ranking Column:
*(Rank from biggest to smallest)*

- Right Click *(On New Table)* > New Column

- Use the following DAX Code:

````dax
Ranking = RANKX(Pareto,            // New Table
                Pareto[Countries]) // New Field from the table
````

## 3rd - Create a New Total Cumulative Column:

````dax
Total Cumulative = CALCULATE(SUM(Pareto[Countries]), 
                   TOPN(Pareto[Ranking], 
                   ALL(Pareto), 
                   [Countries]))
````

## 4th - Create a New Total Sales Column:

````dax
Total Sales = CALCULATE(SUM(Pareto[Countries]),
                        ALL(Pareto))
````

## 5th - Create a New Percentual Cumulative Column:

````dax
Percentual = [Total Cumulative]/[Total Sales]
````

## 6th - Create the New Graph:
*(Line and Clustered Column Chart)*

- X Axis: 		   'Countries'
- Y Column Axis: 'Impact $(K)'
- Y Line Axis:   'Percentual'

The result should be something like this:

![image](https://user-images.githubusercontent.com/16196820/199264818-4930bfa7-cdee-4c35-913f-176257f45fbb.png)


*This was created with Power BI 2022 - 2.109.1021.0*
