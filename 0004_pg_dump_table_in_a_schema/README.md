= Issue

In PostgreSQL, I needed to dump a single table, but it lived in a schema that wasn't the "public" schema

I had tried something like:

```bash
pg_dump -h localhost -n vocabulary -t drug_strength db_name_here
```

But the -t options invalidates the -n option (WHY!?!?!) and so pg_dump said it couldn't find the table I wanted.

= Solution

```bash
pg_dump --file "/tmp/ds.sql" --host "localhost" --port "5432" --table "vocabulary.drug_strength" "conceptql_test_2"
```

Essentially, just put the name of the schema in as part of the name of the table you want to dump.
