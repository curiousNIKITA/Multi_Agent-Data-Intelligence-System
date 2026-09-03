# 🤖 Multi-Agent Data Intelligence System

An intelligent multi-agent system designed to automate data analysis, SQL querying, and ETL workflows using natural language.

The system uses specialized AI agents orchestrated through **LangGraph** to understand a user's request, route it to the appropriate agent, execute data operations using deterministic tools, and return meaningful results.

Instead of using a single AI agent for every task, the system follows a **specialized multi-agent architecture**, where each agent is responsible for a specific type of data operation.


## 🎯 Problem Statement

Organizations work with data stored across databases, CSV files, APIs, and other data sources. Performing common data tasks often requires technical knowledge of:

- SQL
- Data extraction
- Data transformation
- Python
- Pandas
- Databases
- ETL pipelines

Non-technical users may know what information they need but may not know how to write SQL queries or perform data transformations.

For example:

> "Show me the top five customers by revenue."

> "Find customers who have not placed an order in the last 90 days."

> "Extract data from this API and transform it into a structured dataset."

> "Clean this dataset and convert it into JSON."

The **Multi-Agent Data Intelligence System** allows users to express these requests in natural language and delegates the task to specialized agents.


# 💡 Solution

The system uses a **Router Agent** to understand the user's request and determine which specialized agent should handle the task.

The current architecture includes two primary specialized agents:

 **SQL Analyst Agent**
 **ETL Analyst Agent**

The Router Agent analyzes the user's request and routes it to the appropriate agent.


                         User Request
                              │
                              ▼
                    ┌──────────────────┐
                    │   Router Agent   │
                    └──────────────────┘
                              │
                  ┌───────────┴───────────┐
                  │                       │
                  ▼                       ▼
         ┌────────────────┐      ┌────────────────┐
         │ SQL Analyst    │      │ ETL Analyst    │
         │ Agent          │      │ Agent          │
         └────────────────┘      └────────────────┘
                  │                       │
                  ▼                       ▼
            SQL Operations          Data Operations
                  │                       │
                  └───────────┬───────────┘
                              │
                              ▼
                        Final Response



## 🏗️ Architecture

The system follows a hierarchical agent architecture:

```
┌─────────────────────────────────────────────────────────────┐
│                    Data Agent (Router)                      │
│         Routes user queries to appropriate sub-agents       │
└────────────────────┬────────────────────────────────────────┘
                     │
         ┌───────────┴───────────┐
         │                       │
         ▼                       ▼
    ┌──────────────┐        ┌──────────────┐
    │ SQL Analyst  │        │ ETL Analyst  │
    │   Agent      │        │   Agent      │
    └──────────────┘        └──────────────┘
         │                       │
         ├─► Query Curation      ├─► Extract Load
         ├─► Schema Context      ├─► Transform Load
         ├─► SQL Generation      └─► Code Execution
         ├─► Safety Validation   
         ├─► Query Execution     
         └─► Answer Generation   
```

### State Flow

1. **User Input** → Natural language query
2. **Router Node** → Classifies query as SQL or ETL
3. **Agent Dispatch** → Routes to appropriate sub-agent
4. **Processing** → Each agent processes the task
5. **Output** → Returns structured result to user



## 1. Router Agent

The Router Agent acts as the entry point for user requests.

- Understand the user's natural-language request
- Identify the type of task
- Select the appropriate specialized agent
- Route the request through the LangGraph workflow

Example:

User:
"Show me the top 10 customers by revenue."

        ↓

Router Agent

        ↓

SQL Analyst Agent
```

Another example:

```text
User:
"Extract data from an API, clean it, and convert it to CSV."

        ↓

Router Agent

        ↓

ETL Analyst Agent

The Router Agent ensures that specialized tasks are handled by the agent best suited for the operation.


## 2. SQL Analyst Agent

The SQL Analyst Agent is responsible for database-related tasks.

It can interpret natural-language requests and perform SQL-based analysis.

Typical responsibilities include:

- Generating SQL queries
- Retrieving data from PostgreSQL
- Filtering data
- Aggregating metrics
- Sorting results
- Identifying top-performing entities
- Performing analytical queries
- Returning structured query results

Example:

User Question:

"Show the top 5 customers by total revenue."

        ↓

SQL Analyst Agent

        ↓

Generate SQL Query

        ↓

Execute Query

        ↓

PostgreSQL

        ↓

