1. **Data Normalization Process**:
    
    - What: It’s the process of organizing data to reduce redundancy and improve data integrity.
    - **How to Achieve**: Organize data into tables, reduce redundancy, and use techniques like dividing data into smaller tables and defining relationships between them.
    - **Why Needed**: To ensure data integrity, reduce redundancy, and improve query performance.
- **Keys**
    
    - **Primary Key**: A unique identifier for a record in a table.
    - **Foreign Key**: A reference to a primary key in another table to establish relationships.
    - **Composite Key**: A combination of two or more columns to uniquely identify a record.
- **Indexes**
    
    - **What**: Data structures that improve query performance by allowing faster lookups.
    - **When Needed**: For frequently queried columns or columns used in WHERE, JOIN, or ORDER BY clauses.
    - **Too Many Indexes**: Increases storage costs and slows down write operations like INSERT, UPDATE, DELETE.