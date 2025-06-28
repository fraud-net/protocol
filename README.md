# Fraud-Net Protocol

## Overview

The Fraud-Net Protocol enables organizations to collaborate in fraud detection and prevention through standardized threat intelligence sharing and coordinated defense mechanisms.

## Protocol Requirements

### Participant Implementation

All participating organizations must implement:

1. **Discovery File**: `.well-known/anti-fraud.txt`
   - Contains URL to the organization's fraud intelligence endpoint
   - Must be publicly accessible via HTTPS

2. **Fraud Intelligence Endpoint**
   - Returns email hashes of known fraudulent accounts
   - Provides contact email for API key requests
   - Implements secure authentication for data access

### Example `.well-known/anti-fraud.txt`

```
endpoint=https://api.example.com/fraud-intelligence
contact=security@example.com
```

### Endpoint Response Format

```json
{
  "email_hashes": [
    "5d41402abc4b2a76b9719d911017c592",
    "7d865e959b2466918c9863afca942d0f"
  ],
  "contact_email": "security@example.com",
  "api_key_request": "security@example.com",
  "hash_count": 1,
  "hash_algorithm": "SHA-512"
}
```

## Hash Calculation Algorithm

### Normalization Phase
1. Convert email to lowercase
2. Remove leading/trailing whitespace
3. Apply Unicode normalization (NFC)
4. Remove dots from Gmail addresses (before @)
5. Extract base address (ignore +tags)

### Hashing Phase
1. Apply consecutive hashing using specified algorithm (default: SHA-512)
2. Number of hash iterations specified by `hash_count` field
3. Each iteration hashes the result of the previous iteration
4. Final hash is hex-encoded

### Example
```
Email: John.Doe+test@gmail.com
Normalized: johndoe@gmail.com
Hash 1: SHA-512(johndoe@gmail.com)
Hash 2: SHA-512(Hash 1) [if hash_count = 2]
```

## Implementation Steps

1. Deploy `.well-known/anti-fraud.txt` file on your domain
2. Implement fraud intelligence endpoint
3. Configure email hash generation with normalization
4. Set up API key management system
5. Begin sharing fraud intelligence data

## Security Standards

- HTTPS required for all communications
- API key authentication for endpoint access
- Configurable hash iterations for security
- Rate limiting on API endpoints

## Network Discovery

Organizations can discover other participants by:
1. Checking `.well-known/anti-fraud.txt` files
2. Requesting API keys via contact emails
3. Accessing fraud intelligence endpoints

---

*Unifying forces in fighting fraud through standardized intelligence sharing*