Return Results
```

Example SQL operation:

```sql
SELECT
    customer_id,
    SUM(revenue) AS total_revenue
FROM orders
GROUP BY customer_id
ORDER BY total_revenue DESC
LIMIT 5;
The agent uses database tools to execute the query and retrieve actual results.


## 3. ETL Analyst Agent

The ETL Analyst Agent handles data extraction and transformation workflows.

ETL stands for:

E → Extract
T → Transform
L → Load


The agent can handle tasks such as:

- Reading CSV files
- Processing datasets
- API data extraction (JSON to structured formats)
- Data Transformation using pandas
- Multi-format support (CSV, JSON, Parquet)


Example workflow:

API / CSV / Data Source
          │
          ▼
       Extract
          │
          ▼
       Transform
          │
          ▼
    Clean Dataset
          │
          ▼
      Final Output


The ETL agent uses Python and Pandas-based operations to process data.


# ⚙️ System Workflow

The complete workflow follows the sequence below:

1. User submits a request
            │
            ▼
2. Request enters the LangGraph workflow
            │
            ▼
3. Router Agent analyzes the request
            │
            ▼
4. Task is classified
            │
     ┌──────┴──────┐
     │             │
     ▼             ▼
 SQL Task       ETL Task
     │             │
     ▼             ▼
SQL Agent      ETL Agent
     │             │
     ▼             ▼
Database       Pandas / API /
Query          Data Processing
     │             │
     └──────┬──────┘
            │
            ▼
5. Results are returned


# 🏗️ Technology Stack

## AI and Agent Orchestration

- LangGraph
- Large Language Models
- Agent routing
- Conditional workflows
- Structured agent state

## Data Processing

- Python
- Pandas

## Database

- PostgreSQL
- SQLAlchemy

## Validation and Data Models

- Pydantic

## Environment and Configuration

- Python virtual environment
- Environment variables
- `.env` configuration





📁 Project Structure
Data_Agent/
├── agents/                          # Agent implementations
│   ├── __init__.py
│   ├── data_agent.py               # Main router agent
│   ├── sql_analyst.py              # SQL query agent
│   └── etl_analyst.py              # ETL operations agent
│
├── Models/                          # Data models
│   ├── __init__.py
│   └── schema.py                   # Pydantic schemas for state management
│
├── utils/                           # Utility modules
│   ├── __init__.py
│   ├── database.py                 # PostgreSQL utilities
│   ├── etl_tools.py                # ETL operations toolkit
│   ├── llm_pick.py                 # LLM selection logic
│
├── data/                            # Data directory
│   ├── extract/                     # Extracted data storage
│   ├── transform/                   # Transformed data storage
│   ├── payments.csv                 # Sample dataset
│   ├── ratings.csv                  # Sample dataset
│   ├── rides.csv                    # Sample dataset
│   ├── users.csv                    # Sample dataset
│   └── vehicles.csv                 # Sample dataset
│
├── main.py                          # Entry point
├── feed_db.py                       # Database initialization script
├── pyproject.toml                   # Project metadata and dependencies
└── README.md                         # This file


> The exact folder structure may evolve as the system is extended with additional agents, tools, APIs, and production infrastructure.



# 📊 Current Capabilities

The current system focuses on:

- Multi-agent architecture
- LangGraph-based orchestration
- Conditional agent routing
- SQL analysis workflows
- PostgreSQL integration
- ETL workflows
- Pandas-based data transformation
- Natural-language task routing
- Structured agent state



# 🎯 Future Vision

The long-term vision is to evolve this project into a **Data Intelligence Platform** capable of connecting multiple specialized agents to enterprise data systems.


# 📈 Planned Integration with Business Intelligence

This project provides a foundation for more advanced data intelligence workflows.

Future extensions can support:

- Business KPI analysis
- Data Platform Improvements[EX- CRM, ERP, Power BI, Dynamic 365]
- Automated data profiling
- MCP Integration
- Root cause analysis
- Anomaly detection
- Business insights
- Recommendation generation
- Automated reports
- Cloud deployment
- Interactive dashboards

This creates a natural path from a **data operations agent** toward a broader **AI-powered business intelligence system**.


# 🤝 Contributing

Contributions, suggestions, and improvements are welcome.

You can:

- Open an issue
- Suggest an improvement
- Propose a new agent
- Add a new data connector
- Improve testing and evaluation

# 📄 License

This project is available for educational, research, and development purposes.

