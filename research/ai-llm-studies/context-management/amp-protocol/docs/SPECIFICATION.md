---
title: "AMP: Artifact Migration Protocol"
description: "A standard protocol for managing large artifacts and contexts in sessions with LLMs (Large Language Models)."
date: "2025-03-22"
author: "TQY KOBAYASHI"
category: "protocol-specification"
tags: ["amp", "llm", "protocol", "context-management"]
license: "MIT"
---
# AMP: Artifact Migration Protocol

**Artifact Migration Protocol (AMP)** is a standard protocol for managing large artifacts and contexts in sessions with LLMs (Large Language Models). This protocol is designed to maintain complex conversations over extended periods despite token limitations, and to efficiently store and reuse generated outputs.

**Last Updated:** March 22, 2025  
**Author:** TQY KOBAYASHI (CXO of Takenodai Community Committee & Full-time Faculty at HAL Osaka)  
**Contributors:** Claude, Gemini, Deepseek, ChatGPT

## 1. Purpose and Background

The Artifact Migration Protocol (AMP) was developed to solve the following challenges:

- **Overcoming Context Limitations**: Managing token limits in long sessions
- **Artifact Preservation**: Maintaining consistency of generated outputs (code, text, etc.)
- **Session Continuity**: Minimizing interruptions and enabling seamless dialogue
- **Cross-Model Compatibility**: Enabling consistent data maintenance across various LLMs

## 2. Specification

### 2.1 Basic Structure

AMP is described in JSON format and consists of the following main components:

```json
{
  "command": "migrate",
  "version": "1.0",
  "settings": {
    "splitMode": "auto",
    "safetyMargin": 0.8,
    "thresholds": {
      "text": 2000,
      "numbers": 1500,
      "code": 1000,
      "mixed": 1800
    },
    "markers": {
      "prefix": "AMP-Session",
      "continuation": "AMP-Continue",
      "complete": "AMP-Complete"
    }
  },
  "artifacts": [
    {
      "id": "conversation-1",
      "type": "text/markdown",
      "title": "First Conversation",
      "content": "This is where the first conversation content is described.",
      "checksum": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855"
    },
    {
      "id": "code-snippet-1",
      "type": "text/code",
      "title": "Sample Code",
      "content": "console.log('Hello, World!');",
      "checksum": "7f83b1657ff1fc53b92dc18148a1d65dfc2d4b1fa3d677284addd200126d9069"
    }
  ],
  "context": {
    "sessionId": "amp-20250322-12345",
    "timestamp": "2025-03-22T17:55:42Z",
    "segmentCount": 3,
    "currentSegment": 1
  }
}
```

### 2.2 Parameter Descriptions

#### command
- `migrate`: Start or continue a new session
- `continue`: Continue an existing session
- `complete`: Complete a session

#### settings
- `splitMode`: Method of division
  - `auto`: System automatically determines optimal division
  - `fixed`: Fixed-size division
  - `adaptive`: Dynamic division based on data complexity
  - `manual`: Manual control of division

- `safetyMargin`: Safety margin for token limits (0.0 to 1.0)

- `thresholds`: Division thresholds by data type
  - `text`: Regular text (word count)
  - `numbers`: Numerical data (item count)
  - `code`: Program code (line count)
  - `mixed`: Mixed content (estimated token count)

- `markers`: Segment identification markers
  - `prefix`: Session start marker
  - `continuation`: Continuation segment marker
  - `complete`: Completion marker

#### artifacts
An array of artifacts. Each artifact includes:
- `id`: Unique identifier
- `type`: MIME type (e.g., text/markdown, application/json)
- `title`: Human-readable title
- `content`: Actual content of the artifact
- `checksum`: Hash value for integrity verification

#### context
- `sessionId`: Unique identifier for the session
- `timestamp`: ISO format date and time
- `segmentCount`: Total number of segments
- `currentSegment`: Current segment number

## 3. Usage

### 3.1 Starting a Session

When starting a new session:

```json
{
  "command": "migrate",
  "version": "1.0",
  "settings": {
    /* Configuration information */
  },
  "artifacts": [
    /* Initial artifacts */
  ],
  "context": {
    "sessionId": "new-session-ID",
    "timestamp": "start-time",
    "segmentCount": 1,
    "currentSegment": 1
  }
}
```

### 3.2 Continuing a Session

When continuing a divided session:

```json
{
  "command": "continue",
  "version": "1.0",
  "context": {
    "sessionId": "amp-20250322-12345",
    "timestamp": "2025-03-22T18:02:15Z",
    "segmentCount": 3,
    "currentSegment": 2
  },
  "previousSegment": {
    "checksum": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855"
  },
  "recoveryStrategy": {
    "onConnectionError": "retry",
    "maxRetries": 3,
    "retryIntervalMs": 1000,
    "fallbackMode": "localCache"
  }
}
```

### 3.3 Completing a Session

