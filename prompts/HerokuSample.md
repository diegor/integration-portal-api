# Banking API Developer Portal - Application Requirements

## Overview
Create a web application that serves as a Developer Portal for banking APIs. The portal connects to a Salesforce Integration Portal API to fetch and display API assets with interactive documentation.



## API Integration

### Authentication
- **Type**: OAuth 2.0 client credentials flow (system-to-system)
- **Token Endpoint**: `{SF_URL}/services/oauth2/token`
- **Grant Type**: `client_credentials`
- **Credentials**: Provided via environment variables (CLIENT_ID, CLIENT_SECRET)
- **Token Caching**: Implement token caching with expiration handling

### Salesforce Configuration
- **Base URL**: Configurable via SF_URL environment variable
- **Endpoint to get assets**: `{SF_URL}/services/data/v65.0/integration/portals/{PORTAL_ID}/assets`
- **Endpoint to get asset detail**: `{SF_URL}/services/data/v65.0/integration/portals/{PORTAL_ID}/assets/{ASSET_ID}`
- **Portal ID**: Configurable (e.g., "1m7xx0000000001AAA")
- **Response Format for get assets**:
  ```
  {
  "count": 4,
  "assets": [
    {
      "description": "Locate ATMs",
      "id": "1m8xx0000000001AAA",
      "managedContentId": "20Yxx0000011QtSEAU",
      "managedContentText": null,
      "name": "ATM",
      "portalId": "1m7xx0000000001AAA",
      "source": "ApiCatalog",
      "sourceReference": "1LKxx0000004CAe",
      "specification": null,
      "type": "RestApi",
      "version": "1.0"
    },
    {
      "description": "Manage personal accounts",
      "id": "1m8xx0000000002AAA",
      "managedContentId": "20Yxx0000011QtTEAU",
      "managedContentText": null,
      "name": "Accounts",
      "portalId": "1m7xx0000000001AAA",
      "source": "ApiCatalog",
      "sourceReference": "1LKxx0000004CAg",
      "specification": null,
      "type": "RestApi",
      "version": "1.0"
    },
    {
      "description": "Manage Investment Accounts",
      "id": "1m8xx0000000003AAA",
      "managedContentId": "20Yxx0000011QtUEAU",
      "managedContentText": null,
      "name": "Investments",
      "portalId": "1m7xx0000000001AAA",
      "source": "ApiCatalog",
      "sourceReference": "1LKxx0000004CCG",
      "specification": null,
      "type": "RestApi",
      "version": "1.0"
    },
    {
      "description": "Manage Loans for Customers",
      "id": "1m8xx0000000004AAA",
      "managedContentId": "20Yxx0000011QtVEAU",
      "managedContentText": null,
      "name": "Loans",
      "portalId": "1m7xx0000000001AAA",
      "source": "ApiCatalog",
      "sourceReference": "1LKxx0000004CCH",
      "specification": null,
      "type": "RestApi",
      "version": "1.0"
    }
  ]
}


- **Response Sample for asset detail**

```
  {
    "description": "Locate ATMs",
    "id": "1m8xx0000000001AAA",
    "managedContentId": "20Yxx0000011QtSEAU",
    "managedContentText": null,
    "name": "ATM",
    "portalId": "1m7xx0000000001AAA",
    "source": "ApiCatalog",
    "sourceReference": "1LKxx0000004CAe",
    "specification": "{\n  &quot;openapi&quot;: &quot;3.0.0&quot;,\n  &quot;info&quot;: {\n    &quot;title&quot;: &quot;Bank ATM Locator API&quot;,\n    &quot;description&quot;: &quot;API for locating nearby Automatic Teller Machines (ATMs) based on geography, address, and ID.&quot;,\n    &quot;version&quot;: &quot;1.1.0&quot;\n  },\n  &quot;tags&quot;: [\n    {\n      &quot;name&quot;: &quot;ATM Location&quot;,\n      &quot;description&quot;: &quot;Operations related to searching and retrieving ATM locations.&quot;\n    }\n  ],\n  &quot;paths&quot;: {\n    &quot;/atm/search/coordinates&quot;: {\n      &quot;get&quot;: {\n        &quot;servers&quot;: [\n          {\n            &quot;url&quot;: &quot;https://api.bankname.com/v1&quot;,\n            &quot;description&quot;: &quot;Production server&quot;\n          },\n          {\n            &quot;url&quot;: &quot;https://sandbox.api.bankname.com/v1&quot;,\n            &quot;description&quot;: &quot;Sandbox environment&quot;\n          }\n        ],\n        &quot;tags&quot;: [\n          &quot;ATM Location&quot;\n        ],\n        &quot;summary&quot;: &quot;Search for nearby ATMs by coordinates&quot;,\n        &quot;operationId&quot;: &quot;getNearbyATMsByCoords&quot;,\n        &quot;description&quot;: &quot;Retrieves a list of ATMs within a specified radius of a given geographic coordinate, with optional service filtering.&quot;,\n        &quot;parameters&quot;: [\n          {\n            &quot;name&quot;: &quot;latitude&quot;,\n            &quot;in&quot;: &quot;query&quot;,\n            &quot;description&quot;: &quot;The center point latitude for the search.&quot;,\n            &quot;required&quot;: true,\n            &quot;schema&quot;: {\n              &quot;type&quot;: &quot;number&quot;,\n              &quot;format&quot;: &quot;float&quot;,\n              &quot;example&quot;: 34.0522\n            }\n          },\n          {\n            &quot;name&quot;: &quot;longitude&quot;,\n            &quot;in&quot;: &quot;query&quot;,\n            &quot;description&quot;: &quot;The center point longitude for the search.&quot;,\n            &quot;required&quot;: true,\n            &quot;schema&quot;: {\n              &quot;type&quot;: &quot;number&quot;,\n              &quot;format&quot;: &quot;float&quot;,\n              &quot;example&quot;: -118.2437\n            }\n          },\n          {\n            &quot;name&quot;: &quot;radius&quot;,\n            &quot;in&quot;: &quot;query&quot;,\n            &quot;description&quot;: &quot;Search radius in kilometers .&quot;,\n            &quot;required&quot;: false,\n            &quot;schema&quot;: {\n              &quot;type&quot;: &quot;integer&quot;,\n              &quot;format&quot;: &quot;int32&quot;,\n              &quot;default&quot;: 5\n            }\n          },\n          {\n            &quot;name&quot;: &quot;services&quot;,\n            &quot;in&quot;: &quot;query&quot;,\n            &quot;description&quot;: &quot;Filter by available services. Multiple values allowed.&quot;,\n            &quot;required&quot;: false,\n            &quot;style&quot;: &quot;form&quot;,\n            &quot;explode&quot;: true,\n            &quot;schema&quot;: {\n              &quot;type&quot;: &quot;array&quot;,\n              &quot;items&quot;: {\n                &quot;$ref&quot;: &quot;#/components/schemas/ATMService&quot;\n              }\n            }\n          }\n        ],\n        &quot;responses&quot;: {\n          &quot;200&quot;: {\n            &quot;description&quot;: &quot;Successful response - list of nearby ATMs.&quot;,\n            &quot;content&quot;: {\n              &quot;application/json&quot;: {\n                &quot;schema&quot;: {\n                  &quot;type&quot;: &quot;array&quot;,\n                  &quot;items&quot;: {\n                    &quot;$ref&quot;: &quot;#/components/schemas/ATMLocation&quot;\n                  }\n                }\n              }\n            }\n          },\n          &quot;400&quot;: {\n            &quot;description&quot;: &quot;Invalid request parameters (e.g., missing coordinates or invalid radius).&quot;,\n            &quot;content&quot;: {\n              &quot;application/json&quot;: {\n                &quot;schema&quot;: {\n                  &quot;$ref&quot;: &quot;#/components/schemas/ErrorResponse&quot;\n                }\n              }\n            }\n          },\n          &quot;500&quot;: {\n            &quot;description&quot;: &quot;Internal server error.&quot;,\n            &quot;content&quot;: {\n              &quot;application/json&quot;: {\n                &quot;schema&quot;: {\n                  &quot;$ref&quot;: &quot;#/components/schemas/ErrorResponse&quot;\n                }\n              }\n            }\n          }\n        }\n      }\n    },\n    &quot;/atm/search/address&quot;: {\n      &quot;get&quot;: {\n        &quot;servers&quot;: [\n          {\n            &quot;url&quot;: &quot;https://api.bankname.com/v1&quot;,\n            &quot;description&quot;: &quot;Production server&quot;\n          },\n          {\n            &quot;url&quot;: &quot;https://sandbox.api.bankname.com/v1&quot;,\n            &quot;description&quot;: &quot;Sandbox environment&quot;\n          }\n        ],\n        &quot;tags&quot;: [\n          &quot;ATM Location&quot;\n        ],\n        &quot;summary&quot;: &quot;Search for nearby ATMs by address or postal code&quot;,\n        &quot;operationId&quot;: &quot;getNearbyATMsByAddress&quot;,\n        &quot;description&quot;: &quot;Retrieves a list of ATMs near a specified address, postal code, or city.&quot;,\n        &quot;parameters&quot;: [\n          {\n            &quot;name&quot;: &quot;searchString&quot;,\n            &quot;in&quot;: &quot;query&quot;,\n            &quot;description&quot;: &quot;A full or partial address, city, or postal code (e.g., &#92;&quot;90210&#92;&quot; or &#92;&quot;123 Main St, Anytown&#92;&quot;).&quot;,\n            &quot;required&quot;: true,\n            &quot;schema&quot;: {\n              &quot;type&quot;: &quot;string&quot;,\n              &quot;example&quot;: &quot;100 Pine St, New York&quot;\n            }\n          },\n          {\n            &quot;name&quot;: &quot;radius&quot;,\n            &quot;in&quot;: &quot;query&quot;,\n            &quot;description&quot;: &quot;Search radius in kilometers.&quot;,\n            &quot;required&quot;: false,\n            &quot;schema&quot;: {\n              &quot;type&quot;: &quot;integer&quot;,\n              &quot;format&quot;: &quot;int32&quot;,\n              &quot;default&quot;: 5\n            }\n          },\n          {\n            &quot;name&quot;: &quot;surchargeFree&quot;,\n            &quot;in&quot;: &quot;query&quot;,\n            &quot;description&quot;: &quot;Filter to include only surcharge-free ATMs.&quot;,\n            &quot;required&quot;: false,\n            &quot;schema&quot;: {\n              &quot;type&quot;: &quot;boolean&quot;,\n              &quot;default&quot;: false\n            }\n          }\n        ],\n        &quot;responses&quot;: {\n          &quot;200&quot;: {\n            &quot;description&quot;: &quot;Successful response - list of nearby ATMs.&quot;,\n            &quot;content&quot;: {\n              &quot;application/json&quot;: {\n                &quot;schema&quot;: {\n                  &quot;type&quot;: &quot;array&quot;,\n                  &quot;items&quot;: {\n                    &quot;$ref&quot;: &quot;#/components/schemas/ATMLocation&quot;\n                  }\n                }\n              }\n            }\n          },\n          &quot;400&quot;: {\n            &quot;description&quot;: &quot;Invalid request parameters (e.g., missing searchString).&quot;,\n            &quot;content&quot;: {\n              &quot;application/json&quot;: {\n                &quot;schema&quot;: {\n                  &quot;$ref&quot;: &quot;#/components/schemas/ErrorResponse&quot;\n                }\n              }\n            }\n          }\n        }\n      }\n    },\n    &quot;/atm/{atmId}&quot;: {\n      &quot;get&quot;: {\n        &quot;servers&quot;: [\n          {\n            &quot;url&quot;: &quot;https://api.bankname.com/v1&quot;,\n            &quot;description&quot;: &quot;Production server&quot;\n          },\n          {\n            &quot;url&quot;: &quot;https://sandbox.api.bankname.com/v1&quot;,\n            &quot;description&quot;: &quot;Sandbox environment&quot;\n          }\n        ],\n        &quot;tags&quot;: [\n          &quot;ATM Location&quot;\n        ],\n        &quot;summary&quot;: &quot;Get ATM details by ID&quot;,\n        &quot;operationId&quot;: &quot;getATMById&quot;,\n        &quot;description&quot;: &quot;Retrieves the detailed information for a specific ATM using its unique identifier.&quot;,\n        &quot;parameters&quot;: [\n          {\n            &quot;name&quot;: &quot;atmId&quot;,\n            &quot;in&quot;: &quot;path&quot;,\n            &quot;description&quot;: &quot;The unique identifier of the ATM.&quot;,\n            &quot;schema&quot;: {\n              &quot;type&quot;: &quot;string&quot;,\n              &quot;example&quot;: &quot;ATM-45678&quot;\n            },\n            &quot;required&quot;: true\n          }\n        ],\n        &quot;responses&quot;: {\n          &quot;200&quot;: {\n            &quot;description&quot;: &quot;Successful response - detailed ATM object.&quot;,\n            &quot;content&quot;: {\n              &quot;application/json&quot;: {\n                &quot;schema&quot;: {\n                  &quot;$ref&quot;: &quot;#/components/schemas/ATMLocation&quot;\n                }\n              }\n            }\n          },\n          &quot;404&quot;: {\n            &quot;description&quot;: &quot;ATM not found for the given ID.&quot;,\n            &quot;content&quot;: {\n              &quot;application/json&quot;: {\n                &quot;schema&quot;: {\n                  &quot;$ref&quot;: &quot;#/components/schemas/ErrorResponse&quot;\n                }\n              }\n            }\n          },\n          &quot;500&quot;: {\n            &quot;description&quot;: &quot;Internal server error.&quot;,\n            &quot;content&quot;: {\n              &quot;application/json&quot;: {\n                &quot;schema&quot;: {\n                  &quot;$ref&quot;: &quot;#/components/schemas/ErrorResponse&quot;\n                }\n              }\n            }\n          }\n        }\n      }\n    }\n  },\n  &quot;components&quot;: {\n    &quot;schemas&quot;: {\n      &quot;ATMService&quot;: {\n        &quot;type&quot;: &quot;string&quot;,\n        &quot;description&quot;: &quot;Available services at the ATM.&quot;,\n        &quot;enum&quot;: [\n          &quot;CashWithdrawal&quot;,\n          &quot;Deposits&quot;,\n          &quot;PINChange&quot;,\n          &quot;BalanceInquiry&quot;,\n          &quot;MobileTopUp&quot;,\n          &quot;SurchargeFree&quot;,\n          &quot;ForeignCurrency&quot;\n        ]\n      },\n      &quot;ATMLocation&quot;: {\n        &quot;type&quot;: &quot;object&quot;,\n        &quot;description&quot;: &quot;Details of an individual ATM.&quot;,\n        &quot;required&quot;: [\n          &quot;id&quot;,\n          &quot;name&quot;,\n          &quot;location&quot;,\n          &quot;address&quot;\n        ],\n        &quot;properties&quot;: {\n          &quot;id&quot;: {\n            &quot;type&quot;: &quot;string&quot;,\n            &quot;description&quot;: &quot;Unique identifier for the ATM.&quot;,\n            &quot;example&quot;: &quot;ATM-12345&quot;\n          },\n          &quot;name&quot;: {\n            &quot;type&quot;: &quot;string&quot;,\n            &quot;description&quot;: &quot;Location name (e.g., &#92;&quot;Bank Headquarters Lobby&#92;&quot;).&quot;,\n            &quot;example&quot;: &quot;Downtown Branch ATM 1&quot;\n          },\n          &quot;location&quot;: {\n            &quot;$ref&quot;: &quot;#/components/schemas/GeoPoint&quot;\n          },\n          &quot;address&quot;: {\n            &quot;$ref&quot;: &quot;#/components/schemas/Address&quot;\n          },\n          &quot;distanceKm&quot;: {\n            &quot;type&quot;: &quot;number&quot;,\n            &quot;format&quot;: &quot;float&quot;,\n            &quot;description&quot;: &quot;Distance from the search center in kilometers.&quot;,\n            &quot;example&quot;: 1.5\n          },\n          &quot;isAvailable247&quot;: {\n            &quot;type&quot;: &quot;boolean&quot;,\n            &quot;description&quot;: &quot;Indicates if the ATM is accessible 24/7.&quot;,\n            &quot;example&quot;: true\n          },\n          &quot;services&quot;: {\n            &quot;items&quot;: {\n              &quot;$ref&quot;: &quot;#/components/schemas/ATMService&quot;\n            },\n            &quot;type&quot;: &quot;array&quot;,\n            &quot;description&quot;: &quot;Services available at this ATM.&quot;,\n            &quot;example&quot;: [\n              &quot;CashWithdrawal&quot;,\n              &quot;BalanceInquiry&quot;,\n              &quot;SurchargeFree&quot;\n            ]\n          },\n          &quot;accessNotes&quot;: {\n            &quot;nullable&quot;: true,\n            &quot;type&quot;: &quot;string&quot;,\n            &quot;description&quot;: &quot;Specific instructions for access (e.g., &#92;&quot;Inside mall, second floor&#92;&quot;).&quot;\n          }\n        }\n      },\n      &quot;ErrorResponse&quot;: {\n        &quot;type&quot;: &quot;object&quot;,\n        &quot;description&quot;: &quot;Standard error response format.&quot;,\n        &quot;properties&quot;: {\n          &quot;code&quot;: {\n            &quot;type&quot;: &quot;string&quot;,\n            &quot;example&quot;: &quot;BAD_REQUEST&quot;\n          },\n          &quot;message&quot;: {\n            &quot;type&quot;: &quot;string&quot;,\n            &quot;example&quot;: &quot;Missing required query parameter: latitude.&quot;\n          }\n        }\n      },\n      &quot;GeoPoint&quot;: {\n        &quot;type&quot;: &quot;object&quot;,\n        &quot;description&quot;: &quot;Geographic coordinates.&quot;,\n        &quot;required&quot;: [\n          &quot;latitude&quot;,\n          &quot;longitude&quot;\n        ],\n        &quot;properties&quot;: {\n          &quot;latitude&quot;: {\n            &quot;type&quot;: &quot;number&quot;,\n            &quot;format&quot;: &quot;float&quot;,\n            &quot;example&quot;: 34.0522\n          },\n          &quot;longitude&quot;: {\n            &quot;type&quot;: &quot;number&quot;,\n            &quot;format&quot;: &quot;float&quot;,\n            &quot;example&quot;: -118.2437\n          }\n        }\n      },\n      &quot;Address&quot;: {\n        &quot;type&quot;: &quot;object&quot;,\n        &quot;description&quot;: &quot;Structured postal address information.&quot;,\n        &quot;required&quot;: [\n          &quot;street&quot;,\n          &quot;city&quot;,\n          &quot;country&quot;\n        ],\n        &quot;properties&quot;: {\n          &quot;street&quot;: {\n            &quot;type&quot;: &quot;string&quot;,\n            &quot;example&quot;: &quot;123 Main St&quot;\n          },\n          &quot;city&quot;: {\n            &quot;type&quot;: &quot;string&quot;,\n            &quot;example&quot;: &quot;Los Angeles&quot;\n          },\n          &quot;stateProvince&quot;: {\n            &quot;type&quot;: &quot;string&quot;,\n            &quot;example&quot;: &quot;CA&quot;\n          },\n          &quot;postalCode&quot;: {\n            &quot;type&quot;: &quot;string&quot;,\n            &quot;example&quot;: &quot;90012&quot;\n          },\n          &quot;country&quot;: {\n            &quot;type&quot;: &quot;string&quot;,\n            &quot;example&quot;: &quot;USA&quot;\n          }\n        }\n      }\n    }\n  }\n}",
    "type": "RestApi",
    "version": "1.0"
}

  

