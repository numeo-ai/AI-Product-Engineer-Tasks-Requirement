### Task  
Build a simple **customer support email agent** with the following requirements:  

#### Client App (minimal functionality)  
- Allow users to connect their Gmail account.  
- Support disconnecting an account or adding multiple Gmail accounts.  
- Nothing else is required for the client UI.  

#### Backend  
- Continuously listen for incoming emails.  
- When an email arrives, pass it to an agent that categorizes the email into one of three categories:  

1. **Question**  
   - Use a **RAG (Retrieval-Augmented Generation)** approach for answering questions.  
   - If the system cannot provide an answer, save the email as **unhandled** in PostgreSQL with **high importance**.  

2. **Refund**  
   - Maintain a **Postgres table** with a list of orders.  
   - If a refund request email is received:  
     - If the order ID is missing, ask the user for it.  
     - If the ID is provided:  
       - Check if it exists in the DB.  
       - If found → mark it as refund requested and send a response that the refund will be processed within 3 days.  
       - If not found → respond that the order ID is invalid.  
         - If the user replies again with the same invalid ID → create a row in a table for **not found refund requests**.  
         - If the user replies with something else (but not an ID) → also log it in **not found refund requests**.  

3. **Other / Non-sense Emails**  
   - Save them in a **Postgres table** for unhandled emails.  
   - Include a field indicating the assessed **importance level**.  

---

## Tech Stack  
- Python  
- PostgreSQL  
- Gmail API (or similar)  
