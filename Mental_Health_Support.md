# Mental Health Support Platform - Actionable Requirements

## Project Overview

Residents of the Waterloo Region face significant barriers in accessing mental health support due to fragmented services, stigma, cultural mismatches, and long wait times. Our platform aims to centralize mental health resources and support inclusive, timely, and stigma-free access for all residents.

This document outlines actionable, research-backed requirements and implementation strategies prioritized for structured delivery.

---

## Priority Levels

- 🔴 **High Priority** → Critical for core functionality.  
- 🟠 **Medium Priority** → Important but not immediately blocking.  
- 🟢 **Low Priority** → Enhancements or non-essential features.

---

## Actionable Requirements

### 1. Centralized Access to Mental Health Services

**User Story:**  
_As a resident seeking mental health support, I want a centralized platform to find local services easily so that I don’t get overwhelmed._

**Priority:** 🔴 High  
*GitHub Issue: #1*

#### Purpose

Based on community data from the Region of Waterloo Mental Wellness Strategy and reports such as Safe and Well WR – Mental Health Needs and the Analysis of Community Engagement Input, it is evident that mental health services in the region are fragmented, making it difficult for residents to access appropriate support (Region of Waterloo, 2023; Safe and Well WR, 2023).

To address this, we will implement a centralized directory platform that consolidates local mental health services, includes smart filters, and provides direct contact capabilities to simplify access and empower residents.

#### Implementation Strategy

- Build a directory to consolidate local services (e.g., CMHA, Here 24/7, family counseling).
- Add advanced filtering options based on service type, demographics, urgency, and accessibility.
- Integrate one-click contact mechanisms (e.g., call, email, directions).
- Design a responsive UI that supports both desktop and mobile users.

---

### Sub-Issues

#### Sub-Issue 1: We assume that centralizing access to mental health resources will reduce time-to-help and improve user satisfaction.

**Priority:** 🔴 High  
- Validate this assumption through A/B testing between a static resource list and the centralized directory.
- Collect time-to-locate-service metrics and user feedback.
- Track the number of completed referrals/contact attempts via the platform.

#### Sub-Issue 2: Design and Set Up Directory Database

**Priority:** 🟠 Medium  
**Goal:** Create a structured and searchable database of all local mental health services.  
**Approach:** Use SQLite/PostgreSQL to allow quick queries and flexible metadata filtering.  
**Tasks:**

- Define data schema with attributes (e.g., name, description, contact, service type, population).
- Implement indexing for search optimization.
- Create SQL scripts for table creation and sample data population.

**SQL Implementation Suggestion:**

```sql
CREATE TABLE mental_health_services (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    service_type VARCHAR(100),
    demographic_target VARCHAR(100),
    contact_email VARCHAR(255),
    contact_phone VARCHAR(50),
    urgency_level VARCHAR(50),
    description TEXT
);

CREATE INDEX idx_service_type ON mental_health_services(service_type);
```

#### Sub-Issue 3: Develop Backend API for Directory Access

**Priority:** 🔴 High  
**Goal:** Enable the front end and other systems to retrieve filtered results and contact details via REST endpoints.  
**Approach:** Build using Express.js or Django REST Framework.  
**Tasks:**

- ```GET /services``` to fetch all services with optional filters.
-  ```GET /services/:id``` to retrieve a specific service.
- ```POST /services``` to allow admin users to add new entries.
- Implement filtering query parameters (e.g., ```?type=emergency&demographic=youth```).

```python
@app.route('/services', methods=['GET'])
def get_services():
    """
    Fetch and filter mental health services.
    Returns:
        JSON list of services with contact and type details.
    """
```
#### Sub-Issue 4: Implement Frontend UI for Service Discovery

**Priority:** 🔴 High  
**Goal:** Provide users with an intuitive way to discover and connect with local services.  
**Tasks:**

- Design a card-based service listing UI with filterable panels.
- Show summary info: service name, brief description, and a 'Connect' button.
- Enable detailed view on click with full info and map directions.

#### Sub-Issue 5: Implement Search, Filter, and Contact Interaction

**Priority:** 🟠 Medium  
**Goal:** Improve discoverability and actionability of services.  
**Tasks:**

- Add a search bar for keyword-based queries.
- Add filter dropdowns (type, age group, urgency).
- Integrate phone and email action buttons.
- Use Google Maps API for directions integration.

#### Sub-Issue 6: Test and Validate Usability

**Priority:** 🟢 Low  
**Goal:** Ensure the platform significantly improves service discovery time and experience.  
**Tasks:**

- Conduct usability testing with at least 10 users from different age groups.
- Measure navigation time, success rate, and satisfaction.
- Use feedback to iterate UI/UX.

## 2. Timely Support and Reduced Wait Times

**User Story:**  
_As a person struggling with mental health, I want to get support quickly or know how long I’ll wait, so that I don’t feel ignored._

**Priority:** 🔴 High  
*GitHub Issue: #2*
## 2. Timely Support and Reduced Wait Times

**User Story:**  
_As a person struggling with mental health, I want to get support quickly or know how long I’ll wait, so that I don’t feel ignored._

**Priority:** 🔴 High  
*GitHub Issue: #2*

### Purpose

Through research of publicly available articles and regional mental health planning documents, we found that long wait times are a major barrier to timely mental health support in the Region of Waterloo (O’Neill et al., 2021; Waterloo Undergraduate Student Association, 2023).

