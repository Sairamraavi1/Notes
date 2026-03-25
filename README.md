SELECT username, account_status, created
FROM dba_users
WHERE username = 'SNF_ECC_BODS_USER';

SELECT username, created
FROM dba_users
WHERE username = 'SNF_ECC_BODS_USER';

SELECT * 
FROM dba_role_privs 
WHERE grantee = 'SNF_ECC_BODS_USER';


SELECT * 
FROM dba_sys_privs 
WHERE grantee = 'SNF_ECC_BODS_USER';

SELECT * 
FROM dba_tab_privs 
WHERE grantee = 'SNF_ECC_BODS_USER';