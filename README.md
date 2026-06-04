# FEC Campaign Finance Data Warehouse

### Basic Information

* **Members:** Dana Ortiz, [dana.ortiz@gwu.edu](mailto:dana.ortiz@gwu.edu)
* **Date:** May 2026
* **Model Version:** 1.0
* **License:** MIT
* **Model Implementation Code:** [FEC Campaign Finance Data Warehouse](FEC-Campaign-Finance-Data-Warehouse.ipynb)

### Intended Use

* **Intended Uses:** This project is an educational example of building a SQL-based data warehouse in AWS using Federal Election Commission campaign finance data. The project loads raw FEC contribution and committee data, cleans and standardizes fields, builds a star schema, validates fact and dimension relationships, and uses SQL queries to answer analytical questions about contribution patterns, contributor types, occupations, party affiliation, states, and connected organizations.
* **Out-of-Scope Use Cases:** Any legal compliance review, election law auditing, donor targeting, campaign strategy, financial reporting, real-time election monitoring, or official political analysis. This project is strictly for educational demonstrations of SQL, ETL, dimensional modeling, and business intelligence workflows.

### Training Data

* **Data Dictionary:**

| Name                           | Modeling Role       | Measurement Level | Description                                                                         |
| ------------------------------ | ------------------- | ----------------- | ----------------------------------------------------------------------------------- |
| **cmte_id**                    | input / natural key | text              | FEC committee ID used to connect contribution transactions to committee information |
| **amndt_ind**                  | input               | categorical       | Amendment indicator from the FEC contribution file                                  |
| **rpt_tp**                     | input               | categorical       | Report type code, later decoded into a readable report type description             |
| **rpt_tp_desc**                | engineered input    | categorical       | Cleaned description of the report type                                              |
| **transaction_pgi**            | input               | categorical       | Primary/general/election period indicator                                           |
| **transaction_pgi_desc**       | engineered input    | categorical       | Cleaned description of the transaction election period                              |
| **image_num**                  | input               | text              | FEC image number associated with the transaction                                    |
| **transaction_tp**             | input               | categorical       | Transaction type code                                                               |
| **transaction_tp_desc**        | engineered input    | categorical       | Cleaned transaction type description                                                |
| **entity_tp**                  | input               | categorical       | Contributor entity type code, such as individual, organization, PAC, or candidate   |
| **entity_desc**                | engineered input    | categorical       | Readable contributor entity type description                                        |
| **contributor_name**           | input               | text              | Contributor name after cleaning and standardization                                 |
| **contributor_city**           | input               | text              | Contributor city                                                                    |
| **contributor_state**          | input               | categorical       | Contributor state                                                                   |
| **contributor_zip_code**       | input               | text              | Contributor ZIP code                                                                |
| **employer**                   | input               | text              | Contributor employer                                                                |
| **occupation**                 | input               | text              | Contributor occupation                                                              |
| **transaction_date**           | input / engineered  | date              | Transaction date converted from raw FEC text format                                 |
| **date_key**                   | engineered key      | int               | Integer date key used to connect transactions to the date dimension                 |
| **transaction_amt**            | measure             | numeric           | Original transaction amount, including positive contributions and negative refunds  |
| **positive_contribution_amt**  | measure             | numeric           | Positive contribution amount, with refunds stored as zero                           |
| **refund_amt**                 | measure             | numeric           | Refund amount, with negative transactions converted into positive refund values     |
| **other_id**                   | input               | text              | Other FEC ID when applicable                                                        |
| **tran_id**                    | input               | text              | Transaction ID                                                                      |
| **file_num**                   | input               | text              | FEC file number                                                                     |
| **memo_cd**                    | input               | categorical       | Memo code from the FEC file                                                         |
| **is_memo**                    | engineered input    | boolean           | Indicates whether the row is a memo transaction                                     |
| **memo_text**                  | input               | text              | Memo text associated with the contribution                                          |
| **sub_id**                     | input               | text              | FEC submission ID                                                                   |
| **committee_name**             | input               | text              | Name of the political committee                                                     |
| **treasurer_name**             | input               | text              | Committee treasurer name                                                            |
| **committee_city**             | input               | text              | Committee city                                                                      |
| **committee_state**            | input               | categorical       | Committee state                                                                     |
| **committee_zip**              | input               | text              | Committee ZIP code                                                                  |
| **committee_designation**      | input               | categorical       | FEC committee designation code                                                      |
| **committee_designation_desc** | engineered input    | categorical       | Readable committee designation description                                          |
| **committee_type**             | input               | categorical       | FEC committee type code                                                             |
| **committee_type_desc**        | engineered input    | categorical       | Readable committee type description                                                 |
| **party_affiliation**          | input               | categorical       | Party affiliation associated with the committee                                     |
| **filing_frequency**           | input               | categorical       | Committee filing frequency code                                                     |
| **filing_frequency_desc**      | engineered input    | categorical       | Readable filing frequency description                                               |
| **organization_type**          | input               | categorical       | Organization type code                                                              |
| **organization_type_desc**     | engineered input    | categorical       | Readable organization type description                                              |
| **connected_org_name**         | input               | text              | Name of connected organization, if available                                        |
| **cand_id**                    | input               | text              | Candidate ID connected to the committee, if applicable                              |

