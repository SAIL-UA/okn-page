---
layout: default
title: "API (Old Version)"
---

## Overview
*by Fabricio Gutierrez Juarez (fjgutierrez@ua.edu) • last updated October 24, 2025*

The RuralKG API Backend provides an intelligent query processing system that automatically classifies and routes questions to the most appropriate service. This comprehensive documentation covers all available endpoints, their message formats, and practical use cases for OKN Justice teams.

## Base URL

```
http://52.170.155.134:8001/
```

## Authentication

Currently no authentication required. (Add authentication details if applicable)

---

## 🎯 Primary Endpoints (Recommended for Team Use)

### 1. `/query` - Main Query Endpoint (RECOMMENDED)

**Purpose:** Clean, organized endpoint for processing any type of query with automatic classification and routing.

**Method:** `POST`

**Request Format:**

```json
{
  "question": "string (required)",
  "top_k": "integer (optional, default: 5)",
  "use_synthesis": "boolean (optional, default: false)"
}
```

**Response Format:**

```json
{
  "success": true,
  "query": "Original question",
  "query_type": "data_query|knowledge_query|service_query",
  "routing_decision": "analyze|knowledge|search",
  "processing_time_ms": 1234.56,
  "response_data": {
    /* Service-specific response data */
  },
  "preprocessing": {
    /* Query analysis metadata */
  },
  "error": null
}
```

**Example Use Cases:**

1. **Data Query Example:**

```bash
curl -X POST "http://52.170.155.134:8001/query" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "What percentage of adults have depression?",
    "top_k": 10,
    "use_synthesis": true
  }'
```

2. **Knowledge Query Example:**

```bash
curl -X POST "http://52.170.155.134:8001/query" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "What are the symptoms of anxiety disorders?",
    "use_synthesis": true
  }'
```

3. **Service Query Example:**

```bash
curl -X POST "http://52.170.155.134:8001/query" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "Find mental health services near me in Los Angeles",
    "top_k": 5
  }'
```

### 2. `/classify` - Smart Router (Alternative to /query)

**Purpose:** Automatically classify and route queries to the appropriate endpoint.

**Method:** `POST`

**Request Format:**

```json
{
  "question": "string (required)",
  "top_k": "integer (optional, default: 5)",
  "use_synthesis": "boolean (optional, default: false)",
  "county": "string (optional)",
  "state": "string (optional)"
}
```

**Response Format:** Same as `/query` endpoint.

---

## 🔍 Specialized Endpoints

### 3. `/preprocess` - Query Analysis

**Purpose:** Analyze and extract metadata from user queries.

**Method:** `POST`

**Request Format:**

```json
{
  "question": "string (required)"
}
```

**Response Format:**

```json
{
  "reasoning": "Explanation of query analysis",
  "should_split": false,
  "sub_queries": [],
  "original_query": "Original question",
  "query_type": "data_query|knowledge_query|service_query",
  "entities": ["entity1", "entity2"],
  "relationships": ["relationship1"],
  "location_context": "Geographic context",
  "city": "Extracted city",
  "county": "Extracted county",
  "state": "Extracted state",
  "analysis_focus": "Main focus area",
  "data_types": ["required", "data", "types"],
  "complexity_score": 7.5,
  "miles": null
}
```

**Example:**

```bash
curl -X POST "http://52.170.155.134:8001/preprocess" \
  -H "Content-Type: application/json" \
  -d '{"question": "What is the depression rate in California?"}'
```

### 4. `/analyze` - Data Query Analysis

**Purpose:** Analyze data queries using variable search across multiple datasets.

**Method:** `POST`

**Request Format:**

```json
{
  "question": "string (required)",
  "include_synthesis": "boolean (optional, default: true)",
  "top_k": "integer (optional, default: 5, max: 20)"
}
```

**Response Format:**

