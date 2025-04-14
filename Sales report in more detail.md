#Sales report: 

# report Link : https://app.powerbi.com/view?r=eyJrIjoiOWJiZmViOTEtZjdkNy00ODA1LWIzZmUtNzRjNjhiNTlhZWY5IiwidCI6ImFhYzE4OGY0LWMwYWItNGY3Yi1iOWQzLTFiYjU4NGQ0ZDQxOSJ9  

# About Report: 

   This report provides a comprehensive analysis of order shipping timelines, highlighting how many days each order took to ship, including those shipped early, on time, or delayed. It also enables users to identify orders with the longest shipping durations. Additionally, the report offers visibility into the profit generated per order and presents insights into country-wise sales performance for specific items. A ranking mechanism has been implemented to help users quickly identify top-performing countries and products. The interactive visualizations allow for at-a-glance data exploration and informed decision-making.  

# Steps followed: 

Imported data from an Excel file into tool for report creation and analysis.   

Utilized Power Query Editor to perform data profiling by enabling key options like Column Distribution, Column Quality, and Column Profile under the View tab for better data understanding and validation. 

After applying transformations and closing the query editor, transitioned to the Report View to design the interactive dashboard. 
 Incorporated a variety of visual elements, including Table, Slicer, Text Box, and Card visuals, Bookmarks, Buttons to enhance user interaction and insight delivery.  

Employed DAX functions to:  

Create calculated columns for sorting and analytical purposes.  
Derive a new column calculating the number of days between order and shipment. 
Implement conditional formatting on the "Days Gap" column to visually represent early, on-time, and delayed shipments for quick interpretation by users. 

                        =============== DAX Function =========  

Days gap with UNICHAR = IF('Sample_Sales Records'[Days gap]>4,UNICHAR(128308), 

                       IF('Sample_Sales Records'[Days gap]<4, UNICHAR(128994), 

                       IF('Sample_Sales Records'[Days gap]=4, UNICHAR(128993) 

                       )))  

 Using the second table visual I represented the profits based on order ID's. In this table I mentioned Order ID, Revenue, Cost, Profit columns for easy understanding. 

 Apart from table I added slicer for three fields Ordered date, shipped date, Days gap by representing color. For this I gave hierarchy for date (Month, Year) by writing DAX function, creating Custom column in power query.   

                    ================ DAX Function =================  

          ship month = month('Sample_Sales Records'[Ship Date]) 

          Ship year = year('Sample_Sales Records'[Ship Date])           

                    ================== Custom Column ==================== 

          order month = Date.month([orderDate]) 

          order year = Date.year([orderDate])  

For the same Date slicers I also added sorted to showcase dates in descending order for that also I created new column for sorting such as converting month's number column in to month's name filed, numbering for sorting. So, that user can easily analyze the data using the date.     

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

In worksheet I focused on showing the rank of the country based on the Revenue.  

For this I used Rank(), Earlier() functions to find the rank based on the countries, Revenue.  

DAX Function:  

 Rank=  

RANKX( 

    FILTER( 

        'Sample_Sales Records', 

        'Sample_Sales Records'[Country] = EARLIER('Sample_Sales Records'[Country]) 

    ), 

    'Sample_Sales Records'[Total Revenue], 

    , 

    DESC 

) 
 

In addition to working with DAX, slicers, and sorting, I implemented bookmarks and buttons to enhance report interactivity and navigation. I created bookmarks, buttons to enable seamless page-to-page navigation, incorporated open/close menu functionality for slicers and cards, and included reset filter options. These enhancements improved the user experience by making the report more dynamic, intuitive, and visually organized.   

 Added cards to show the Max and min profit by creating a new measure. 

 checked all the visuals are working properly or not before publishing to service. 

 created the gateway connection and scheduled refreshment for the semantic model in service. 
 

               This is how I created this Sales Report. 
