exec dbms_stats.gather_schema_stats(
  ownname => 'PSERP_R',
  estimate_percent => DBMS_STATS.AUTO_SAMPLE_SIZE,
  degree => 8,
  cascade => true
);