When completing a session:

```json
{
  "command": "complete",
  "version": "1.0",
  "context": {
    "sessionId": "amp-20250322-12345",
    "timestamp": "2025-03-22T18:15:37Z",
    "segmentCount": 3,
    "currentSegment": 3
  },
  "summary": {
    "artifactsCount": 5,
    "totalSize": 28450,
    "totalTokens": 8129,
    "processingTimeMs": 12583
  }
}
```

## 4. Implementation Guidelines

### 4.1 Division Strategy

Experiments with multiple major LLMs have shown that the optimal division strategy varies by data type and model. For detailed experimental results, refer to the appendix "AMP Token Optimization Guide."

Recommended division values for major models:

| Model | Text | Numerical Data | Code | Mixed |
|--------|---------|-----------|-------|------|
| Claude | 200 words | 225 items | 600 lines | ~600 tokens |
| Deepseek | 250 words | 300 items | 850 lines | ~900 tokens |

### 4.2 Token Estimation Considerations

- Character-to-token ratio may vary by model
- Numerical values and special characters may consume different numbers of tokens
- Patterned content may consume more tokens than expected

### 4.3 Session Management Best Practices

- Clearly mark the beginning and end of each segment
- Use checksums to verify integrity
- Repeat important context information within segments
- Provide a complete artifact list in the final segment

### 4.4 Checksum Generation and Verification

Checksums are important for ensuring artifact integrity. The recommended checksum generation method is SHA-256 hash.

```javascript
// JavaScript example of checksum generation
async function generateChecksum(content) {
  const encoder = new TextEncoder();
  const data = encoder.encode(content);
  const hashBuffer = await crypto.subtle.digest('SHA-256', data);
  const hashArray = Array.from(new Uint8Array(hashBuffer));
  const hashHex = hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
  return hashHex;
}

// Checksum verification example
async function verifyChecksum(content, expectedChecksum) {
  const actualChecksum = await generateChecksum(content);
  return actualChecksum === expectedChecksum;
}
```

#### Python Implementation Example

```python
import hashlib

def generate_checksum(content):
    """Generate SHA-256 checksum from string content"""
    if isinstance(content, str):
        content = content.encode('utf-8')
    return hashlib.sha256(content).hexdigest()

def verify_checksum(content, expected_checksum):
    """Verify if checksum matches"""
    actual_checksum = generate_checksum(content)
    return actual_checksum == expected_checksum
```

### 4.5 Error Recovery Strategy

In the AMP protocol, recovery strategies for communication errors and segment loss are important. The following are recommended recovery strategies:

#### 4.5.1 Response to Communication Errors

1. **Retry Strategy**:
   - Exponential backoff retry (first 1 second, second 2 seconds, third 4 seconds...)
   - Set maximum number of retries (default: 3 times)

2. **Using Local Cache**:
   - Save the last successful response locally
   - Restore context from cache during communication failure

3. **Partial Session Restart**:
   - Restart the session from the last verified checksum point
   - Estimate and reconstruct lost data

#### 4.5.2 Segment Loss Detection and Response

1. **Loss Detection Method**:
   - Check for segment number continuity
   - Verify data integrity through checksum verification

2. **Response Flow for Loss**:
```
Loss Detection → Attempt Recovery from Previous Segment → 
If Partially Restored: Request Differential Update → 
If Cannot Restore: Request Missing Segment → 
If Still Failed: Restart from Last Known State
```

3. **Utilizing Duplicate Information Between Segments**:
   - Use duplicate information at the beginning/end to estimate boundaries
   - Store important context information redundantly across multiple segments

## 5. Cross-Model Compatibility

AMP is usable on major LLMs, but optimal parameter adjustment may be necessary for each model. Currently tested models:

- Claude (Anthropic)
- Gemini (Google)
- Deepseek
- ChatGPT (OpenAI)

### 5.1 Cross-Model Implementation Examples

It has been confirmed that the AMP protocol can be implemented with similar structures across multiple AI models. The implementation methods and characteristics for each model are as follows:

#### Implementation Differences by Model

| Model | Implementation Method | Session Retention | Notes |
|--------|---------|--------------|---------|
| **Claude** | AMP JSON can be manually included in user profile | Persistent across sessions via profile | Can maintain state between sessions |
| **ChatGPT** | Initial explicit AMP JSON is automatically stored in user profile | Automatically saved | Can be referenced and updated after initial presentation |
| **Gemini** | AMP JSON initialization required for each new session | Session-limited | Requires initialization command each time |
| **Deepseek** | AMP JSON initialization required for each new session | Session-limited | Requires initialization command each time |
| **Copilot** | AMP JSON initialization required for each new session | Session-limited | Requires initialization command each time |

#### Claude Implementation Example

