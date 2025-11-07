# Banking API Developer Portal - Application Requirements

## Overview
Create a web application that serves as a Developer Portal for banking APIs. The portal connects to a Salesforce Integration Portal API to fetch and display API assets with interactive documentation.

## Technical Stack
- **Backend**: Node.js with Express.js
- **Frontend**: Vanilla JavaScript (no framework)
- **Styling**: Custom CSS with modern design
- **API Documentation**: Swagger UI for OpenAPI specifications
- **Deployment**: Heroku

## API Integration

### Authentication
- **Type**: OAuth 2.0 client credentials flow (system-to-system)
- **Token Endpoint**: `{SF_URL}/services/oauth2/token`
- **Grant Type**: `client_credentials`
- **Credentials**: Provided via environment variables (CLIENT_ID, CLIENT_SECRET)
- **Token Caching**: Implement token caching with expiration handling

### Salesforce Configuration
- **Base URL**: Configurable via SF_URL environment variable
- **API Endpoint**: `/services/data/v65.0/integration/mcp/portals/{PORTAL_ID}/assets`
- **Portal ID**: Configurable (e.g., "HomeBanking")
- **Response Format**: JSON with structure `{ count: number, items: [] }`

### Asset Structure
Each asset contains:
- `name`: API name
- `description`: Brief description
- `version`: Version identifier (e.g., "v1")
- `documentation`: OpenAPI specification (HTML-encoded JSON string)

## Application Features

### Home Page
1. **Header Section**
   - Banking-themed header with icon
   - Application title: "Banking API Developer Portal"
   - Subtitle: "Explore and integrate our banking APIs"

2. **API Cards Carousel**
   - Display 3 cards per view on desktop
   - Responsive design (2 cards on tablet, 1 on mobile)
   - Auto-rotating carousel (5-second intervals)
   - Manual navigation with previous/next buttons
   - Pagination dots with click navigation
   
3. **API Card Design**
   - Icon/emoji representation (based on API name)
   - API name as title
   - Description (truncated to 3 lines)
   - Version badge (without extra "v" prefix)
   - "View Details" button

### Detail Page
1. **API Header**
   - Large icon display
   - API name
   - Full description
   - Version badge

2. **Interactive Documentation**
   - Full-page Swagger UI rendering
   - Parse and decode HTML entities from documentation field
   - Display complete OpenAPI specification
   - Interactive "Try it out" functionality
   - Request/response examples
   - Schema definitions

### Backend Implementation

#### Server Endpoints
- `GET /` - Serve home page
- `GET /api/assets` - Fetch all assets from Salesforce
- `GET /api/assets/:id` - Fetch specific asset by id, externalId, or name
- `GET /detail/:id` - Serve detail page

#### Key Features
- Token caching with automatic refresh
- Error handling for API failures
- Support for multiple response structures (items, assets, records arrays)
- Search by multiple identifier types (id, externalId, name)

### Frontend Implementation

#### Home Page (index.html, app.js)
- Fetch assets from `/api/assets`
- Parse response to extract items array
- Generate cards dynamically
- Implement carousel with smooth transitions
- Handle various API response formats
- URL-encode identifiers for detail links

#### Detail Page (detail.html, detail.js)
- Extract asset ID from URL path
- Fetch asset data from `/api/assets/:id`
- Parse and decode HTML entities in documentation
- Initialize Swagger UI with decoded OpenAPI spec
- Display error messages for missing or invalid specs

## Styling Requirements

### Design Principles
- Modern, professional banking aesthetic
- Blue/green gradient color scheme
- Card-based UI with shadows and hover effects
- Responsive design for all screen sizes
- Smooth animations and transitions

### Key Components
- Gradient header with contrasting text
- Elevated cards with hover animations
- Version badges with primary color
- Rounded corners and soft shadows
- Mobile-first responsive breakpoints

## Deployment Configuration

### Heroku Setup
- **Procfile**: `web: node server.js`
- **Node.js Engine**: >=18.0.0
- **Environment Variables**:
  - `SF_URL`: Salesforce instance URL
  - `CLIENT_ID`: OAuth client ID
  - `CLIENT_SECRET`: OAuth client secret
  - `PORT`: Auto-assigned by Heroku

### Dependencies
```json
{
  "express": "^4.18.2",
  "axios": "^1.6.2",
  "dotenv": "^16.3.1"
}
File Structure
/
├── server.js              # Express server with OAuth and API endpoints
├── package.json           # Dependencies and npm scripts
├── Procfile              # Heroku process definition
├── .env.example          # Environment variable template
├── public/
│   ├── index.html        # Home page with carousel
│   ├── detail.html       # Detail page for API documentation
│   ├── app.js           # Home page JavaScript
│   ├── detail.js        # Detail page JavaScript
│   └── styles.css       # All application styles
Special Considerations
HTML Entity Decoding: Documentation field contains HTML-encoded JSON. Decode entities like &quot;, &amp;, &lt;, &gt;, &#39;, &#92; before parsing.

Asset Identification: Assets may not have an id field. Use name as fallback identifier and search by multiple fields (id, externalId, name).

Absolute Paths: Use absolute paths (/styles.css, /detail.js) for assets in detail.html to avoid 404 errors on nested routes.

Version Display: Don't add "v" prefix if version already contains it.

Response Flexibility: Handle various response structures (direct arrays, nested items/assets/records/data arrays).

Implementation Steps
Create Express server with OAuth authentication and token caching
Implement API proxy endpoints
Build home page with responsive carousel
Create detail page with Swagger UI integration
Add comprehensive error handling
Style with modern, professional design
Configure for Heroku deployment
Set environment variables
Deploy and test
Success Criteria
Home page displays all APIs in an attractive carousel
Carousel auto-rotates and has manual controls
Detail pages load and display interactive Swagger UI
OpenAPI specifications render correctly with decoded entities
Application is fully responsive on all devices
OAuth authentication works with token caching
Error states are handled gracefully
Application deploys successfully to Heroku