* **Source of Training Data:**

  * [Individual Contribution Transactions](https://istm-4212.s3.us-east-1.amazonaws.com/contribution+by+individuals_2024.zip)
  * [Committee Master File](https://istm-4212.s3.us-east-1.amazonaws.com/committee_2024.zip)

* **How training data was divided into training and validation data:** No train-validation split was used because this is a SQL data warehouse and business intelligence project, not a supervised machine learning model. The complete dataset was loaded into raw staging tables, cleaned into standardized tables, transformed into a star schema, and validated through SQL integrity checks.

* **Number of rows in training and validation data:**

  * Raw individual contribution rows: 26,323,444
  * Raw committee rows: 20,941
  * Clean individual contribution rows: 26,323,444
  * Clean committee rows: 20,941
  * Training rows: Not applicable
  * Validation rows: Not applicable

### Test Data

* **Source of test data:** No separate test dataset was used. The project used SQL validation checks to confirm that the data warehouse was built correctly.
* **Number of rows in test data:** Not applicable.
* **State any differences in columns between training and test data:** Not applicable. This project uses a complete ETL and dimensional modeling workflow rather than a train/test modeling workflow.

### Model Details

* **Columns used as inputs in the final model:** This project does not use a predictive model. The final warehouse uses cleaned contribution, contributor, committee, date, report, election, and transaction fields from the FEC datasets.
* **Column(s) used as target(s) in the final model:** None. This is a data warehousing and SQL analytics project, not a supervised learning project.
* **Type of model:** SQL data warehouse / dimensional model / star schema
* **Software used to implement the model:** AWS, Ubuntu, Jupyter Notebook, Python, SQL, PostgreSQL, ipython-sql, Matplotlib
* **Version of the modeling software:** Python 3.14.5, PostgreSQL 16.13
* **Hyperparameters or other settings of your model:**

Project database setup:

```sql
CREATE DATABASE fec_project;
```

Raw staging tables:

```sql
CREATE TABLE raw_individual_contributions (...);
CREATE TABLE raw_committee (...);
```

Data loading approach:

```sql
COPY raw_committee
FROM '/home/ubuntu/fec_project/cm.txt'
DELIMITER '|'
NULL ''
CSV;
```

```sql
COPY raw_individual_contributions
FROM '/home/ubuntu/fec_project/itcont_2024_*.txt'
DELIMITER '|'
NULL ''
CSV;
```

Star schema tables:

```sql
CREATE TABLE dim_committee (...);
CREATE TABLE dim_contributor (...);
CREATE TABLE dim_date (...);
CREATE TABLE dim_report_election (...);
CREATE TABLE fact_contribution (...);
```

Fact table grain:

```sql
-- One row per contribution transaction
```

Core fact table measures:

```sql
transaction_amt
positive_contribution_amt
refund_amt
contribution_count
is_memo
```

Indexing strategy:

```sql
CREATE INDEX idx_fact_contribution_committee_key
    ON fact_contribution (committee_key);

CREATE INDEX idx_fact_contribution_contributor_key
    ON fact_contribution (contributor_key);

CREATE INDEX idx_fact_contribution_date_key
    ON fact_contribution (date_key);

CREATE INDEX idx_fact_contribution_report_election_key
    ON fact_contribution (report_election_key);
```

### Quantitative Analysis

* **Metrics Used to Evaluate:** Row counts, fact/dimension integrity checks, missing foreign key checks, contribution totals, refund totals, memo row counts, and SQL query outputs.

Final warehouse row counts:

| Table                   | Row Count      |
| ----------------------- | -------------- |
| **dim_committee**       | **20,942**     |
| **dim_contributor**     | **4,956,972**  |
| **dim_date**            | **139**        |
| **dim_report_election** | **993**        |
| **fact_contribution**   | **26,323,444** |

Validation checks:

| Check                                             | Result         |
| ------------------------------------------------- | -------------- |
| Clean contribution rows                           | **26,323,444** |
| Fact contribution rows                            | **26,323,444** |
| Missing committee keys                            | **0**          |
| Missing contributor keys                          | **0**          |
| Missing date keys                                 | **0**          |
| Missing report/election keys                      | **0**          |
| Fact rows without committee dimension match       | **0**          |
| Fact rows without contributor dimension match     | **0**          |
| Fact rows without date dimension match            | **0**          |
| Fact rows without report/election dimension match | **0**          |

Fact table measures:

| Measure                                | Value                 |
| -------------------------------------- | --------------------- |
| **Total fact rows**                    | **26,323,444**        |
| **Total contribution count**           | **26,323,444**        |
| **Net transaction amount**             | **$7,628,350,747.00** |
| **Total positive contribution amount** | **$7,709,099,062.00** |
| **Total refund amount**                | **$80,748,315.00**    |
| **Memo rows kept**                     | **46,037**            |

Main SQL analysis questions:

| Question                                     | Purpose                                                                               |
| -------------------------------------------- | ------------------------------------------------------------------------------------- |
| **Q1: Contribution Amounts by Entity Type**  | Compares positive non-memo contribution totals by contributor entity type             |
| **Q2: Top Party by State**                   | Identifies which party receives the highest positive contribution total in each state |
| **Q3: Top Occupations and Party Preference** | Compares occupation groups by top party affiliation and contribution totals           |
| **Q4: Connected Organizations and Funding**  | Identifies connected organizations with the most committee-linked funding             |

Selected findings:

| Analysis                            | Main Finding                                                                                                                                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Entity type analysis**            | Individual contributors dominate the transaction-level contribution file                                                                                                                    |
| **State-party analysis**            | Several high-volume states, including California and New York, show Democratic-leading contribution totals, while states such as Texas and Ohio show Republican-leading contribution totals |
| **Occupation analysis**             | Large-volume groups like NOT EMPLOYED and RETIRED contribute many transactions, while groups such as ENTREPRENEUR, CEO, and EXECUTIVE show higher average transaction amounts               |
| **Connected organization analysis** | Some connected organizations show mass-donation patterns with many smaller contributions, while others show fewer but much larger average contributions                                     |

### Ethical Considerations

* **Potential negative impacts of using this model:**

  * *Math or Software Problems:* Large public datasets require careful cleaning, and incorrect handling of missing committee IDs, memo rows, negative transactions, date parsing, or duplicate contributor identities could produce misleading totals. Contributor identity is especially difficult because the FEC dataset does not provide one clean universal contributor ID, so this project creates a contributor natural key using multiple fields.
  * *Real World Risks:* Campaign finance data can be politically sensitive. If this project were misused outside an educational context, it could support misleading political narratives, oversimplified claims about donor behavior, or inappropriate profiling of contributors, occupations, states, or organizations.

* **Uncertainties relating to the impacts of using the model:**

  * *Math or Software Uncertainties:* Some committee IDs in contribution records do not match the committee master file, so placeholder committee dimension rows are used to preserve fact rows. Some party affiliations are unknown, and many contribution records have missing or standardized values such as `UNKNOWN` or `NONE`. These choices preserve data volume but may affect interpretation.
  * *Real World Uncertainties:* Contribution patterns can change quickly during election cycles. The data reflects a specific 2024 FEC reporting window and should not be generalized to all campaign finance activity without additional context. External factors such as campaign events, fundraising deadlines, candidate changes, legal rules, and reporting practices may explain some observed patterns.

* **Unexpected Results:** The warehouse retained 46,037 memo rows instead of removing them. This was an intentional design choice because memo rows may still contain useful transaction information, but they are flagged with `is_memo` and filtered out of the main positive contribution analyses when needed. Another important result was that two contribution rows had committee IDs that did not match the committee master file, so placeholder committee dimension rows were created to preserve all fact rows and avoid broken joins.

### AI Use Disclosure

AI tools were used to assist in refining comments and documentation. All SQL logic, analysis decisions, warehouse validation, outputs, and final conclusions should be reviewed and verified by the project author before submission.
