# 🤖 Multi-Agent Data Intelligence System

An intelligent multi-agent system designed to automate data analysis, SQL querying, and ETL workflows using natural language.

The system uses specialized AI agents orchestrated through **LangGraph** to understand a user's request, route it to the appropriate agent, execute data operations using deterministic tools, and return meaningful results.

Instead of using a single AI agent for every task, the system follows a **specialized multi-agent architecture**, where each agent is responsible for a specific type of data operation.

---

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

---

# 💡 Solution

The system uses a **Router Agent** to understand the user's request and determine which specialized agent should handle the task.

The current architecture includes two primary specialized agents:

- **SQL Analyst Agent**
- **ETL Analyst Agent**

The Router Agent analyzes the user's request and routes it to the appropriate agent.

```text
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
```

---

# 🧠 Multi-Agent Architecture

## 1. Router Agent

The Router Agent acts as the entry point for user requests.

Its responsibility is to:

- Understand the user's natural-language request
- Identify the type of task
- Select the appropriate specialized agent
- Route the request through the LangGraph workflow

Example:

```text
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
```

The Router Agent ensures that specialized tasks are handled by the agent best suited for the operation.

---

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

```text
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
```

The agent uses database tools to execute the query and retrieve actual results.

---

## 3. ETL Analyst Agent

The ETL Analyst Agent handles data extraction and transformation workflows.

ETL stands for:

```text
E → Extract
T → Transform
L → Load
```

The agent can handle tasks such as:

- Reading CSV files
- Processing datasets
- Calling APIs
- Cleaning data
- Transforming columns
- Handling missing values
- Filtering records
- Converting data formats
- Preparing structured datasets

Example workflow:

```text
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
```

The ETL agent uses Python and Pandas-based operations to process data.

---

# ⚙️ System Workflow

The complete workflow follows the sequence below:

```text
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
```

---

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

---

# 📁 Project Architecture
Architecture

The system follows a hierarchical agent architecture:

```
┌─────────────────────────────────────────────────────────────┐
│         Multi_Agent-Data-Intelligence (Router)              │
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

```text
Multi-Agent-Data-Intelligence-System/
│
├── agents/
│   ├── router_agent.py
│   ├── sql_agent.py
│   └── etl_agent.py
│
├── graph/
│   └── workflow.py
│
├── tools/
│   ├── sql_tools.py
│   └── etl_tools.py
│
├── database/
│   ├── connection.py
│   └── models.py
│
├── state/
│   └── agent_state.py
│
├── config/
│   └── settings.py
│
├── tests/
│
├── requirements.txt
│
├── .env.example
│
├── main.py
│
└── README.md
```

> The exact folder structure may evolve as the system is extended with additional agents, tools, APIs, and production infrastructure.

---

# 🚀 Installation

## 1. Clone the Repository

```bash
git clone https://github.com/curiousNIKITA/Multi_Agent-Data-Intelligence-System.git
cd Multi_Agent-Data-Intelligence-System
```

---

## 2. Create a Virtual Environment

```bash
python -m venv .venv
```

Activate the environment.

### Windows

```bash
.venv\Scripts\activate
```

### macOS/Linux

```bash
source .venv/bin/activate
```

---

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 4. Configure Environment Variables

Create a `.env` file:

```bash
cp .env.example .env
```

Configure the required environment variables for:

- LLM provider
- API keys
- PostgreSQL connection
- Application configuration

Example:

```env
DATABASE_URL=postgresql://username:password@localhost:5432/database_name

LLM_PROVIDER=your_provider
LLM_MODEL=your_model
```

Never commit API keys or passwords to GitHub.

---

# 💻 Example Usage

## SQL Analysis

User request:

```text
Show me the top 5 customers by revenue.
```

System workflow:

```text
User Request
      ↓
Router Agent
      ↓
SQL Analyst Agent
      ↓
Generate SQL
      ↓
Execute Query
      ↓
PostgreSQL
      ↓
Return Results
```

---

## ETL Workflow

User request:

```text
Load a CSV file, remove duplicate records, handle missing values, and save the cleaned data.
```

System workflow:

```text
User Request
      ↓
Router Agent
      ↓
ETL Analyst Agent
      ↓
Read Dataset
      ↓
Transform Data
      ↓
Clean Data
      ↓
