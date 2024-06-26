# Practical Exam: Sample SQL Associate

Tech Solutions Inc. is a leading technology company specializing in software development and IT consulting services. The company prides itself on delivering innovative solutions to clients across various industries. With a dedicated team of skilled professionals, TechSolutions has earned a reputation for excellence in the tech industry.

Tech Solutions Inc. has been experiencing a decline in customer satisfaction ratings over the past few months. Customer feedback surveys and support tickets indicate an increase in dissatisfaction among clients. The company is concerned about this trend as it directly impacts customer retention, reputation, and overall business growth.

You are working with the customer support team to provide data to managers to help the company take proactive measures to address these concerns effectively. 

## Data

The following schema diagram shows the tables available.
![support_db](https://github.com/NguyenMav/DataCamp_SQL_Advance_SQL_Associate_Certificate_Practice/assets/149219810/e90981bd-5660-4894-8b88-9aa8ebfd7426)

# Task 1

Before you can start any analysis, you need to confirm that the data is accurate and reflects what you expect to see. 

It is known that there are some issues with the `support` table, and the data team have provided the following data description. 

Write a query to return data matching this description. You must match all column names and description criteria.

| Column Name | Criteria                                                |
|-------------|---------------------------------------------------------|
|id | Discrete. The unique identifier of the support ticket. </br>Missing values are not possible due to the database structure.|
| customer_id | Discrete. The unique identifier of the customer. </br>Missing values should be replaced with 0.|
| category | Nominal. The gategory of the support request, can be one of Feedback, Billing Enquiry, Bug, Installation Problem, Other. </br>Missing values should be replaced with Other. |
| status | Nominal. The current status of the support ticket, one of Open, In Progress or Resolved. </br>Missing values should be replaced with 'Resolved'. |
| creation_date | Discrete. The date the ticket was created. Can be any date in 2023. </br>Missing values should be replaced with 2023-01-01. |
| response_time | Discrete. The number of days taken to respond to the support ticket. </br>Missing values should be replaced with 0. |
| resolution_time | Continuos. The number of hours taken to resolve the support ticket, rounded to 2 decimal places. </br>Missing values should be replaced with 0. |

```sql
-- Write your query for task 1 in this cell
-- Answer
SELECT
    id,
    customer_id,
    INITCAP(category) AS category,
    CASE 
        WHEN status IS NULL THEN 'Resolved'
        WHEN status = '-' THEN 'Resolved'
        ELSE status 
    	END AS status,
    creation_date,
    response_time,
    TRIM(resolution_time, 'hours') AS resolution_time
FROM
    support;
```

| id  | customer_id | category            | status      | creation_date          | response_time | resolution_time |
|-----|-------------|---------------------|-------------|------------------------|---------------|-----------------|
| 1062| 1062        | Installation Problem| In Progress | 2023-01-26T00:00:00.000| 6             | 0               |
| 892 | 892         | Billing Enquiry     | Open        | 2023-06-18T00:00:00.000| 3             | 0               |
| 433 | 433         | Feedback            | Open        | 2023-08-17T00:00:00.000| 1             | 0               |
| 764 | 764         | Billing Enquiry     | Open        | 2023-01-16T00:00:00.000| 3             | 0               |
| 1144| 1144        | Billing Enquiry     | Open        | 2023-06-01T00:00:00.000| 2             | 0               |
| 288 | 288         | Feedback            | Open        | 2023-01-22T00:00:00.000| 2             | 0               |
| 1495| 1495        | Bug                 | In Progress | 2023-02-05T00:00:00.000| 1             | 0               |
| 1090| 1090        | Bug                 | In Progress | 2023-05-09T00:00:00.000| 3             | 0               |
| 1397| 1397        | Feedback            | In Progress | 2023-09-17T00:00:00.000| 2             | 0               |

Rows: 1987

```sql
-- Original support table
SELECT * 
FROM public.support;
```

| id  | customer_id | category            | status      | creation_date          | response_time | resolution_time |
|-----|-------------|---------------------|-------------|------------------------|---------------|-----------------|
| 940 | 940         | Installation Problem| In Progress | 2023-09-21T00:00:00.000| 5             | 0 hours         |
| 1932| 1932        | Other               | -           | 2023-09-23T00:00:00.000| 2             | 3.53 hours      |
| 1744| 1744        | Installation Problem| In Progress | 2023-11-20T00:00:00.000| 8             | 0 hours         |
| 569 | 569         | Billing Enquiry     | Open        | 2023-05-15T00:00:00.000| 3             | 0 hours         |
| 621 | 621         | Installation Problem| In Progress | 2023-04-03T00:00:00.000| 7             | 0 hours         |
| 539 | 539         | Feedback            | Open        | 2023-03-04T00:00:00.000| 1             | 0 hours         |
| 1189| 1189        | Bug                 | Resolved    | 2023-01-20T00:00:00.000| 7             | 3.91 hours      |
| 1852| 1852        | Other               | -           | 2023-11-12T00:00:00.000| 1             | 5.11 hours      |
| 1120| 1120        | Billing Enquiry     | Open        | 2023-02-27T00:00:00.000| 3             | 0 hours         |
| 47  | 47          | Feedback            | Open        | 2023-02-06T00:00:00.000| 1             | 0 hours         |

```sql
-- checking details about the support table
SELECT *
FROM information_schema.columns
WHERE table_name = 'support';
```

| table_catalog               | table_schema | table_name | column_name     | ordinal_position | column_default | is_nullable | data_type | character_maximum_length | character_octet_length | numeric_precision | numeric_precision_radix | numeric_scale | datetime_precision | interval_type | interval_precision | character_set_catalog | character_set_schema | character_set_name | collation_catalog | collation_schema | collation_name | domain_catalog | domain_schema | domain_name | udt_catalog               | udt_schema | udt_name | scope_catalog | scope_schema | scope_name | maximum_cardinality | dtd_identifier | is_self_referencing | is_identity | identity_generation | identity_start | identity_increment | identity_maximum | identity_minimum | identity_cycle | is_generated | generation_expression | is_updatable |
|-----------------------------|--------------|------------|-----------------|------------------|----------------|-------------|-----------|--------------------------|------------------------|-------------------|-------------------------|---------------|--------------------|---------------|--------------------|----------------------|---------------------|-------------------|------------------|----------------|----------------|----------------|---------------|-------------|----------------------|-------------|-----------|---------------|--------------|------------|---------------------|----------------|--------------------|-------------|--------------------|----------------|--------------------|-----------------|-----------------|----------------|--------------|---------------------|--------------|
| certification_customer_support | public       | support    | id              | 1                | null           | YES         | integer   | null                     | null                   | 32                | 2                       | 0             | null               | null          | null               | null                 | null                | null              | null             | null           | null           | certification_customer_support | pg_catalog  | int4      | null          | null         | null       | 1                   | NO             | NO                 | null        | null               | null           | null               | null            | NO              | NEVER          | null                 | YES          |
| certification_customer_support | public       | support    | customer_id     | 2                | null           | YES         | integer   | null                     | null                   | 32                | 2                       | 0             | null               | null          | null               | null                 | null                | null              | null             | null           | null           | certification_customer_support | pg_catalog  | int4      | null          | null         | null       | 2                   | NO             | NO                 | null        | null               | null           | null               | null            | NO              | NEVER          | null                 | YES          |
| certification_customer_support | public       | support    | category        | 3                | null           | YES         | text      | null                     | 1073741824             | null              | null                    | null          | null               | null          | null               | null                 | null                | null              | null             | null           | null           | certification_customer_support | pg_catalog  | text      | null          | null         | null       | 3                   | NO             | NO                 | null        | null               | null           | null               | null            | NO              | NEVER          | null                 | YES          |
| certification_customer_support | public       | support    | status          | 4                | null           | YES         | text      | null                     | 1073741824             | null              | null                    | null          | null               | null          | null               | null                 | null                | null              | null             | null           | null           | certification_customer_support | pg_catalog  | text      | null          | null         | null       | 4                   | NO             | NO                 | null        | null               | null           | null               | null            | NO              | NEVER          | null                 | YES          |
| certification_customer_support | public       | support    | creation_date   | 5                | null           | YES         | date      | null                     | null                   | null              | null                    | null          | 0                  | null          | null               | null                 | null                | null              | null             | null           | null           | certification_customer_support | pg_catalog  | date      | null          | null         | null       | 5                   | NO             | NO                 | null        | null               | null           | null               | null            | NO              | NEVER          | null                 | YES          |
| certification_customer_support | public       | support    | response_time   | 6                | null           | YES         | integer   | null                     | null                   | 32                | 2                       | 0             | null               | null          | null               | null                 | null                | null              | null             | null           | null           | certification_customer_support | pg_catalog  | int4      | null          | null         | null       | 6                   | NO             | NO                 | null        | null               | null           | null               | null            | NO              | NEVER          | null                 | YES          |
| certification_customer_support | public       | support    | resolution_time | 7                | null           | YES         | text      | null                     | 1073741824             | null              | null                    | null          | null               | null          | null               | null                 | null                | null              | null             | null           | null           | certification_customer_support | pg_catalog  | text      | null          | null         | null       | 7                   | NO             | NO                 | null        | null               | null           | null               | null            | NO              | NEVER          | null                 | YES          |

## Notes

- **is_nullable:** Indicates if the column can accept NULL values.
- **data_type:** Specifies the data type of the column.
- **numeric_precision, numeric_scale:** Relevant for numeric data types.
- **character_maximum_length, character_octet_length:** Relevant for character data types.
- **datetime_precision:** Relevant for date/time data types.
- **is_self_referencing:** Indicates if the column is self-referencing.
- **is_identity, identity_generation:** Indicates if the column is an identity column and its generation strategy.
- **is_generated, generation_expression:** Indicates if the column is generated and its generation expression.
- **is_updatable:** Indicates if the column is updatable.

# Task 2

It is suspected that the response time to tickets is a big factor in unhappiness.

Calculate the minimum and maximum response time for each category of support ticket. 

Your output should include the columns `category`, `min_response` and `max_response`. 

Values should be rounded to two decimal places where appropriate. 

```sql
-- Write your query for task 2 in this cell
SELECT category, 
	MIN(response_time) AS min_response, 
	MAX(response_time) AS max_response
FROM support
GROUP BY category;
```

| Category             | Min Response Time | Max Response Time |
|----------------------|-------------------|-------------------|
| Other                | 1                 | 5                 |
| Bug                  | 1                 | 13                |
| Feedback             | 1                 | 2                 |
| Billing enquiry      | 2                 | 8                 |
| Installation Problem | 5                 | 17                |

## Notes

- **Category:** The type of support request.
- **Min Response Time:** The minimum number of days taken to respond to a support ticket in this category.
- **Max Response Time:** The maximum number of days taken to respond to a support ticket in this category.

# Task 3

The support team want to know more about the `rating` provided by customers who reported `Bugs` or `Installation Problem`s. 

Write a query to return the `rating` from the survey, the `customer_id`, `category` and `response_time` of the support ticket, for the customers & categories of interest. 

Use the original support table, not the output of task 1. 

```sql
-- Write your query for task 3 in this cell
SELECT rating, support.customer_id, category, response_time
FROM support
INNER JOIN Survey
	ON support.customer_id = Survey.customer_id
WHERE category = 'Bug' OR category = 'Installation Problem'
```
| Rating | Customer ID | Category             | Response Time |
|--------|-------------|----------------------|---------------|
| 6      | 621         | Installation Problem | 7             |
| 5      | 1703        | Installation Problem | 6             |
| 5      | 766         | Installation Problem | 7             |
| 5      | 1824        | Bug                  | 3             |
| 4      | 931         | Installation Problem | 9             |
| 6      | 1795        | Installation Problem | 7             |
| 5      | 1703        | Bug                  | 2             |
| 2      | 747         | Bug                  | 2             |
| 5      | 1836        | Bug                  | 1             |
| 7      | 1882        | Installation Problem | 6             |
| 6      | 1882        | Installation Problem | 6             |
| 7      | 1772        | Installation Problem | 5             |
| 3      | 879         | Bug                  | 3             |
| 3      | 902         | Bug                  | 2             |
| 5      | 1569        | Installation Problem | 6             |
| 3      | 1035        | Bug                  | 4             |
| 3      | 930         | Installation Problem | 6             |
| 5      | 1836        | Bug                  | 2             |
| 6      | 1553        | Installation Problem | 6             |
| 3      | 707         | Installation Problem | 6             |
| 5      | 1796        | Installation Problem | 5             |
| 4      | 1220        | Bug                  | 6             |
| 3      | 1614        | Installation Problem | 6             |
| 4      | 559         | Bug                  | 3             |
| 6      | 1821        | Bug                  | 3             |
