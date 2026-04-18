# FarmCare - Comprehensive Project Report

## 1. Project Overview
**FarmCare** is a smart, AI-driven digital platform designed specifically for Cardamom farmers in Kerala. The system leverages artificial intelligence to identify crop diseases, provides real-time market prices, allows for professional soil testing requests, and delivers bilingual support (English and Malayalam) to assist the farming community.

---

## 2. System Modules
Based on the architecture, the system is divided into several interconnected modules:

1. **User Management Module**: Handles registration, authentication, and role management (Farmer, Admin). It maintains user profiles, farm sizes, and location details.
2. **AI Crop Doctor Module (Disease Detection)**: Allows farmers to upload images of cardamom plants. It integrates with Google Gemini AI to analyze the image, detect diseases, and recommend treatments.
3. **Market Price Tracking Module**: Automatically scrapes and stores daily cardamom auction prices across various markets and grades to help farmers track price trends.
4. **Soil Testing Module**: Enables farmers to request professional soil testing. Admins can track, approve, schedule, and complete these requests.
5. **Community & Content Module**: Manages public content such as announcements, team member details, and a community gallery for sharing farm-related media.

---

## 3. Database Tables (Data Dictionary)

### T.1 User Profile
| Field Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | INT | Primary Key | Unique identifier |
| `user_id` | INT | Foreign Key, Not Null | Links to standard Auth User |
| `role` | VARCHAR(10) | Default 'farmer' | Role: 'farmer' or 'admin' |
| `phone` | VARCHAR(15) | Nullable | Contact number |
| `location` | VARCHAR(200) | Nullable | Village/Address |
| `district_id` | INT | Foreign Key, Nullable | Links to District |
| `farm_size` | DECIMAL(10,2) | Nullable | Farm size in acres |
| `profile_picture` | VARCHAR(100) | Nullable | Path to profile image |
| `bio` | TEXT | Nullable | User biography |
| `created_at` | DATETIME | Not Null | Record creation timestamp |

### T.2 AI Report
| Field Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | INT | Primary Key | Unique identifier |
| `farmer_id` | INT | Foreign Key, Not Null | Links to the requesting user |
| `image` | VARCHAR(100) | Not Null | Uploaded plant image path |
| `status` | VARCHAR(20) | Default 'pending' | Processing status |
| `disease_detected` | VARCHAR(200) | Nullable | Name of identified disease |
| `confidence_level` | VARCHAR(50) | Nullable | AI confidence percentage |
| `severity` | VARCHAR(50) | Nullable | Disease severity |
| `recommendation_id` | INT | Foreign Key, Nullable | Linked established recommendation |
| `gemini_response` | TEXT | Nullable | Raw AI analysis result |
| `created_at` | DATETIME | Not Null | Date and time requested |

### T.3 Soil Test Request
| Field Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | INT | Primary Key | Unique identifier |
| `farmer_id` | INT | Foreign Key, Not Null | Links to the requesting user |
| `farm_location` | VARCHAR(300) | Not Null | Specific location for test |
| `farm_size` | DECIMAL(10,2) | Not Null | Size of the farm |
| `status` | VARCHAR(20) | Default 'pending' | Request status |
| `scheduled_date` | DATE | Nullable | Date scheduled for test |
| `requested_at` | DATETIME | Not Null | Date of request creation |

### T.4 Cardamom Price
| Field Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | INT | Primary Key | Unique identifier |
| `market` | VARCHAR(200) | Not Null | Name of the market |
| `grade` | VARCHAR(20) | Default 'other' | Cardamom grade (8mm, 7mm, etc) |
| `modal_price` | DECIMAL(10,2) | Nullable | Average/Modal price |
| `date` | DATE | Not Null | Date of price record |
| `scraped_at` | DATETIME | Not Null | Timestamp of scrape operation |