```html
<userPreferences>
{
  "command": "migrate",
  "version": "1.0",
  "settings": {
    "splitMode": "auto",
    "safetyMargin": 0.8,
    "thresholds": {
      "text": 2000,
      "numbers": 1500,
      "code": 1000,
      "mixed": 1800
    },
    "markers": {
      "prefix": "AMP-Session",
      "continuation": "AMP-Continue",
      "complete": "AMP-Complete"
    }
  },
  "artifacts": [],
  "context": {
    "sessionId": "uniqueSessionID",
    "timestamp": "ISODateTimeFormat",
    "segmentCount": 0,
    "currentSegment": 0
  }
}
</userPreferences>
```

#### ChatGPT Implementation Example

When specified in the initial prompt as follows, it will be retained between sessions:

```
Please save the following JSON format as a user profile and always refer to it:

{
  "command": "migrate",
  "version": "1.0",
  "settings": {
    "splitMode": "auto",
    "safetyMargin": 0.8,
    "thresholds": {
      "text": 2000,
      "numbers": 1500,
      "code": 1000,
      "mixed": 1800
    },
    "markers": {
      "prefix": "AMP-Session",
      "continuation": "AMP-Continue",
      "complete": "AMP-Complete"
    }
  },
  "artifacts": [],
  "context": {
    "sessionId": "uniqueSessionID",
    "timestamp": "ISODateTimeFormat",
    "segmentCount": 0,
    "currentSegment": 0
  }
}
```

#### Deepseek/Gemini Implementation Example

For these models, JSON like the following needs to be presented for each new session:

```json
{
  "command": "migrate",
  "version": "1.0",
  "settings": {
    "splitMode": "auto",
    "safetyMargin": 0.8,
    "thresholds": {
      "text": 2000,
      "numbers": 1500,
      "code": 1000,
      "mixed": 1800
    },
    "markers": {
      "prefix": "AMP-Session",
      "continuation": "AMP-Continue",
      "complete": "AMP-Complete"
    }
  },
  "artifacts": [
    {
      "id": "conversation-1",
      "type": "text/markdown",
      "title": "First Conversation",
      "content": "This is where the first conversation content is described.",
      "checksum": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855"
    },
    {
      "id": "code-snippet-1",
      "type": "text/code",
      "title": "Sample Code",
      "content": "console.log('Hello, World!');",
      "checksum": "7f83b1657ff1fc53b92dc18148a1d65dfc2d4b1fa3d677284addd200126d9069"
    }
  ],
  "context": {
    "sessionId": "uniqueSessionID",
    "timestamp": "ISODateTimeFormat",
    "segmentCount": 3,
    "currentSegment": 1
  }
}
```

#### Copilot Implementation Example

Copilot can also be implemented with customized parameters:

```json
{
  "command": "migrate",
  "version": "1.0",
  "settings": {
    "splitMode": "manual",  // Set the data division method to manual
    "safetyMargin": 0.9,    // Set the safety margin to 90%
    "thresholds": {
      "text": 2500,         // Set text token limit to 2500
      "numbers": 2000,      // Set numerical token limit to 2000
      "code": 1500,         // Set code token limit to 1500
      "mixed": 2200         // Set mixed data token limit to 2200
    },
    "markers": {
      "prefix": "Custom-Session",      // Customize session start marker
      "continuation": "Custom-Continue", // Customize session continuation marker
      "complete": "Custom-Complete"    // Customize session completion marker
    },
    "recoveryOptions": {               // Add error recovery options
      "retryCount": 3,                 // Number of retries
      "retryInterval": 2000,           // Retry interval (milliseconds)
      "cacheStrategy": "sessionStorage" // Cache strategy
    }
  },
  "artifacts": [
    {
      "id": "conversation-1",
      "type": "text/markdown",
      "title": "First Conversation",
      "content": "This is where the first conversation content is described.",
      "checksum": "7d9394b780182a038387dee71b31b19d4131fab8940c5a1a813f0d24d566e4be"
    }
  ],
  "context": {
    "sessionId": "copilot-20250322-67890",
    "timestamp": "2025-03-22T18:10:15Z",
    "segmentCount": 1,
    "currentSegment": 1
  }
}
```

The consistent structure across models demonstrates that AMP has model-independent versatility. Despite differences in implementation methods, the basic protocol structure is commonly usable.

## 6. Future Extensions

- **Differential Updates**: Efficient update method that only sends changed parts
- **Compression Methods**: Efficient encoding for large artifacts
- **Encryption Support**: Secure transfer of confidential data
- **Distributed Sessions**: Context sharing across multiple models

## 7. Contributions and Feedback

AMP is a continuously evolving open protocol. We welcome feedback such as implementation examples, performance measurements, and suggestions for improvement.

## Appendix

For detailed experimental results, optimization guides by data type, and parameter settings by model, please refer to the separate "AMP Token Optimization Guide". This technical document provides detailed explanations of token depletion patterns and optimal division strategies for each model.

---

*This specification defines the initial version of AMP and may be updated based on community feedback.*