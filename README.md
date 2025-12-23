-- If DNS/hostname works, you can use hostname.
ALTER SYSTEM SET LOCAL_LISTENER =
'(ADDRESS=(PROTOCOL=TCP)(HOST=your-hostname-or-ip)(PORT=1521))' SCOPE=BOTH;

-- Make sure the DB knows its service name (usually already set)
SHOW PARAMETER service_names;

-- Force immediate registration
ALTER SYSTEM REGISTER;

