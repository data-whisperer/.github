# Job Search & Matching System  

This project is a **Job Search Platform** that fetches jobs from multiple job boards and intelligently matches them with user profiles using AI/ML models.  

The goal is to **deliver highly relevant job recommendations** based on user preferences, resumes, and keyword-driven searches.  

---

## ⚙️ Architecture


<img width="6676" height="5186" alt="JobMatchPlatform" src="https://github.com/user-attachments/assets/c043d7c0-11a5-4e21-b926-22c532b567a9" />

---


## 🚀 Functional Requirements  

### 1. User Management System  
- User Registration & Authentication  
- Profile creation & management  
- Job preferences (location, role, type, experience)  
- Resume upload & analysis (extract skills, roles, keywords)  

### 2. Resume Analysis System  
- Extract skills & keywords from uploaded resumes  
- Store structured data in user profiles  

### 3. Keyword Generator (Worker)  
- Uses ML/LLM models to generate relevant keywords from user profiles and preferences  
- Stores generated keywords in queue for job fetching  

### 4. Cron Job (Scheduler)  
- Triggers keyword-based searches every day  
- Initiates job fetching pipeline  

### 5. Job Fetch System (Worker)  
- Fetches jobs from multiple job boards using APIs/scrapers  
- Stores job descriptions in cache & database  
- Ensures deduplication using idempotency keys  

### 6. Job Processor System (Worker)  
- Talks to AI/LLM model to score relevance of a job against a user profile  
- Produces a “match score”  

### 7. Job Management System  
- Stores job postings in DB  
- Handles job deduplication, updates & queries  
- Provides APIs for frontend  

---

## ⚙️ Non-Functional Requirements  

### 1. Consistency vs Availability  
- A job posting can afford **eventual consistency**.  
- Availability is prioritized over strict consistency.  

### 2. Idempotency  
- Every job fetched is assigned a **unique idempotency key**.  
- Prevents duplicate processing, reducing AI/LLM computation costs.  

### 3. Scalability & Throughput  
- Designed with workers, queues, and caches for horizontal scaling.  
- Load balancers manage worker nodes.  

---

## 🔄 Workflow  

1. User registers & sets job preferences  
2. Resume uploaded → analyzed → keywords generated  
3. Cron job triggers job fetching daily  
4. Job Fetch Worker pulls jobs from job boards  
5. Jobs stored in DB + Cache (with idempotency check)  
6. Job Processor Worker calls AI/LLM to score job relevance  
7. Jobs ranked & served to frontend for user  


