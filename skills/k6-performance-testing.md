# K6 Performance Testing Skill

**PURPOSE:** Guide the creation of k6 load performance test files for both Go backend services and Micro-Frontend (MFE) applications.

---

## Overview

k6 is a modern load testing tool for testing the performance and reliability of APIs, microservices, and web applications. This skill provides patterns and best practices for creating k6 test scripts.

## Test Types

### 1. Smoke Test
- **Purpose:** Verify basic functionality with minimal load
- **Load:** 1-5 VUs for 1-2 minutes
- **Use When:** Initial testing, CI/CD validation, sanity checks

### 2. Load Test
- **Purpose:** Test system under expected/realistic load
- **Load:** Normal production traffic patterns
- **Use When:** Regular performance validation, capacity planning

### 3. Stress Test
- **Purpose:** Push system beyond normal capacity to find breaking points
- **Load:** Gradually increasing (e.g., 100 → 200 → 300 VUs)
- **Use When:** Finding system limits, capacity planning

### 4. Spike Test
- **Purpose:** Test system behavior under sudden traffic bursts
- **Load:** Rapid increase (e.g., 10 → 500 VUs in seconds)
- **Use When:** Testing auto-scaling, resilience, cache behavior

---

## Folder Structure

Create the following structure in the repository root:

```
perf-test/
├── README.md              # Overview, installation, usage
├── .gitignore             # Ignore test results and outputs
├── script.js              # Main comprehensive load test (recommended starting point)
├── config/
│   └── environments.js    # Environment configurations (local, dev, qa, staging)
├── data/
│   └── event-ids.json     # Test data (event IDs, etc.) - easily updateable
├── scenarios/
│   ├── smoke.js           # Basic functionality test (1-2 VUs, 1 min)
│   ├── load.js            # Realistic traffic simulation (10-50 VUs, 10 min)
│   ├── stress.js          # Gradually increasing load (up to 150 VUs, 20 min)
│   └── spike.js           # Sudden traffic burst (spikes to 150 VUs, 10 min)
├── tests/
│   └── [app-name].test.js # Test implementation with reusable test functions
└── utils/
    └── helpers.js         # Helper functions and shared utilities
```

### Key Files:

