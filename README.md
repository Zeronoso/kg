# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
Create a database with pgadmin for a ee-commerce business with the provided data, and give insightful analytics based on said data provided.

## Process
- Import the 5 excel cvs files provided into a new Postgres database.
- review said data once imported and see what needs to be altered and/or removed.
- set up primary/foreign keys so that the tables can relate to one another.
- remove/altering null, empty, and otherwise insignificant values.


## Results
4 of the 5 tables were connected via the "sku" column. Analytics table had to be left by itself as it had no columns that could be related to the other tables.
With these 4 tables, we can perform a number of analysis's for the business in order to make future business directions.

## Challenges 
- Cleaning the analytics table was difficult due how many entries populated the table, some queries took minutes and cleaning duplicates was also very difficult runtime wise.
- Missing data was in abundance, making some analysis more or less unreliable due to said missing information in certain columns.
- I did accidently set all countries to null when trying to clean, so I had to delete the database and reimport from the excel file. This highlighted the importance of creating temporary tables / backups.

## Future Goals
I would like to be able to normalize the data, but I have little practice in doing so and would need more experience in order to normalize more effectively.
