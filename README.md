BEGIN
  FOR obj IN (
    SELECT object_name
    FROM dba_objects
    WHERE owner = 'PSERP_R'
      AND object_type IN ('TABLE', 'VIEW')
  )
  LOOP
    EXECUTE IMMEDIATE
      'GRANT SELECT ON PSERP_R.' || obj.object_name || ' TO PSERP_R_READ_ROLE';
  END LOOP;
END;
/