To address this, we will implement real-time wait time estimates, mood tracking, and interim self-help tools to keep users informed, engaged, and emotionally supported while they await access to care.

### Implementation Strategy

- Display estimated wait times for each service provider.
- Provide interim support tools like mood check-ins, breathing exercises, and CBT modules.
- Notify users when their position in a waitlist progresses or an earlier appointment becomes available.
- Ensure all wait time and support data is accessible via API endpoints for reuse in other systems.

---

### Sub-Issues

#### Sub-Issue 1: We assume that showing estimated wait times and progress notifications will reduce user anxiety and increase service follow-through

**Priority:** 🔴 High  
- Validate this by conducting surveys and usage analytics across two groups: with and without the wait time display.
- Track user retention and engagement metrics over a 14-day waiting period.
- Compare help-seeking completion rates for both groups.

#### Sub-Issue 2: Design and Set Up Database for Wait Time Tracking

**Priority:** 🟠 Medium  
**Goal:** Create a scalable and structured table to store wait estimates, user progress, and notification history.  
**Approach:** Use PostgreSQL or SQLite with scheduled updates and indexing for query efficiency. 
**Tasks:**

- Design table to store average wait times, service type, urgency, and last updated timestamp.
- Track user-specific progress for queueing systems.
- Add notification flags and timestamps for update dispatch.

**SQL Implementation Suggestion:**
```sql
CREATE TABLE service_wait_times (
    id SERIAL PRIMARY KEY,
    service_name VARCHAR(255),
    average_wait_days INT,
    urgency_level VARCHAR(50),
    last_updated TIMESTAMP
);

CREATE TABLE user_wait_notifications (
    id SERIAL PRIMARY KEY,
    user_id INT,
    service_id INT,
    status VARCHAR(100),
    notify_at TIMESTAMP,
    is_notified BOOLEAN DEFAULT FALSE
);
```
#### Sub-Issue 3: Implement Self-Help Module and Mood Tracker

**Priority:** 🔴 High  
**Goal:** Keep users supported during the waiting period by offering useful tools.  
**Approach:** Provide a collection of guided breathing, journaling, and CBT content. 
**Tasks:**

- Design UI for mood check-in with emoji slider and notes.
- Embed calming resources: guided breathing, anxiety grounding exercises, etc.
- Store check-in history in local storage or backend DB.

**Python Implementation Suggestion:**
```python
def record_mood_checkin(user_id: int, mood_score: int, notes: str):
    """
    Stores a user's daily mental health mood check-in.
    """
```
#### Sub-Issue 4: Develop Backend API for Wait Time and Notifications

**Priority:** 🔴 High  
**Goal:** Make wait times and support tools programmatically accessible.  
**Approach:** Use Flask or Django REST Framework.  
**Tasks:**

- ```GET /wait_times``` – fetch current wait times.
- ```POST /mood_checkin``` – store mood entry.
- ```GET /user_notifications``` – fetch user alerts.
- ```POST /notify_user``` – update position or cancellation.

**Python Implementation Suggestion:**
```python
@app.route('/wait_times', methods=['GET'])
def get_wait_times():
    """
    Returns current average wait times by service.
    """
```
#### Sub-Issue 5: Implement UI for Wait Time Display and Tools

**Priority:** 🟠 Medium  
**Goal:** Display real-time wait times and enable access to support tools.
**Tasks:**

- Show estimated wait time below each service name (e.g., “3 days avg.”).
- Add visual indicators like progress bars or countdown timers.
- Provide a “Coping Tools” tab for self-help resources.
- Alert user when their waitlist position improves.


#### Sub-Issue 6: Handle Notifications and User Preferences

**Priority:** 🟢 Low  
**Goal:** Keep users updated via preferred channels (email, SMS, in-app).
**Tasks:**

- Add toggle for user to opt-in/out of notifications.
- Queue and send alerts via `notify_user()` function.
- Show in-app status updates (e.g., “You’ve moved up the queue!”).

## 3. Destigmatization and Mental Health Literacy

**User Story:**  
_As a person hesitant to seek help, I want to access anonymous and educational resources, so I can learn and engage safely._

**Priority:** 🔴 High  
*GitHub Issue: #3*

### Purpose

Using insights gathered from mental health organizations and peer-reviewed studies, we concluded that stigma remains a significant barrier preventing individuals—particularly youth—from seeking mental health support (CMHA Ontario, 2022; BMC Health Services Research, 2021).

To address this, we will implement anonymous educational tools, moderated discussion forums, and gamified content to promote literacy and safe engagement. These features will encourage open learning without fear of judgment, helping to normalize mental health conversations and encourage early help-seeking behavior.

### Implementation Strategy

- Develop a library of culturally sensitive articles, videos, and resources.
- Enable anonymous access to Q&A forums and moderated discussions.
- Introduce gamification to increase engagement (badges, streaks, points).
- Track content usage and sentiment to measure literacy improvements.

---

### Sub-Issues

#### Sub-Issue 1: We assume that anonymous access and relatable content will reduce stigma and encourage participation.

**Priority:** 🔴 High  
- Validate this assumption through anonymous feedback surveys and content completion metrics.
- Compare user engagement across anonymous vs. non-anonymous modules.
- Track user return rates and time spent in education sections.
