# Types of Tables in Database Management

## 1. Fact Tables
- **Purpose**: Store measurable, quantitative data related to business processes.
- **Examples**: Sales, transactions, revenue data.
- **Use/Importance**: Central to data analysis and reporting.

---

## 2. Dimension Tables
- **Purpose**: Provide descriptive, qualitative context for facts.
- **Examples**: Products, customers, locations.
- **Use/Importance**: Essential for understanding the "who, what, where, when, why, and how" of facts.

---

## 3. Aggregate Tables
- **Purpose**: Store pre-calculated summaries to improve query performance.
- **Examples**: Monthly sales totals, yearly revenue.
- **Use/Importance**: Highly useful for speeding up reporting and analytics.

---

## 4. Transaction Tables
- **Purpose**: Capture raw, granular data from individual events or transactions.
- **Examples**: Purchase orders, customer interactions.
- **Use/Importance**: Foundational for building fact tables and detailed analysis.

---

## 5. Lookup Tables
- **Purpose**: Provide static reference data for consistency and reusability.
- **Examples**: Country codes, tax rates, status flags.
- **Use/Importance**: Frequently used for validation and joining other tables.

---

## 6. Slowly Changing Dimension (SCD) Tables
- **Purpose**: Track historical changes to dimension attributes.
- **Examples**: Changes in customer address or product pricing over time.
- **Use/Importance**: Critical for time-based analysis and historical comparisons.

---

## 7. Bridge Tables
- **Purpose**: Resolve many-to-many relationships between dimensions and facts.
- **Examples**: StudentCourses (linking students and courses).
- **Use/Importance**: Necessary for complex data relationships.

---

## 8. Staging Tables
- **Purpose**: Temporarily hold data during ETL processes.
- **Examples**: Raw data before cleansing or transformation.
- **Use/Importance**: Essential for data integration and processing pipelines.

---

## 9. History Tables
- **Purpose**: Store archived data for auditing, analysis, or compliance purposes.
- **Examples**: Archived sales data, audit logs.
- **Use/Importance**: Useful for legal compliance and long-term trend analysis.
