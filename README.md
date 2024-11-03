# Privacy Data Vault API

This repository contains an implementation of a **Privacy Data Vault API** designed to securely tokenize, store, and detokenize sensitive data with authenticated access. The API is built in Go and provides robust data protection, ensuring that sensitive information is securely stored and only accessible by authorized services.

## Features

- **Tokenization and Detokenization**: Converts sensitive data into secure tokens for storage and retrieval, safeguarding data while in transit and at rest.
- **Authentication and Authorization**: Implements service-to-service authentication using API keys and client secrets, enabling fine-grained access control to ensure only authorized clients can tokenize or detokenize data.
- **Data Encryption**: Protects sensitive data using AES-GCM encryption, ensuring that only authorized services can decrypt and access the original data.
- **Secure Storage with Redis**: Utilizes Redis as a high-performance data store for managing tokenized data, enhancing both security and speed.
- **Granular Access Control**: Adds permissions to differentiate between writer (tokenization) and reader (detokenization) roles, ensuring that services only perform authorized actions.
- **Production-ready Security Practices**: Designed for deployment with HTTPS, following best practices for data security and compliance.

## Getting Started

### Prerequisites

- **Go**: The code is written in Go and requires Go 1.16+.
- **Redis**: Redis is used to store tokenized data.

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/yourusername/privacy-data-vault.git
   cd privacy-data-vault
   ```

2. Install dependencies:

   ```bash
   go mod tidy
   ```

3. Configure environment variables for your API key and client secret management (example configuration provided in `.env.example`).

### Usage

- **Tokenize Data**: Endpoint to generate secure tokens for sensitive data fields.
- **Detokenize Data**: Endpoint to retrieve original data from tokens if the client has appropriate permissions.
- **API Authentication**: Secures each request using API keys and client secrets, validated through middleware.

Great job on the updates! Here’s how you can modify your README file to include usage instructions for the new API key issuance and verification functionality, as well as establishing a Redis server for the application.

### README.md

# Vault API

## Overview

Vault API is a service that allows you to tokenize and detokenize sensitive data securely. It uses API keys for authentication and Redis for data storage.

## Prerequisites

- Go 1.17+
- Redis Server

## Setting Up Redis

Before running the application, you need to have a Redis server running. You can install Redis locally or use or via docker.

## Running the Application

Make sure you have the necessary environment variables set up for your application, including the secret key used for encryption.

### Start the application

```bash
go run main.go
```

## API Endpoints

### 1. Issue API Key

**POST** `/api/keys`

#### Request Body

```json
{
  "organization": "your_organization",
  "permissions": ["tokenize", "detokenize"]
}
```

#### Response

```json
{
  "apiKey": "generated_api_key",
  "clientSecret": "generated_client_secret",
  "organization": "your_organization",
  "permissions": ["tokenize", "detokenize"]
}
```

### 2. Tokenize Data

**POST** `/api/tokenize`

#### Headers

- `X-API-KEY`: Your API key
- `X-SECRET-KEY`: Your client secret

#### Request Body

```json
{
  "id": "unique_identifier",
  "data": {
    "field1": "sensitive_data1",
    "field2": "sensitive_data2"
  }
}
```

#### Response

```json
{
  "id": "unique_identifier",
  "data": {
    "field1": "generated_token1",
    "field2": "generated_token2"
  }
}
```

### 3. Detokenize Data

**POST** `/api/detokenize`

#### Headers

- `X-API-KEY`: Your API key
- `X-SECRET-KEY`: Your client secret

#### Request Body

```json
{
  "id": "unique_identifier",
  "data": {
    "field1": "generated_token1",
    "field2": "generated_token2"
  }
}
```

#### Response

```json
{
  "id": "unique_identifier",
  "data": {
    "field1": "sensitive_data1",
    "field2": "sensitive_data2"
  }
}
```
