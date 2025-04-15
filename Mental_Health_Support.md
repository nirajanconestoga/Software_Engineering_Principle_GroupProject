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

#### Sub-Issue 2: Design and Set Up Database for Educational Content and User Events

**Priority:** 🟠 Medium  
**Goal:**  Create structured storage for mental health resources, forum entries, and user engagement logs.  
**Approach:**  Use PostgreSQL with tagging and audit trail for moderation.
**Tasks:**

- Create tables for articles, forum posts, tags, and views.
- Store anonymity flags and timestamps for user submissions.
- Link educational content with learning badges.

**SQL Implementation Suggestion:**
```sql
CREATE TABLE mental_health_articles (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255),
    content TEXT,
    tags TEXT[],
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE forum_posts (
    id SERIAL PRIMARY KEY,
    post_body TEXT,
    is_anonymous BOOLEAN DEFAULT TRUE,
    user_id INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE user_engagement (
    id SERIAL PRIMARY KEY,
    user_id INT,
    article_id INT,
    completed BOOLEAN,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### Sub-Issue 3: Implement Anonymous Educational Platform and Forum

**Priority:** 🔴 High  
**Goal:** Create a judgment-free space to learn and engage in mental health topics. 
**Approach:** Design UI/UX to support safe, anonymous engagement.  
**Tasks:**

- Build UI for browsing and completing educational resources.
- Implement dropdowns or quizzes embedded in content.
- Create an anonymous forum with basic posting and comment features.
- Moderate content automatically with keyword flags.

**Python Implementation Suggestion:**
```python
def post_anonymous_question(content: str, user_id: int = None):
    """
    Saves a new forum post anonymously or linked to user_id if provided.
    """
```

#### Sub-Issue 4: Develop Backend API for Forum, Resources, and Quizzes

**Priority:** 🔴 High  
**Goal:** Enable all features programmatically for future reuse or mobile version. 
**Approach:** Use Django REST Framework or Flask with JWT support. 
**Tasks:**

- `GET /articles` – return educational materials
- `POST /forum` – submit anonymous post
- `POST /quiz_result` – log badge progress
- Add moderation endpoints to allow admin content review.

**Python Implementation Suggestion:**
```python
@app.route('/forum', methods=['POST'])
def submit_forum_post():
    """
    Submits a new anonymous forum post for review or publishing.
    """
```

#### Sub-Issue 5: Build UI for Literacy Progress and Gamification

**Priority:** 🟠 Medium  
**Goal:** Motivate users to learn consistently through small rewards.  
**Tasks:**

- Design progress tracker showing completed articles and badges earned.
- Add a streak counter or XP system for continued engagement.
- Display motivational messages on completion.
- Allow users to view learning milestones anonymously.

#### Sub-Issue 6: Measure Impact on Literacy and Stigma

**Priority:** 🟢 Low  
**Goal:** Assess if platform tools are reducing stigma and improving knowledge.

**Tasks:**

- Conduct pre- and post-intervention quizzes.
- Analyze forum sentiment with NLP.
- Share anonymous success stories or testimonials.

## 4. Cultural and Language Inclusivity

**User Story:**  
_As a newcomer or diverse resident, I want support in my language and culture, so I can access help without misunderstanding._

**Priority:** 🔴 High  
*GitHub Issue: #4*

### Purpose

Based on public data and regional studies, we observed that language and cultural mismatches significantly hinder access to mental health services for many residents—particularly newcomers and culturally diverse populations (CYRRC, 2021).

To address this, we will implement a multilingual and culturally adaptive interface that provides personalized support for users based on their language and cultural context. This will increase accessibility, user trust, and service utilization among underserved communities.

### Implementation Strategy

- Translate the entire user interface and core content into the region’s top 5 non-English languages.
- Curate and localize mental health resources to be culturally relevant.
- Add filters and labels for language preferences and cultural identity when searching for services.
- Ensure interface directionality and accessibility standards (e.g., RTL support for Arabic).


---

### Sub-Issues

#### Sub-Issue 1: We assume that offering localized, translated resources will significantly improve platform accessibility for multicultural users.

**Priority:** 🔴 High  
- Validate this by testing comprehension and comfort levels among speakers of each supported language.
- Track bounce rates and completion rates of support flows by language group.
- Run A/B comparisons between translated and non-translated UI sessions.

#### Sub-Issue 2: Design and Set Up Language and Cultural Resource Database

**Priority:** 🟠 Medium  
**Goal:** Store localized versions of UI strings, educational content, and culturally adapted recommendations.  
**Approach:** Use structured tables with language codes, cultural tags, and fallback defaults. 
**Tasks:**

- Create translation key-value store (e.g., en-US, fr-CA, ur-PK).
- Create cultural metadata schemas for service listings.
- Link resources to specific cultural identities and user profiles.

**SQL Implementation Suggestion:**
```sql
CREATE TABLE translations (
    id SERIAL PRIMARY KEY,
    key VARCHAR(255),
    language_code VARCHAR(10),
    translated_text TEXT
);

CREATE TABLE cultural_tags (
    id SERIAL PRIMARY KEY,
    service_id INT,
    culture VARCHAR(100),
    language VARCHAR(50)
);
```
#### Sub-Issue 3: Implement Multilingual UI and Language Toggle

**Priority:** 🔴 High  
**Goal:** Allow users to interact with the platform in their preferred language.  
**Approach:** Use i18n libraries like `i18next` or `Vue-i18n` to support frontend localization.
**Tasks:**

- Implement language selector dropdown on homepage.
- Ensure translated versions of all navigation, forms, and support flows.
- Integrate language detection from browser or user profile.

**Frontend Example:**
```html
<i18n-t keypath="welcome_message"></i18n-t>
```

#### Sub-Issue 4: Culturally Adapt Resource Content and Tags

**Priority:** 🟠 Medium  
**Goal:**  Ensure resources resonate with diverse backgrounds and norms. 
**Tasks:**

- Audit existing educational content for cultural bias or assumptions.
- Create alternative narratives/examples aligned with community values.
- Tag service listings with “Culturally Appropriate”, “Language-Specific”, etc.

#### Sub-Issue 5: Conduct Multicultural Usability Testing

**Priority:** 🟢 Low  
**Goal:** Ensure translated and localized experiences are effective and intuitive.
 experiences  
**Tasks:**

- Recruit 2–3 users per language group for usability testing.
- Monitor session flows, task success, and qualitative feedback.
- Refine translations, interface spacing, and directionality (LTR/RTL).