```json
{
  "query": "Original question",
  "query_type": "data_query",
  "detected_datasets": [1, 2, 3],
  "selected_variable": {
    "id": "var_123",
    "code": "DEPRESSION",
    "content": "Depression in past year",
    "description": "Variable description",
    "dataset": 1,
    "year": 2022,
    "similarity_score": 0.95,
    "answer_options": [
      {
        "code": "YES",
        "content": "Yes",
        "frequency": 1234,
        "percentage": 15.2
      }
    ]
  },
  "top_variables": [],
  "final_answer": "Based on the data, approximately 15.2% of adults reported having depression...",
  "reasoning": "Variable selection reasoning",
  "polars_query": "Generated Polars code",
  "error": null
}
```

**Example:**

```bash
curl -X POST "http://52.170.155.134:8001/analyze" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "How many people use marijuana in the last month?",
    "include_synthesis": true,
    "top_k": 5
  }'
```

### 5. `/knowledge` - Knowledge Search

**Purpose:** Search for knowledge using semantic search over text collection.

**Method:** `POST`

**Request Format:**

```json
{
  "question": "string (required)",
  "top_k": "integer (optional, default: 5, max: 20)",
  "use_synthesis": "boolean (optional, default: true)"
}
```

**Response Format:**

```json
{
  "query": "Original question",
  "query_type": "knowledge_query",
  "results": [
    {
      "id": "text_123",
      "title": "Understanding Depression",
      "content": "Depression is a common mental health condition...",
      "year": 2022,
      "dataset": 1,
      "score": 0.95,
      "source_table": "text_nsduh",
      "title_id": "depression_guide",
      "page_number": 1,
      "source_info": {
        "source_id": "text_123",
        "title_id": "depression_guide",
        "type": "definition",
        "page_number": 1
      }
    }
  ],
  "selected_result": {
    /* Same structure as results[0] */
  },
  "synthesized_answer": "Depression is a common mental health condition characterized by...",
  "total_found": 1,
  "processing_time_ms": 1250.5,
  "success": true,
  "error": null
}
```

**Example:**

```bash
curl -X POST "http://52.170.155.134:8001/knowledge" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "What are the symptoms of anxiety disorders?",
    "top_k": 5,
    "use_synthesis": true
  }'
```

### 6. `/search` - Service Search

**Purpose:** Search for mental health treatment services using AI-powered ServiceHelper.

**Method:** `POST`

**Request Format:**

```json
{
  "question": "string (required)",
  "county": "string (optional)",
  "state": "string (optional)"
}
```

**Response Format:**

```json
{
  "success": true,
  "answer": "I found several mental health services in your area...",
  "provider_ids": ["provider_123", "provider_456"],
  "service_ids": ["service_789", "service_012"],
  "treatment_provider_flag": true,
  "trace": [
    {
      "stage": "search",
      "message": "Searching for mental health services",
      "data": {}
    }
  ],
  "processing_time_ms": 1234.56,
  "error": null
}
```

**Example:**

```bash
curl -X POST "http://52.170.155.134:8001/search" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "Find depression treatment in Los Angeles",
    "county": "Los Angeles County",
    "state": "California"
  }'
```

---

## 📡 Streaming Endpoint

### 7. `/classify/stream` - Real-time Processing

**Purpose:** Streaming version with real-time logs for long-running queries.

**Method:** `GET`

**Parameters:**

- `message`: string (required) - The user's question
- `top_k`: integer (optional, default: 10)
- `use_synthesis`: boolean (optional, default: true)

**Response:** Server-Sent Events (SSE) stream

**Example:**

```bash
curl -N "http://52.170.155.134:8001/classify/stream?message=Find%20mental%20health%20services&top_k=5&use_synthesis=true"
```

---

## 🏥 Additional API Endpoints

### 8. `/api/providers/{tp_id}` - Get Provider Details

**Purpose:** Get treatment provider information by ID.

**Method:** `GET`

**Parameters:**

- `tp_id`: integer (required) - Provider ID

**Response Format:**