Return Processed Result
```

---

# 🔐 Safety and Validation

The system is designed with validation around data and query operations.

Current safety considerations include:

- Structured input handling
- SQL query validation
- Controlled database operations
- Environment-based credential management
- Separation between agent reasoning and data tools

As the system evolves, additional production security controls can include:

- Role-based access control
- Authentication and authorization
- Query permission policies
- Rate limiting
- Audit logging
- Isolated execution environments
- Resource limits
- Secret management

---

# 🧪 Testing

Run the test suite using:

```bash
pytest -v
```

Testing can cover:

- Agent routing
- SQL operations
- ETL transformations
- State transitions
- Tool execution
- Error handling

---

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

---

# 🚧 Production Roadmap

The project is being extended toward a more scalable production architecture.

## Phase 1 — Core Multi-Agent System

- [x] Router Agent
- [x] SQL Analyst Agent
- [x] ETL Analyst Agent
- [x] LangGraph orchestration
- [x] Conditional routing

## Phase 2 — Data Platform Improvements

- [ ] CSV upload API
- [ ] Excel support
- [ ] Google Sheets connector
- [ ] Additional database connecter
- [ ] Dataset profiling
- [ ] Data quality validation

## Phase 3 — Production API Layer

- [ ] FastAPI backend
- [ ] REST API endpoints
- [ ] Request validation
- [ ] Authentication
- [ ] Rate limiting

## Phase 4 — Advanced Agent Architecture

- [ ] Supervisor/Orchestrator Agent
- [ ] Data Quality Agent
- [ ] Analysis Agent
- [ ] Insight Agent
- [ ] Agent collaboration workflows

## Phase 5 — MCP Integration

Planned integration with the Model Context Protocol for standardized tool connectivity.

Potential capabilities include:

```text
AI Agent
    │
    ▼
MCP Client
    │
    ▼
MCP Server
    │
 ┌──┼─────────────┐
 ▼  ▼             ▼
SQL Data      Files       External APIs
```

This can allow agents to access tools through standardized interfaces.

---

## Phase 6 — Scalability

Future architecture may include:

- Async task processing
- Background workers
- Distributed processing
- Caching
- Containerization
- Cloud deployment
- Horizontal scaling

---

## Phase 7 — Observability

Future production improvements can include:

- Structured logging
- Agent tracing
- Tool execution tracking
- Error monitoring
- Performance metrics
- Cost monitoring
- Request auditing

---

# 🎯 Future Vision

The long-term vision is to evolve this project into a **Data Intelligence Platform** capable of connecting multiple specialized agents to enterprise data systems.

A potential future architecture could look like:

```text
                         Business User
                              │
                              ▼
                      Natural Language
                              │
                              ▼
                    Supervisor Agent
                              │
       ┌──────────────────────┼──────────────────────┐
       │                      │                      │
       ▼                      ▼                      ▼
  SQL Agent              ETL Agent           Data Quality Agent
       │                      │                      │
       └──────────────────────┼──────────────────────┘
                              │
                              ▼
                        Data Tools
                              │
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
        ▼                     ▼                     ▼
    PostgreSQL              Files                APIs
                              │
                              ▼
                       Final Response
```

---

# 🎓 What This Project Demonstrates

This project demonstrates practical experience with:

- Multi-agent system design
- LangGraph
- Agent routing
- Conditional workflows
- Specialized AI agents
- LLM orchestration
- SQL automation
- PostgreSQL
- ETL pipelines
- Pandas
- Tool-based AI systems
- Structured state management
- Pydantic validation
- Production-oriented AI architecture

---


The project focuses not only on LLM interaction but also on connecting AI agents to real data systems and deterministic tools.

---

# 👨‍💻 Project Information

**Multi-Agent Data Intelligence System** is an engineering project focused on exploring and building practical multi-agent workflows for data analysis and data operations.

The architecture follows the principle that:

> **AI agents should orchestrate and reason about tasks, while deterministic tools perform database queries and data transformations.**

This separation helps make data operations more reliable, traceable, and easier to extend.

---

# 📈 Planned Integration with Business Intelligence

This project provides a foundation for more advanced data intelligence workflows.

Future extensions can support:

- Business KPI analysis
- Automated data profiling
- Root cause analysis
- Anomaly detection
- Business insights
- Recommendation generation
- Automated reports
- Interactive dashboards

This creates a natural path from a **data operations agent** toward a broader **AI-powered business intelligence system**.

---

# 🤝 Contributing

Contributions, suggestions, and improvements are welcome.

You can:

- Open an issue
- Suggest an improvement
- Propose a new agent
- Add a new data connector
- Improve testing and evaluation

---

# 📄 License

This project is available for educational, research, and development purposes.

Add an appropriate license file to the repository based on your preferred usage model.
