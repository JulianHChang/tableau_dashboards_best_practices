# Combining Data


# Joins
Joining is a method to combine data from multiple data sources or combine data from different
tables in a single data source. When we refer data from multiple data sources then it is called
as cross-database joins.
There are 4 types of joins:

## Inner – 
The resulting table contains values that have matches in both tables.
## Left – 
The resulting table contains all values from the left table and corresponding matches
from the right table.
## Right - 
The resulting table contains all values from the right table and corresponding matches
from the left table.
## Full Outer – 
The resulting table contains all values from both tables.
# Blending
Blend is a left join after aggregation at the level of granularity.
We need separate data connection for every participant in blend. One data source is
considered as primary and the other is secondary. The primary data source is determined by
the field you first add to a view. Tableau shows this by giving that data source a blue check
mark in the data pane. If you then add a field from a secondary data source, then that data
source will be considered as secondary and shown in orange color check mark in the data pane.
Data blending is worksheet specific. It means that we can change which data source to be
considered as primary and which is secondary on different worksheet in a same workbook.
The major difference between join and blend - Join combine data before aggregation at row
level whereas blend combines data after aggregation.
# Unions
It is a way to combine multiple tables from single data source. For optimal results, the tables
that we combine using a union must have the same structure i.e. same number of fields and
that fields must have the same names and data types.
When we union multiple sheets, two new columns automatically generated, Sheet and
TableName.
The difference between join and union – Join appends columns from one table to another
whereas union appends rows.