```json
{
  "tp_id": 123,
  "tp_name": "Provider Name",
  "tp_name_sub": "Sub Name",
  "address_line_1": "123 Main St",
  "address_line_2": "Suite 100",
  "state_code": "CA",
  "zip_code": "90210",
  "phone": "(555) 123-4567",
  "intake1": "Intake info",
  "intake2": "Additional intake",
  "intake1a": "Intake 1a",
  "intake2a": "Intake 2a",
  "latitude": 34.0522,
  "longitude": -118.2437
}
```

**Example:**

```bash
curl "http://52.170.155.134:8001/api/providers/123"
```

### 9. `/api/places/details` - Google Places Integration

**Purpose:** Get place details from Google Places API.

**Method:** `GET`

**Parameters:**

- `name`: string (required) - Place name
- `lat`: float (required) - Latitude
- `lng`: float (required) - Longitude

**Response Format:**

```json
{
  "found": true,
  "name": "Place Name",
  "address": "123 Main St, City, State",
  "rating": 4.5,
  "weekday_text": ["Monday: 9:00 AM – 5:00 PM", "Tuesday: 9:00 AM – 5:00 PM"]
}
```

**Example:**

```bash
curl "http://52.170.155.134:8001/api/places/details?name=Mental%20Health%20Center&lat=34.0522&lng=-118.2437"
```

### 10. `/api/kg/*` - Knowledge Graph Endpoints

**Purpose:** Access knowledge graph data.

**Available Endpoints:**

- `GET /api/kg/all` - Get full knowledge graph
- `GET /api/kg/relations/{node_id}` - Get node relations

**Example:**

```bash
curl "http://52.170.155.134:8001/api/kg/all"
```

---

## 🏥 Health and Status Endpoints

### 11. `/health` - Health Check

**Purpose:** Check service status and dependencies.

**Method:** `GET`

**Response Format:**

```json
{
  "postgres": "ok",
  "milvus": "ok",
  "openai": "disabled"
}
```

### 12. `/security/status` - Security Status

**Purpose:** Security monitoring information.

**Method:** `GET`

**Response Format:**

```json
{
  "status": "active",
  "rate_limiting": "enabled",
  "security_headers": "enabled",
  "suspicious_patterns": "blocked"
}
```

### 13. `/` - Root Endpoint

**Purpose:** Service information and available endpoints.

**Method:** `GET`

**Response Format:**

```json
{
  "service": "ChatNIO Backend",
  "status": "running",
  "endpoints": {
    "health": "/health",
    "query": "/query (POST - clean, organized API for team consumption)",
    "preprocess": "/preprocess",
    "classify": "/classify (smart router)",
    "classify_stream": "/classify/stream (GET - streaming version)",
    "search": "/search (AI-powered service lookup)",
    "analyze": "/analyze (data query analysis)",
    "knowledge": "/knowledge (semantic text search)",
    "api_providers": "/api/providers/{tp_id}",
    "api_places": "/api/places/details",
    "api_knowledge": "/api/kg",
    "docs": "/docs"
  }
}
```

---

## 📋 Postman Collection Examples

### Collection Setup

1. **Base URL:** `http://52.170.155.134:8001`
2. **Headers:** `Content-Type: application/json`

### Request Examples

#### 1. Main Query Endpoint

```json
POST /query
{
  "question": "What percentage of adults have depression?",
  "top_k": 10,
  "use_synthesis": true
}
```

#### 2. Data Analysis

```json
POST /analyze
{
  "question": "How many people use marijuana in the last month?",
  "include_synthesis": true,
  "top_k": 5
}
```

#### 3. Knowledge Search

```json
POST /knowledge
{
  "question": "What are the symptoms of anxiety disorders?",
  "top_k": 5,
  "use_synthesis": true
}
```

#### 4. Service Search

```json
POST /search
{
  "question": "Find depression treatment in Los Angeles",
  "county": "Los Angeles County",
  "state": "California"
}
```

#### 5. Query Preprocessing

```json
POST /preprocess
{
  "question": "What is the depression rate in California?"
}
```

#### 6. Health Check

```json
GET /health
```

#### 7. Get Provider Details

```json
GET /api/providers/123
```

