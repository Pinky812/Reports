#Sales report:

# report Link : https://app.powerbi.com/view?r=eyJrIjoiOWJiZmViOTEtZjdkNy00ODA1LWIzZmUtNzRjNjhiNTlhZWY5IiwidCI6ImFhYzE4OGY0LWMwYWItNGY3Yi1iOWQzLTFiYjU4NGQ0ZDQxOSJ9

# About Report:
   With this report Organization can easily understand about in how many days the order has shipped and which order took long time, in time, before the mentioned time as well they can also know about the profit for every order in one glance.

# Steps followed:
- Step 1 : Load data into Power BI Desktop, Data Source is excel file.

- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.

- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".

- Step 4 : It was observed that in none of the columns errors & empty values were present except column named "Arrival Delay".

-Step 5 : By clicking on Apply and close it took me to report view where I could develop the report. Here I used table, slicer, Text box, card visuals for my report. It seems simple but I worked on DAX functions for Sorting columns, .

-Step 6 : Let's see the table which is representing about the order shipment such as how many days it took, what is the ordered date, shipped date and day, in this table I created a new column using DAX function to represent days gap in color format so that user can easily understand.

                        =============== DAX Function =========

Days gap with UNICHAR = IF('Sample_Sales Records'[Days gap]>4,UNICHAR(128308),
                       IF('Sample_Sales Records'[Days gap]<4, UNICHAR(128994),
                       IF('Sample_Sales Records'[Days gap]=4, UNICHAR(128993)
                       )))

-Step 7 : Using the second table visual I represented the profit's based on order ID's. In this table I mentioned Order ID, Revenue, Cost, Profit columns for easy understanding.

-Step 8 : Apart from table I added slicer for three fields Ordered date, shipped date, Days gap by representing color. For this I gave hierarchy for date (Month, Year) by writing DAX function, creating Custom column in power query. 

                    ================ DAX Function =================

          ship month = month('Sample_Sales Records'[Ship Date])
          Ship year = year('Sample_Sales Records'[Ship Date])
         
                    ================== Custom Column ====================
          order month = Date.month([orderDate])
          order year = Date.year([orderDate])


- Step 9 : For the same Date slicers I also added sorted to showcase dates in descending order for that also I created new column for sorting such as converting month's number column in to month's name filed, numbering for sorting. So, that user can easily analyse the data using the date.
   
                       =================== DAX Function =====================

             MonthName = FORMAT(DATE(2000,'Sample_Sales Records'[OrderMonth],1),"MMM")
             Ship week name = FORMAT('Ship Date'[Ship Date],"DDD")
             Sortyear = SWITCH(TRUE(),'Sample_Sales Records'[OrderYear]="2017",1,
                         'Sample_Sales Records'[OrderYear]="2016",2, 
                         'Sample_Sales Records'[OrderYear]="2015",3,
                         'Sample_Sales Records'[OrderYear]="2014",4,
                         'Sample_Sales Records'[OrderYear]="2013",5,
                         'Sample_Sales Records'[OrderYear]="2012",6,
                         'Sample_Sales Records'[OrderYear]="2011",7,
                         'Sample_Sales Records'[OrderYear]="2010",8,
                         Blank()
                         )  
           ShipSortMonth = SWITCH(TRUE(),'Sample_Sales Records'[ship month]= 1,"1",
                          'Sample_Sales Records'[ship month]= 2,"2",
                          'Sample_Sales Records'[ship month]= 3,"3",
                          'Sample_Sales Records'[ship month]=4,"4",
                          'Sample_Sales Records'[ship month]=5,"5",
                          'Sample_Sales Records'[ship month]=6,"6",
                          'Sample_Sales Records'[ship month]=7,"7",
                          'Sample_Sales Records'[ship month]=8,"8",
                          'Sample_Sales Records'[ship month]=9,"9",
                          'Sample_Sales Records'[ship month]=10,"10",
                          'Sample_Sales Records'[ship month] =11,"11",
                          'Sample_Sales Records'[ship month]=12,"12",
                                                              Blank()
                                                              )
-Step 10 : Added cards to show the Max and min profit by creating a new measure.
-Step 11: checked all the visuals are working properly or no before publishing to service.
-Step 11: created the gateway connection and scheduled refreshment for the semantic model in service.

               This is how I created this Sales Report.