### Asset Structure
Each asset contains:
- `id`: Asset id
- `name`: API name
- `description`: Brief description
- `version`: Version identifier 
- `specification`: specification (HTML-encoded JSON string) as OAS

## Application Features

### Home Page

1. **API Cards Carousel**
   - Display 3 cards per view on desktop
   - Responsive design (2 cards on tablet, 1 on mobile)
   - Auto-rotating carousel (5-second intervals)
   - Manual navigation with previous/next buttons
   - Pagination dots with click navigation
   
2. **API Card Design**
   - Icon/emoji representation (based on API name)
   - API name as title
   - Description (truncated to 3 lines)
   - Version badge (remove v prefix if already part of asset version
   - "View Details" button

### Detail Page
1. **API Header**
   - Large icon display
   - API name
   - Full description

     

2. **Interactive Documentation**
   - Full-page Swagger UI rendering
   - Parse and decode HTML entities from specification field 
   - Display complete OpenAPI specification
   - Interactive "Try it out" functionality
   - Request/response examples
   - Schema definitions
   - The specification field contains HTML entities (&quot;, &amp;, etc.) that need decoding before JSON parsing

#
#### Key Features
- Token caching with automatic refresh
- Error handling for API failures


### Frontend Implementation

#### Home Page (index.html, app.js)
- Fetch assets from `/api/assets`
- Parse response to extract items array
- Generate cards dynamically
- Implement carousel with smooth transitions
- Handle various API response formats
- URL-encode identifiers for detail links

#### Detail Page (detail.html, detail.js)
- Parse and decode HTML entities in specification
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

