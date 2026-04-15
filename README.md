1.Integrated Claude/Anthropic API key and using spring's ChatClient API to send user  questions as prompt to the chat model or LLM's- Claude

Used Postgress as Vector store as well as Chat Memory store.
List of relations
Schema |         Name          | Type  |   Owner    
--------+-----------------------+-------+------------
public | dog                   | table | postgresml
public | spring_ai_chat_memory | table | postgresml
public | vector_store          | table | postgresml
(3 rows)

Table "public.spring_ai_chat_memory"
Column      |            Type             | Collation | Nullable | Default
-----------------+-----------------------------+-----------+----------+---------
conversation_id | character varying(36)       |           | not null |
content         | text                        |           | not null |
type            | character varying(10)       |           | not null |
timestamp       | timestamp without time zone |           | not null | 

postgresml=# select * from spring_ai_chat_memory;
conversation_id |                                                             content                                                              |   type    |        timestamp        
-----------------+----------------------------------------------------------------------------------------------------------------------------------+-----------+-------------------------
jlong           | "i ate pizza yesterday"                                                                                                          | USER      | 2026-04-13 22:59:38.565
jlong           | "i ate pizza yesterday"                                                                                                          | USER      | 2026-04-13 22:59:38.566
jlong           | I'll remember that you ate pizza yesterday.                                                                                     +| ASSISTANT | 2026-04-13 22:59:38.567
|                                                                                                                                 +|           |