### T.5 Disease Recommendation
| Field Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | INT | Primary Key | Unique identifier |
| `disease_name` | VARCHAR(200) | Unique, Not Null | Name of the disease |
| `description` | TEXT | Not Null | Disease overview |
| `symptoms` | TEXT | Not Null | Common symptoms |
| `medicine` | TEXT | Not Null | Recommended chemical/organic medicine |
| `severity` | VARCHAR(20) | Default 'medium' | General severity level |

---

## 4. Entity-Relationship (ER) Diagram (Reference Style)

Based on the provided reference style, this is a Chen-style ER Diagram with explicit Entities (Rectangles), Attributes (Ovals), and Relationships (Diamonds).

```mermaid
graph TD
    %% Entities
    U[USER]
    AR[AI_REPORT]
    ST[SOIL_TEST_REQUEST]
    D[DISTRICT]
    REC[DISEASE_RECOMMENDATION]

    %% Relationships
    Submits{Submits}
    Requests{Requests}
    LocatedIn{Located in}
    CategorizedBy{Categorized by}

    %% Connect Entities to Relationships
    U --- Submits
    Submits --- AR

    U --- Requests
    Requests --- ST

    U --- LocatedIn
    LocatedIn --- D

    AR --- CategorizedBy
    CategorizedBy --- REC

    %% Attributes for USER
    u1([id PK])
    u2([role])
    u3([username])
    u4([phone])
    U --- u1
    U --- u2
    U --- u3
    U --- u4

    %% Attributes for AI_REPORT
    ar1([id PK])
    ar2([status])
    ar3([disease])
    AR --- ar1
    AR --- ar2
    AR --- ar3
    
    %% Attributes for SOIL_TEST
    st1([id PK])
    st2([status])
    st3([location])
    ST --- st1
    ST --- st2
    ST --- st3

    %% Attributes for DISTRICT
    d1([id PK])
    d2([name])
    D --- d1
    D --- d2

    %% Attributes for RECOMMENDATION
    r1([id PK])
    r2([disease_name])
    r3([medicine])
    REC --- r1
    REC --- r2
    REC --- r3
```

---

## 5. Data Flow Diagrams (DFD)

### Comprehensive System DFD
Created to match the exact visual structure from your reference DFD image (using circles for processes, rectangles for users, and cylinders for collections).

```mermaid
graph TD
    %% Users / Actors
    Farmer[Farmers]
    Admin[Admin / Authority]
    
    %% Process Nodes
    UserMgmt((User Management))
    AIMgmt((AI Disease Management))
    SoilMgmt((Soil Test Management))
    PriceMgmt((Price Tracking Management))
    
    %% Data Stores (Collections)
    UsersDB[(users collection)]
    ReportsDB[(ai_reports collection)]
    SoilDB[(soil_requests collection)]
    PricesDB[(prices collection)]
    
    %% User Interactions
    Farmer -->|Register & Login| UserMgmt
    Admin -->|Manage Admins/Roles| UserMgmt
    
    %% Data Store Interactions for User
    UserMgmt -->|Store user datas| UsersDB
    UsersDB -->|Fetch user data| AIMgmt
    
    %% Interactions for AI Reports
    Farmer -->|Submit Crop Images| AIMgmt
    Admin -->|View All Reports| AIMgmt
    AIMgmt -->|Store Reports| ReportsDB
    ReportsDB -->|Fetch Reports| AIMgmt
    AIMgmt -->|Show Diagnostic Status| Farmer
    
    %% Interactions for Soil Testing
    Farmer -->|Request Soil Test| SoilMgmt
    Admin -->|Approve/Schedule Test| SoilMgmt
    SoilMgmt -->|Store Requests| SoilDB
    SoilDB -->|Fetch Requests| SoilMgmt
    SoilMgmt -->|Show Schedule Status| Farmer
    
    %% Interactions for Price Tracking
    Admin -->|Trigger Price Update Scraper| PriceMgmt
    PriceMgmt -->|Store Daily Prices| PricesDB
    PricesDB -->|Fetch Price Trends| PriceMgmt
    PriceMgmt -->|Show Live Prices| Farmer
```
