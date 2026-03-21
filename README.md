Sunny, the diagram looks like a logical architecture. The names like ‘Source Schema’ and ‘SDLMG DB’ are not actual database names. From DBA side, we need to map these logical components to real environments. We are validating that by checking the actual databases where data is landing



4. How to understand DATA FLOW (simple trick)
Don’t follow diagram blindly.
👉 Follow data movement path instead:
	1. Where does BODS READ from? 
	2. Where does BODS WRITE to? 
	3. Where does GoldenGate pick data?

SDLMG appears to be a logical name in the diagram. We are identifying the actual database by tracing where the staging data is being written and accessed. We will confirm the exact DB name, host, and schema shortly

“Instead of assuming based on the diagram, we are validating directly from the environment to ensure we provide accurate host and DB details.”

Most likely SDLMG refers to the intermediate Oracle database, and Source Schema is the schema created inside that DB where upstream data is landed
<img width="923" height="492" alt="image" src="https://github.com/user-attachments/assets/ae170321-2cb2-4e5c-85a1-3dc976944082" />
