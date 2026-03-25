SELECT username,
       account_status,
       created,
       profile,
       default_tablespace,
       temporary_tablespace
FROM dba_users
WHERE username IN (
  'SVC_ELV_ORA_BODS_USER',
  'SVC_ELV_ORA_R_USER',
  'SVC_ELV_ORA_CYBER_USER'
);


SELECT grantee,
       granted_role,
       admin_option,
       default_role
FROM dba_role_privs
WHERE grantee IN (
  'SVC_ELV_ORA_BODS_USER',
  'SVC_ELV_ORA_R_USER',
  'SVC_ELV_ORA_CYBER_USER'
)
ORDER BY grantee, granted_role;

SELECT grantee,
       privilege,
       admin_option
FROM dba_sys_privs
WHERE grantee IN (
  'SVC_ELV_ORA_BODS_USER',
  'SVC_ELV_ORA_R_USER',
  'SVC_ELV_ORA_CYBER_USER'
)
ORDER BY grantee, privilege;

SELECT grantee,
       owner,
       table_name,
       privilege
FROM dba_tab_privs
WHERE grantee IN (
  'SVC_ELV_ORA_BODS_USER',
  'SVC_ELV_ORA_R_USER',
  'SVC_ELV_ORA_CYBER_USER'
)
ORDER BY grantee, owner, table_name, privilege;