
# Enterprise Information Extraction System (Agentic AI)

This module documents the design of an **agentic, multi-agent information extraction system** built to identify, validate, and structure enterprise-level company intelligence at scale.

The system is designed to autonomously extract:
- Global ultimate parent relationships
- Parent–subsidiary hierarchies
- Company attributes (headquarters, legal entities, locations)

All logic is presented at an **architecture and reasoning level**, without exposing any client-specific data or proprietary code.

---

## Problem Statement

Enterprise organizations often need accurate company hierarchy and ownership data for:
- Vendor risk assessment
- Compliance and due diligence
- Procurement and financial analysis

However, this information is:
- Scattered across multiple public sources
- Inconsistent in format and reliability
- Frequently outdated or contradictory
- Difficult to validate at scale

Manual research does not scale beyond a few hundred companies and introduces human error.

The challenge was to design a system that could:
- Discover company information autonomously
- Reason over unstructured web content
- Validate findings using multiple signals
- Assign confidence scores to extracted data
- Operate reliably across thousands of organizations

---

## High-Level Solution Overview

The solution is a **multi-agent orchestration pipeline** where each agent performs a clearly scoped responsibility.

Rather than relying on a single LLM pass, the system:
- Separates discovery, extraction, validation, and scoring
- Uses reasoning-driven agents instead of regex or rules
- Applies confidence scoring before accepting results
- Continuously improves data quality through filtering logic

---

## Agentic Architecture

The pipeline is composed of multiple agents operating sequentially with shared state.

---

### 1. Ingestion & Normalization Agent

**Purpose:**  
Prepare raw company inputs for downstream processing.

**Responsibilities:**
- Ingest structured inputs (company name, country, identifiers)
- Normalize company names (remove noise, symbols, placeholders)
- Detect invalid or incomplete records early
- Flag problematic entries for special handling

This agent ensures downstream agents operate on clean, standardized inputs.

---

### 2. Discovery & Search Agent

**Purpose:**  
Identify authoritative online sources for each company.

**Responsibilities:**
- Query search engines using normalized company names
- Identify official websites, registries, and filings
- Filter out low-quality sources (social media, forums, unreliable aggregators)
- Prioritize trusted domains

This agent controls **source quality**, which directly impacts extraction accuracy.

---

### 3. Web Crawling & Content Extraction Agent

**Purpose:**  
Extract relevant textual content from discovered sources.

**Responsibilities:**
- Crawl selected URLs
- Extract readable content from web pages
- Remove boilerplate, navigation, and noise
- Pass clean text to reasoning agents

This agent bridges raw web data and semantic reasoning.

---

### 4. Entity Reasoning Agent (Global Parent Finder)

**Purpose:**  
Determine ownership and hierarchy relationships.

**Responsibilities:**
- Analyze extracted content using LLM reasoning
- Identify global ultimate parent entities
- Detect subsidiaries and intermediate parents
- Differentiate legal ownership from branding or partnerships

This agent performs the core semantic reasoning task.

---

### 5. Attribute Validation Agent

**Purpose:**  
Validate supporting company attributes.

**Responsibilities:**
- Verify headquarters location
- Confirm legal entity status
- Cross-check addresses and identifiers
- Validate findings
