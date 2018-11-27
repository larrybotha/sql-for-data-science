# Week 1

## Data Models, Part 1: Thinking About Your Data

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/emmOd/data-models-part-1-thinking-about-your-data)

It's important to think about requirements before implementing anything.
Front-loading this work saves time later.

## Data Models, Part 2: The Evolution of Data Models

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/MJ3Q5/data-models-part-2-the-evolution-of-data-models)

- **data modeling:** the organising of multiple tables, and how they relate to
    each other
    - usually represents a business process
    - can help in understanding business processes
    - data models should always represent a real world problem as closely as
        possible

## Data Models, Part 3: Relational vs. Transactional Models

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/HRlau/data-models-part-3-relational-vs-transactional-models)

### Relational vs Transactional model

- relational:
    - allows for easy querying and data manipulation
    - logical and intuitive querying
- transactional:
    - used for storing data, and not necessarily for querying data
    - e.g. medical database of patient data
    - data usually needs to be extracted into a relational model to be made
        sense of

### Building blocks of data models

1. entities
    - these are discrete 'things'
    - e.g. person, place, event, etc.
2. attributes
    - characteristics of entities
    - e.g. age, height, etc.
3. relationships
    - associations between entities
    - one-to-many
          - a customer's invoices
    - many-to-many
          - students assigned to classes
    - one-to-one
          - a manager for a store

### ER diagrams

ER (entity-relationship) diagrams are composed of entity types, and specify
relationships between those entities.

They have the following functions:

- show entity relationships
- show business processes
- are visual aids
- show links through primary keys

### Primary and foreign keys

Primary and foreign keys are the mechanism by which tables are related to each
other.

- primary key
    - a column, or set of columns, that uniquely identifies every row in a table
- foreign key
    - one or more columns that can be used together to identify a single row in
        another table
