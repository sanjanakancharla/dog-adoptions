# Spring AI Claude Integration with PostgreSQL, Chat Memory, and RAG

This project demonstrates how to integrate Claude via the Anthropic API using Spring AI's `ChatClient`, persist chat memory in PostgreSQL, and implement Retrieval Augmented Generation (RAG) using PostgresML as a vector store.

## Features

### 1. Claude / Anthropic integration
Integrated the Claude / Anthropic API and used Spring AI's `ChatClient` API to send user questions as prompts to the chat model.

### 2. PostgreSQL as chat memory store
Used PostgreSQL as a persistent chat memory store.

The following tables are present in the database:

```sql
List of relations
 Schema |         Name          | Type  |   Owner
--------+-----------------------+-------+------------
 public | dog                   | table | postgresml
 public | spring_ai_chat_memory | table | postgresml
 public | vector_store          | table | postgresml
(3 rows)

Structure of the spring_ai_chat_memory table:

Table "public.spring_ai_chat_memory"
     Column      |            Type             | Collation | Nullable | Default
-----------------+-----------------------------+-----------+----------+---------
 conversation_id | character varying(36)       |           | not null |
 content         | text                        |           | not null |
 type            | character varying(10)       |           | not null |
 timestamp       | timestamp without time zone |           | not null |

Sample data from spring_ai_chat_memory:

select * from spring_ai_chat_memory;

 conversation_id |                              content                               |   type    |        timestamp
-----------------+--------------------------------------------------------------------+-----------+-------------------------
 jlong           | "i ate pizza yesterday"                                            | USER      | 2026-04-13 22:59:38.565
 jlong           | "i ate pizza yesterday"                                            | USER      | 2026-04-13 22:59:38.566
 jlong           | I'll remember that you ate pizza yesterday.                        | ASSISTANT | 2026-04-13 22:59:38.567



3. Actuator metrics integration

The Actuator module collects application metrics such as token usage.

Using Prometheus, these metrics can be exported to time series monitoring systems such as:
Prometheus
Datadog

4. Retrieval Augmented Generation (RAG) with vector store

Implemented RAG using PostgresML as the vector store.

All the data present in the data.sql file is loaded into Dog repository entities and then stored in the vector_store table in PostgresML.

This loading is done during instance initialization inside the constructor.

A QuestionAnswerAdvisor also needs to be configured so that the ChatClient can use the vector store during retrieval and question answering.

Setup
Run Docker container for PostgreSQL
chmod +x run.sh
./run.sh
Open a PostgreSQL terminal inside the Docker container

This command opens a PostgreSQL terminal inside the Docker container, connected to the postgresml database as the postgres user.

docker exec -it -u postgres objective_easley psql -d postgresml


How it works
User questions are sent through Spring AI's ChatClient.
Claude processes the prompt using the Anthropic API.
Conversation history is stored in the spring_ai_chat_memory table.
Application metrics such as token usage are collected through Actuator.
Documents are loaded into the vector store for retrieval.
QuestionAnswerAdvisor enables the application to retrieve relevant context before generating responses.



Tech Stack
Java
Spring Boot
Spring AI
Claude / Anthropic API
PostgreSQL
PostgresML
Docker
Prometheus
References
Your First Spring AI App
Josh Long Reference Project

A few small fixes I made so preview will not break:
- wrapped all SQL output in code blocks
- fixed headings and spacing
- cleaned grammar
- converted broken numbering into proper Markdown sections
- used backticks for table names, files, and APIs

One sentence I would improve technically is this one:

> “We also need to configure a QuestionAnswerAdvisor so that the chatClient will know to store the documents in the vector”

A better version is:

```markdown
A `QuestionAnswerAdvisor` needs to be configured so that the `ChatClient` can retrieve relevant documents from the vector store during question answering.

Because the advisor usually helps with retrieval, not storing.