#### 8. Get Place Details

```json
GET /api/places/details?name=Mental%20Health%20Center&lat=34.0522&lng=-118.2437
```

---

## 🎯 Real-World Use Cases for OKN Justice Teams

### Use Case 1: Data Analysis for Policy Research

**Scenario:** Justice team needs to analyze mental health data for policy recommendations.

**API Call:**

```bash
curl -X POST "http://52.170.155.134:8001/query" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "What is the prevalence of substance use disorders among adults in 2022?",
    "top_k": 10,
    "use_synthesis": true
  }'
```

**Expected Response:** Detailed statistical data with percentages, frequencies, and synthesized analysis.

### Use Case 2: Knowledge Base Queries

**Scenario:** Team needs definitions and explanations of mental health concepts.

**API Call:**

```bash
curl -X POST "http://52.170.155.134:8001/query" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "What are the diagnostic criteria for PTSD?",
    "use_synthesis": true
  }'
```

**Expected Response:** Comprehensive knowledge base results with definitions and explanations.

### Use Case 3: Service Locator for Justice-Involved Individuals

**Scenario:** Finding mental health services for justice-involved individuals.

**API Call:**

```bash
curl -X POST "http://52.170.155.134:8001/query" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "Find mental health services for justice-involved individuals in Los Angeles County",
    "top_k": 5
  }'
```

**Expected Response:** List of relevant service providers with contact information and details.

### Use Case 4: Research Data Extraction

**Scenario:** Extracting specific data points for research reports.

**API Call:**

```bash
curl -X POST "http://52.170.155.134:8001/analyze" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "What is the rate of co-occurring mental health and substance use disorders?",
    "include_synthesis": true,
    "top_k": 5
  }'
```

**Expected Response:** Specific variable data with answer options, frequencies, and percentages.

### Use Case 5: Provider Information Lookup

**Scenario:** Getting detailed information about specific treatment providers.

**API Call:**

```bash
curl "http://52.170.155.134:8001/api/providers/123"
```

**Expected Response:** Complete provider details including contact information, address, and intake procedures.

---

## 🔧 Error Handling

### Common Error Responses

#### 1. Service Unavailable

```json
{
  "success": false,
  "error": "Database connection failed"
}
```

#### 2. Invalid Request

```json
{
  "success": false,
  "error": "Question parameter is required"
}
```

#### 3. Processing Timeout

```json
{
  "success": false,
  "error": "Request processing timeout"
}
```

#### 4. Provider Not Found

```json
{
  "detail": "Provider not found"
}
```

#### 5. API Key Missing

```json
{
  "detail": "Google Places API key not configured"
}
```

---

## 📊 Rate Limits

- **Requests per minute:** 60
- **Requests per hour:** 1000

Rate limit headers are included in responses:

- `X-RateLimit-Limit`: Maximum requests allowed
- `X-RateLimit-Remaining`: Remaining requests in current window
- `X-RateLimit-Reset`: Time when the rate limit resets

---

## 🚀 Quick Start Guide

### 1. Test Service Health

```bash
curl "http://52.170.155.134:8001/health"
```

### 2. Try Main Query Endpoint

```bash
curl -X POST "http://52.170.155.134:8001/query" \
  -H "Content-Type: application/json" \
  -d '{"question": "What is depression?"}'
```

### 3. Explore Available Endpoints

```bash
curl "http://52.170.155.134:8001/"
```

### 4. View Interactive Documentation

Open: `http://52.170.155.134:8001/docs`

---

## 📞 Support

For technical support or questions about the API:

- **Service Status:** Check `/health` endpoint
- **Documentation:** Available at `/docs`
- **Root Information:** Available at `/`

---

## 🔄 Changelog

### Version 1.0.0

- Initial release of comprehensive API
- Support for data, knowledge, and service queries
- Automatic query classification and routing
- Rich preprocessing metadata
- Comprehensive error handling
- Multiple specialized endpoints
- Google Places integration
- Knowledge graph access
- Real-time streaming support
