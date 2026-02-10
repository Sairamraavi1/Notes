SELECT column_name, data_type, data_length
FROM dba_tab_columns
WHERE owner = 'SCHEMA_NAME'
  AND table_name = 'TABLE_NAME'
  AND column_name = 'STCD1';

  SELECT column_name
FROM user_tab_columns
WHERE table_name = 'TABLE_NAME'
  AND column_name = 'STCD1';


SELECT owner, table_name
FROM dba_tab_columns
WHERE owner = 'SCHEMA_NAME'
  AND column_name = 'STCD1'
ORDER BY table_name;


SELECT COUNT(*)
FROM SCHEMA_NAME.TABLE_NAME
WHERE STCD1 IS NOT NULL;


SELECT table_name
FROM all_tab_columns
WHERE owner = 'SCHEMA_NAME'
  AND column_name = 'STCD1'
ORDER BY table_name;