1. **script.js** - Main entry point for basic load testing. Use this for quick, comprehensive tests.
2. **scenarios/*.js** - Specialized test scenarios for specific testing needs (smoke, load, stress, spike).
3. **config/environments.js** - Centralized environment configuration. **Important: Do not include production URLs** to prevent accidental load testing of production.
4. **data/*.json** - Test data files (event IDs, user accounts, etc.) in JSON format for easy updates.
5. **tests/[app-name].test.js** - Core test logic with reusable functions that scenarios import.
6. **utils/helpers.js** - Shared helper functions, test data selectors, and utilities.

### Setup Steps:

1. Create the folder structure above
2. Implement `script.js` as your main load test
3. Create specialized scenario files in `scenarios/`
4. Extract test data to `data/` directory for maintainability
5. Place reusable test functions in `tests/`
6. Add helper utilities to `utils/`

---

## Implementation Workflow

Follow these steps to create a complete k6 test suite:

### Step 1: Create Directory Structure

```bash
mkdir -p perf-test/{config,data,scenarios,tests,utils}
touch perf-test/{README.md,.gitignore,script.js}
touch perf-test/config/environments.js
touch perf-test/scenarios/{smoke,load,stress,spike}.js
```

### Step 2: Configure Environments (`config/environments.js`)

Create environment configuration **without production**:

```javascript
export const environments = {
  local: { baseUrl: 'http://localhost:3000', protocol: 'http' },
  qa: { baseUrl: 'https://qa.example.com', protocol: 'https' },
};

export function getBaseUrl() {
  const env = __ENV.ENV || 'local';
  return environments[env]?.baseUrl || environments.local.baseUrl;
}
```

### Step 3: Create Test Data (`data/*.json`)

Extract test data to JSON files:

```json
{
  "eventIds": ["id1", "id2", "id3"],
  "metadata": {
    "lastUpdated": "2026-03-12",
    "description": "Test event IDs from QA environment"
  }
}
```

### Step 4: Create Helper Utilities (`utils/helpers.js`)

Add reusable functions:

```javascript
import { sleep } from 'k6';
import testData from '../data/event-ids.json';

export function randomItem(array) {
  return array[Math.floor(Math.random() * array.length)];
}

export function getRandomEventId() {
  return randomItem(testData.eventIds);
}

export function thinkTime(min = 1, max = 3) {
  const duration = min + Math.random() * (max - min);
  sleep(duration);
}
```

### Step 5: Create Test Functions (`tests/[app-name].test.js`)

Implement reusable test logic:

```javascript
import http from 'k6/http';
import { check } from 'k6';
import { getBaseUrl } from '../config/environments.js';
import { getRandomEventId } from '../utils/helpers.js';

export function testEventPage(baseUrl) {
  const eventId = getRandomEventId();
  const response = http.get(`${baseUrl}/events/${eventId}`);
  
  check(response, {
    'status is 200': (r) => r.status === 200,
    'response has content': (r) => r.body.length > 0,
  });
  
  return response;
}
```

### Step 6: Create Main Script (`script.js`)

Build comprehensive load test:

```javascript
import { testEventPage } from './tests/events.test.js';
import { getBaseUrl } from './config/environments.js';
import { thinkTime } from './utils/helpers.js';

export const options = {
  stages: [
    { duration: '2m', target: 20 },
    { duration: '5m', target: 50 },
    { duration: '2m', target: 0 },
  ],
  thresholds: {
    'http_req_duration': ['p(95)<2000'],
    'http_req_failed': ['rate<0.01'],
  },
};

export default function() {
  const baseUrl = getBaseUrl();
  testEventPage(baseUrl);
  thinkTime(2, 5);
}
```

### Step 7: Create Scenario Files (`scenarios/*.js`)

Each scenario imports from main test implementation:

```javascript
// scenarios/smoke.js
import { testEventPage } from '../tests/events.test.js';
import { getBaseUrl } from '../config/environments.js';

export const options = {
  stages: [
    { duration: '30s', target: 1 },
    { duration: '30s', target: 2 },
  ],
  thresholds: {
    'http_req_duration': ['p(95)<2000'],
    'http_req_failed': ['rate<0.01'],
  },
};

export default function() {
  const baseUrl = getBaseUrl();
  testEventPage(baseUrl);
  sleep(1);
}
```

### Step 8: Create .gitignore

Ignore test outputs:

```
# Test results
*-results.json
results/
*.log

# K6 cloud results
k6-cloud/

# OS files
.DS_Store
```

### Step 9: Document in README.md

Create comprehensive documentation with:
- Overview of what's tested
- Prerequisites and installation
- How to run each scenario
- How to update test data
- Environment variables
- Troubleshooting tips



---

## Approach 1: Go Service (Backend API)

### Characteristics
- Focus on API endpoints (REST, GraphQL, gRPC)
- Test response times, throughput, error rates
- No HTML parsing or asset loading required
- Direct backend testing with request payloads

### Script Template (Go Service)

```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';
import { Rate, Trend, Counter } from 'k6/metrics';

// Custom Metrics
const errorRate = new Rate('errors');
const requestDuration = new Trend('request_duration');
const requestCount = new Counter('requests_total');

// Configuration
const BASE_URL = __ENV.BASE_URL || 'http://localhost:8080';
const API_PATH = __ENV.API_PATH || '/api/v1';

export const options = {
  stages: [
    { duration: '2m', target: 50 },   // Ramp up
    { duration: '5m', target: 50 },   // Stay at 50
    { duration: '2m', target: 100 },  // Ramp to 100
    { duration: '5m', target: 100 },  // Stay at 100
    { duration: '2m', target: 0 },    // Ramp down
  ],
  thresholds: {
    'http_req_duration': ['p(95)<500', 'p(99)<1000'],
    'http_req_failed': ['rate<0.01'],
    'errors': ['rate<0.05'],
  },
};

export function setup() {
  console.log(`Testing API: ${BASE_URL}${API_PATH}`);
  
  // Health check
  const health = http.get(`${BASE_URL}/health`);
  if (health.status !== 200) {
    throw new Error(`Health check failed: ${health.status}`);
  }
  
  return {
    startTime: new Date().toISOString(),
  };
}

export default function (data) {
  const headers = {
    'Content-Type': 'application/json',
    'Accept': 'application/json',
    'Authorization': __ENV.AUTH_TOKEN || 'Bearer test-token',
  };
  
  // Test Case 1: GET request
  const getResponse = http.get(`${BASE_URL}${API_PATH}/resource`, {
    headers,
    tags: { name: 'GET_Resource' },
  });
  
  requestCount.add(1);
  requestDuration.add(getResponse.timings.duration);
  
  const getCheck = check(getResponse, {
    'GET status is 200': (r) => r.status === 200,
    'GET response has data': (r) => r.json('data') !== undefined,
    'GET response time < 500ms': (r) => r.timings.duration < 500,
  });
  
  errorRate.add(!getCheck);
  sleep(1);
  
  // Test Case 2: POST request
  const payload = JSON.stringify({
    name: `test-${Date.now()}`,
    value: Math.random() * 100,
  });
  
  const postResponse = http.post(`${BASE_URL}${API_PATH}/resource`, payload, {
    headers,
    tags: { name: 'POST_Resource' },
  });
  
  requestCount.add(1);
  requestDuration.add(postResponse.timings.duration);
  
  const postCheck = check(postResponse, {
    'POST status is 201 or 200': (r) => r.status === 201 || r.status === 200,
    'POST response has id': (r) => r.json('id') !== undefined,
    'POST response time < 1000ms': (r) => r.timings.duration < 1000,
  });
  
  errorRate.add(!postCheck);
  sleep(2);
}

export function teardown(data) {
  console.log('Load test completed');
}
```

### Key Points for Go Services
- Test actual backend endpoints (no frontend assets)
- Include authentication headers (JWT, API keys, etc.)
- Test different HTTP methods (GET, POST, PUT, DELETE)
- Use appropriate request payloads (JSON, protobuf, etc.)
- Monitor API-specific metrics (response time, error rate, throughput)
- Consider database connection limits and resource constraints

---

## Approach 2: Micro-Frontend (MFE)

### Characteristics
- Focus on frontend performance (SSR HTML, JS bundles, CSS)
- Parse HTML to extract and load critical assets
- Simulate real browser behavior
- Measure frontend-specific metrics

### Script Template (MFE)

```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';
import { Rate, Trend, Counter } from 'k6/metrics';

// Custom Metrics for MFE Performance
const failureRate = new Rate('failed_requests');
const mfePageLoadDuration = new Trend('mfe_page_load_duration');
const mfeBundleLoadDuration = new Trend('mfe_bundle_load_duration');
const mfeVendorLoadDuration = new Trend('mfe_vendor_load_duration');
const mfeCssLoadDuration = new Trend('mfe_css_load_duration');
const mfeTotalAssetSize = new Trend('mfe_total_asset_size_bytes');
const mfePageViews = new Counter('mfe_page_views');

// Configuration
const BASE_URL = __ENV.BASE_URL || 'http://localhost:3000';
const MFE_CONTEXT = __ENV.MFE_CONTEXT || '';

export const options = {
  stages: [
    { duration: '2m', target: 50 },
    { duration: '5m', target: 50 },
    { duration: '2m', target: 100 },
    { duration: '5m', target: 100 },
    { duration: '2m', target: 0 },
  ],
  thresholds: {
    'http_req_duration': ['p(95)<1500'],
    'http_req_failed': ['rate<0.02'],
    'failed_requests': ['rate<0.02'],
    'mfe_page_load_duration': ['p(95)<1000'],
    'mfe_bundle_load_duration': ['p(95)<800'],
    'mfe_css_load_duration': ['p(95)<500'],
  },
};

export function setup() {
  console.log(`Testing MFE: ${BASE_URL}${MFE_CONTEXT}`);
  
  // Health check
  const health = http.get(`${BASE_URL}${MFE_CONTEXT}/`);
  if (health.status !== 200) {
    console.warn(`Health check returned ${health.status}`);
  }
  
  return {
    startTime: new Date().toISOString(),
  };
}

export default function (data) {
  const mfeUrl = `${BASE_URL}${MFE_CONTEXT}/`;
  
  const headers = {
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'Accept-Encoding': 'gzip, deflate, br',
    'Accept-Language': 'en-US,en;q=0.9',
    'User-Agent': 'k6-load-test/1.0',
  };
  
  // Step 1: Load the MFE page (Server-Side Rendered HTML)
  const pageResponse = http.get(mfeUrl, { 
    headers,
    tags: { name: 'MFE_Page_Load' }
  });
  
  mfePageLoadDuration.add(pageResponse.timings.duration);
  mfePageViews.add(1);
  
  const pageLoadCheck = check(pageResponse, {
    'MFE page loaded successfully': (r) => r.status === 200,
    'MFE page load time < 1s': (r) => r.timings.duration < 1000,
    'MFE HTML contains bundle script': (r) => r.body.includes('bundle') || r.body.includes('.js'),
    'MFE HTML is not empty': (r) => r.body.length > 1000,
  });
  
  failureRate.add(pageLoadCheck ? 0 : 1);
  
  if (pageResponse.status !== 200) {
    console.error(`Page load failed: ${pageResponse.status}`);
    return;
  }
  
  // Step 2: Extract asset URLs from HTML
  const scriptMatches = pageResponse.body.match(/<script[^>]+src="([^"]+)"/gi) || [];
  const linkMatches = pageResponse.body.match(/<link[^>]+href="([^"]+\.css)"/gi) || [];
  
  const assetUrls = [];
  
  // Extract JavaScript bundles
  scriptMatches.forEach(match => {
    const srcMatch = match.match(/src="([^"]+)"/);
    if (srcMatch && srcMatch[1]) {
      let assetUrl = srcMatch[1];
      // Skip external resources
      if (assetUrl.startsWith('//') || assetUrl.startsWith('http://') || assetUrl.startsWith('https://')) {
        return;
      }
      // Handle relative URLs
      if (!assetUrl.startsWith('http')) {
        assetUrl = assetUrl.startsWith('/') ? `${BASE_URL}${assetUrl}` : `${BASE_URL}/${assetUrl}`;
      }
      assetUrls.push({ url: assetUrl, type: 'js' });
    }
  });
  
  // Extract CSS files
  linkMatches.forEach(match => {
    const hrefMatch = match.match(/href="([^"]+)"/);
    if (hrefMatch && hrefMatch[1]) {
      let assetUrl = hrefMatch[1];
      // Skip external resources
      if (assetUrl.startsWith('//') || assetUrl.startsWith('http://') || assetUrl.startsWith('https://')) {
        return;
      }
      // Handle relative URLs
      if (!assetUrl.startsWith('http')) {
        assetUrl = assetUrl.startsWith('/') ? `${BASE_URL}${assetUrl}` : `${BASE_URL}/${assetUrl}`;
      }
      assetUrls.push({ url: assetUrl, type: 'css' });
    }
  });
  
  // Step 3: Load all critical assets in parallel (simulating browser behavior)
  const requests = assetUrls.map(asset => ({
    method: 'GET',
    url: asset.url,
    params: {
      headers: {
        'Accept': asset.type === 'js' ? 'application/javascript, */*' : 'text/css,*/*;q=0.1',
        'Accept-Encoding': 'gzip, deflate, br',
        'Referer': mfeUrl,
      },
      tags: { name: `Asset_${asset.type}` }
    }
  }));
  
  const responses = http.batch(requests);
  
  let totalAssetSize = 0;
  
  // Process all asset responses
  responses.forEach((assetResponse, index) => {
    const asset = assetUrls[index];
    
    // Categorize asset for metrics
    if (asset.url.includes('bundle') && asset.url.includes('.js')) {
      if (asset.url.includes('vendors') || asset.url.includes('vendor')) {
        mfeVendorLoadDuration.add(assetResponse.timings.duration);
      } else {
        mfeBundleLoadDuration.add(assetResponse.timings.duration);
      }
    } else if (asset.url.includes('.css')) {
      mfeCssLoadDuration.add(assetResponse.timings.duration);
    }
    
    const assetCheck = check(assetResponse, {
      'asset loaded successfully': (r) => r.status === 200,
      'asset load time < 1s': (r) => r.timings.duration < 1000,
    });
    
    failureRate.add(assetCheck ? 0 : 1);
    
    // Track total asset size
    totalAssetSize += assetResponse.body_size || 0;
  });
  
  mfeTotalAssetSize.add(totalAssetSize);
  
  // Simulate user think time
  sleep(Math.random() * 3 + 2);
}

export function teardown(data) {
  console.log('MFE load test completed');
}
```

### Key Points for MFEs
- Load SSR HTML first, then extract and load assets
- Simulate real browser behavior (parallel asset loading)
- Use realistic browser headers
- Measure frontend-specific metrics (bundle size, CSS load time, etc.)
- Skip external resources (CDN assets, third-party scripts)
- Track total asset size transferred
- Consider caching behavior (70-80% of users may have cached assets)

---

## Test Data Management

### Data Directory Structure

Store test data in the `perf-test/data/` directory as JSON files for easy maintenance and updates.

**Example: `perf-test/data/event-ids.json`**
```json
{
  "eventIds": [
    "ec60215",
    "ec36706",
    "ec65452",
    "ec67131"
  ],
  "metadata": {
    "lastUpdated": "2026-03-12",
    "description": "Active event IDs for load testing.",
    "howToUpdate": "Replace with active events from QA/staging environment."
  }
}
```

### Importing Test Data

In your test files, import data directly:

```javascript
import eventData from '../data/event-ids.json';

export const testData = {
  eventIds: eventData.eventIds,
  // ... other test data
};
```

### Best Practices for Test Data

1. **Separate Data from Logic**: Keep test data in JSON files, not hardcoded in tests
2. **Add Metadata**: Include update date and instructions in your data files
3. **Use Real Data**: Populate with actual IDs/values from test environments
4. **Document Updates**: Note when and why test data should be refreshed
5. **Version Control**: Commit data files so team has consistent test data
6. **Environment-Specific**: Consider separate data files per environment if needed

### Common Test Data Files

```
perf-test/data/
├── event-ids.json       # Event identifiers
├── user-accounts.json   # Test user credentials
├── product-ids.json     # Product SKUs or IDs
├── locations.json       # Store/location data
└── test-config.json     # Test-specific configuration
```

---

## Environment Variables

### Common Variables
- `ENV` or `ENVIRONMENT`: Environment name (local, dev, qa, staging)
  - **Important**: Do NOT include production environment to prevent accidental load testing
- `BASE_URL`: Base URL of the application (if not using environment config)
- `AUTH_TOKEN`: Authorization token for authenticated requests
- `TEST_USER_ID`: User ID for authentication tests

### Environment Configuration File

Create `perf-test/config/environments.js` to manage environments:

```javascript
export const environments = {
  local: {
    baseUrl: 'http://localhost:3000',
    protocol: 'http',
  },
  dev: {
    baseUrl: 'https://dev.example.com',
    protocol: 'https',
  },
  qa: {
    baseUrl: 'https://qa.example.com',
    protocol: 'https',
  },
  staging: {
    baseUrl: 'https://staging.example.com',
    protocol: 'https',
  },
  // DO NOT INCLUDE PRODUCTION
  // Load tests should never run against production
};

export function getBaseUrl() {
  const env = __ENV.ENV || 'local';
  const config = environments[env];
  return config ? config.baseUrl : environments.local.baseUrl;
}
```

---

## Documentation Files

### README.md Structure
- Overview of what the test covers
- Folder structure explanation
- Prerequisites and installation instructions (k6 installation)
- Quick start guide with `script.js`
- Test scenario descriptions (smoke, load, stress, spike)
- How to update test data in `data/` directory
- Environment variables and configuration
- Usage examples for different environments
- Key metrics and performance targets
- Troubleshooting section
- CI/CD integration examples

### Data File Documentation
Each JSON file in `data/` should include a `metadata` section:
```json
{
  "eventIds": [...],
  "metadata": {
    "lastUpdated": "2026-03-12",
    "description": "What this data is used for",
    "howToUpdate": "Instructions for updating this data",
    "source": "Where to get fresh data (DB query, API, etc.)"
  }
}
```

---

## Best Practices

1. **Start Small:** Begin with smoke tests before running full load tests
2. **Never Test Production:** Exclude production URLs from environment configuration to prevent accidental load testing
3. **Separate Test Data:** Store test data in `data/` directory as JSON files for easy updates
4. **Modular Structure:** Keep test logic in `tests/`, scenarios in `scenarios/`, and utilities in `utils/`
5. **Realistic Headers:** Use actual browser/client User-Agent strings
6. **Think Time:** Add sleep() between requests to simulate real users
7. **Error Handling:** Check for critical failures and log detailed errors
8. **Environment Awareness:** Use different load profiles for different environments (e.g., QA gets less load than staging)
9. **Thresholds:** Set realistic performance thresholds based on requirements
10. **Custom Metrics:** Track application-specific metrics beyond standard HTTP metrics
11. **Setup/Teardown:** Use setup() for health checks and teardown() for cleanup
12. **Tags:** Tag requests for better reporting and filtering
13. **Batch Requests:** Use http.batch() for parallel requests (simulates browser behavior)
14. **Version Control:** Commit all test files and data so the team has consistent tests

---

## Common Patterns

### Environment-Based URL Selection

**Using Config File (Recommended):**
```javascript
import { getBaseUrl } from './config/environments.js';

export default function() {
  const baseUrl = getBaseUrl();
  // Use baseUrl in tests
}
```

**Using Environment Variable:**
```javascript
let BASE_URL;
if (__ENV.BASE_URL) {
  BASE_URL = __ENV.BASE_URL;
} else {
  const environment = (__ENV.ENV || 'local').toLowerCase();
  switch (environment) {
    case 'dev': BASE_URL = 'https://dev.example.com'; break;
    case 'qa': BASE_URL = 'https://qa.example.com'; break;
    case 'staging': BASE_URL = 'https://staging.example.com'; break;
    default: BASE_URL = 'http://localhost:3000'; break;
  }
}
```

### Importing Test Data

```javascript
import eventData from './data/event-ids.json';
import userData from './data/user-accounts.json';

export const testData = {
  eventIds: eventData.eventIds,
  users: userData.users,
};

function getRandomEventId() {
  return testData.eventIds[Math.floor(Math.random() * testData.eventIds.length)];
}
```

### User Behavior Simulation
```javascript
// Simulate different user types
const userType = Math.random();
let thinkTime;

if (userType < 0.80) {
  thinkTime = Math.random() * 3 + 2;  // Normal: 2-5s
} else if (userType < 0.95) {
  thinkTime = Math.random() + 0.5;    // Quick: 0.5-1.5s
} else {
  thinkTime = Math.random() * 5 + 5;  // Slow: 5-10s
}

sleep(thinkTime);
```

---

## Running Tests

```bash
# Main comprehensive load test (recommended)
k6 run perf-test/script.js

# Main test with specific environment
k6 run -e ENV=qa perf-test/script.js

# Smoke test (quick validation before deployment)
k6 run perf-test/scenarios/smoke.js

# Load test (normal traffic patterns)
k6 run perf-test/scenarios/load.js

# Stress test (find breaking points)
k6 run perf-test/scenarios/stress.js

# Spike test (sudden traffic bursts)
k6 run perf-test/scenarios/spike.js

# Run with specific environment
k6 run -e ENV=qa perf-test/scenarios/load.js

# Custom VUs and duration
k6 run --vus 100 --duration 5m perf-test/script.js

# Custom stages
k6 run --stage 2m:10,5m:50,2m:0 perf-test/script.js

# With cloud output
k6 run --out cloud perf-test/script.js

# JSON output for CI/CD
k6 run --out json=results.json perf-test/script.js
```

---

## Troubleshooting

- **High error rates:** Check thresholds, increase ramp-up time, verify endpoints
- **Timeouts:** Increase `http_req_duration` threshold or optimize backend
- **Resource exhaustion:** Lower VU count for non-production environments
- **Asset not found:** Verify asset URL extraction regex patterns
- **Authentication failures:** Check token format and expiration

---

## References

- k6 Documentation: https://k6.io/docs/
- k6 Metrics: https://k6.io/docs/using-k6/metrics/
- k6 Thresholds: https://k6.io/docs/using-k6/thresholds/
- k6 Scenarios: https://k6.io/docs/using-k6/scenarios/
