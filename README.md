# NEXT-GEN E-COMMERCE DEVELOPMENT

*Real-Time API Sync for Automated Inventory & Product Data Integration*

*Built with the tools and technologies:*

![Next.js](https://img.shields.io/badge/Next.js-15-black?logo=next.js)
![TypeScript](https://img.shields.io/badge/TypeScript-5.8%2B-3178C6?logo=typescript&logoColor=white)
![Sanity](https://img.shields.io/badge/Sanity-Headless%20CMS-F03E2F?logo=sanity&logoColor=white)
![JSON](https://img.shields.io/badge/JSON-000000?style=flat&logo=json&logoColor=white)
![npm](https://img.shields.io/badge/npm-CB3837?style=flat&logo=npm&logoColor=white)
![Autoprefixer](https://img.shields.io/badge/Autoprefixer-DD3735?style=flat&logo=autoprefixer&logoColor=white)
![PostCSS](https://img.shields.io/badge/PostCSS-DD3A0A?style=flat&logo=postcss&logoColor=white)
![Prettier](https://img.shields.io/badge/Prettier-F7B93E?style=flat&logo=prettier&logoColor=black)
![.ENV](https://img.shields.io/badge/.ENV-ECD53F?style=flat&logo=dotenv&logoColor=black)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![Puppeteer](https://img.shields.io/badge/Puppeteer-40B5A4?style=flat&logo=puppeteer&logoColor=white)
![React Bootstrap](https://img.shields.io/badge/React%20Bootstrap-41E0FD?style=flat&logo=bootstrap&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=flat&logo=react&logoColor=black)
![Zod](https://img.shields.io/badge/Zod-3E67B1?style=flat&logo=zod&logoColor=white)
![ESLint](https://img.shields.io/badge/ESLint-4B32C3?style=flat&logo=eslint&logoColor=white)
![Axios](https://img.shields.io/badge/Axios-5A29E4?style=flat&logo=axios&logoColor=white)
![datefns](https://img.shields.io/badge/datefns-770C56?style=flat&logo=date-fns&logoColor=white)
![React Hook Form](https://img.shields.io/badge/React%20Hook%20Form-EC5990?style=flat&logo=reacthookform&logoColor=white)
![styledcomponents](https://img.shields.io/badge/styledcomponents-DB7093?style=flat&logo=styled-components&logoColor=white)






## Overview

This project was completed during a full-stack developer internship at **JOLOF SYSTEM**. It delivers an e-commerce application for discovering and ordering products, together with a supplier-data integration that imports catalog and inventory information from the **Logicom QuickConnect API** into **Sanity**.

The work addresses a practical commerce problem: maintaining product descriptions, prices, and stock manually across separate systems is slow and can result in stale information, stockouts, or overselling. The project brings the storefront, content management, and supplier import process into one connected solution.

The repository contains two cooperating applications:

- **`frontend/`** — the customer-facing Next.js storefront and its application routes.
- **`backend/`** — the Sanity Studio, content schemas, and Logicom import code.

The system uses a headless architecture: the storefront reads product and content data from Sanity, while the import service transfers supplier data into the same content store.

## Project Goals

- Provide a responsive online catalog for JOLOF SYSTEM products.
- Give customers a way to search and browse products, manage a cart, and submit orders.
- Let administrators maintain catalog and order content through Sanity Studio.
- Reduce manual catalog and stock maintenance by importing supplier data from Logicom.
- Keep the supplier-to-catalog transformation explicit and traceable.

## Features

### Customer storefront

- Home page with promotional content, brands, categories, and selected products.
- Product listing with category navigation, search, and sorting.
- Product detail pages with pricing, stock state, images, and specifications.
- Shopping cart with quantity controls and an order summary.
- Sign-in and checkout flow integrated with Clerk.
- Order confirmation, order history, and order detail views.

### Catalog and order operations

- Sanity content types for products, categories, brands, banners, and orders.
- Sanity Studio screens for content editing and order administration.
- Order creation in Sanity, with order status available for operational follow-up.
- Order confirmation email and PDF summary generation in the create-order route.

### Supplier synchronization

- Authenticated requests to Logicom QuickConnect.
- Import of supplier brands, categories, and products.
- Mapping of supplier product fields—such as SKU, description, price, variants, specifications, and stock—to Sanity documents.
- Create/update behavior keyed by product SKU, with import counts for created, updated, and skipped records.

## Architecture and Data Flow

The application separates presentation, content management, and supplier integration:

1. **Logicom QuickConnect** is the upstream source for supplier catalog and inventory data.
2. **The import service** authenticates, fetches supplier data, maps it to the project’s content model, and writes it through the Sanity client.
3. **Sanity Content Lake** stores products, categories, brands, banners, and orders.
4. **The Next.js storefront** queries Sanity and renders customer-facing pages.
5. **Customer actions** such as checkout create orders in Sanity; order processing can generate a PDF and send a confirmation email.

The report illustrates the project’s core supplier-import pipeline below.

<p align="center">
  <img src="./assets/report/etl-pipeline.png" alt="ETL pipeline from Logicom QuickConnect through transformation to Sanity" width="620">
  <br>
  <sub>ETL process: extract supplier data, transform it to the Sanity model, then load or update documents. Figure extracted from the internship report.</sub>
</p>

### Synchronization process

The import code runs brand, category, and product imports in sequence. For products, it retrieves existing Sanity documents, indexes them by SKU, requests supplier data, maps the response into the product schema, and creates or updates documents. The report presents scheduled pull as the intended operating strategy; the repository’s current npm scripts do not configure a scheduler.

<p align="center">
  <img src="./assets/report/sync-run.png" alt="Sample synchronization output showing created, updated, and skipped import counts" width="700">
  <br>
  <sub>Example import output reproduced from the report; these counts describe that run and are not a live service metric.</sub>
</p>

### Order flow

The report describes an MVC-inspired request flow for order creation: the storefront initiates the request, a Next.js route handles it, and the Sanity client persists the order. The route also contains the PDF and email confirmation steps.

<p align="center">
  <img src="./assets/report/order-flow.png" alt="MVC-inspired order creation flow across the storefront, API route, model, and data store" width="620">
  <br>
  <sub>Order creation flow documented in the internship report.</sub>
</p>



## Internship Methodology

The report follows a staged, evidence-driven development process. It does not identify a named framework such as Scrum; the steps below summarize the approach documented during the internship.

1. **Understand the business context.** Review JOLOF SYSTEM’s sales and catalog needs, identify problems with manual product and inventory updates, and define the customer, administrator, and logistics-manager roles.
2. **Translate needs into requirements.** Record customer journeys and administration tasks, then distinguish functional requirements (catalog, cart, orders, synchronization) from quality goals such as responsiveness, reliability, and scalability.
3. **Research and select the architecture.** Compare monolithic and headless approaches and select Next.js with Sanity so the presentation layer and content store can evolve independently.
4. **Validate the supplier contract before building the importer.** Use Postman to exercise Logicom endpoints, inspect authentication requirements and response payloads, and confirm the fields and data shapes required by the application.
5. **Design the data and integration flow.** Define the Sanity document model, establish the mapping from supplier fields to application fields, and plan the extract-transform-load sequence.
6. **Implement in connected modules.** Build the Logicom API client and import modules, then connect Sanity-backed product data to the storefront, cart, checkout, and order workflows.
7. **Check behavior with observable evidence.** Review API responses in Postman, inspect import success and record counts in the command output, and verify the customer and admin flows through the application screens captured in the report.
8. **Document outcomes and next steps.** Summarize the architecture, implementation, challenges, and possible extensions in the final-year internship report.

The use-case diagram below captures the main customer and administrator responsibilities that informed the requirements.

<p align="center">
  <img src="./assets/report/use-case.png" alt="Customer and administrator use cases for the e-commerce platform" width="760">
  <br>
  <sub>Use-case diagram from the internship report.</sub>
</p>

## Data Model

Sanity is the primary content store for the project. The report identifies the following core document types:

| Document | Purpose |
|---|---|
| **Product** | Supplier-linked item details, including SKU, name, price, inventory, variants, specifications, and images. |
| **Category** | Product grouping, including hierarchical parent/child categories. |
| **Brand** | Manufacturer or brand information associated with products. |
| **Order** | Customer and delivery details, ordered items, totals, and order status. |
| **Banner** | Promotional content displayed by the storefront. |

The model diagram below is extracted from the report and shows the relationships between the principal documents and nested product/order data.

<p align="center">
  <img src="./assets/report/sanity-data-model.png" alt="Sanity document model for products, orders, categories, brands, and related data" width="850">
  <br>
  <sub>Core Sanity schema model from the internship report.</sub>
</p>

## Application Screenshots

The following screenshots are selected from the implementation chapter of the internship report.

### Home page

<p align="center">
  <img src="./assets/report/storefront-home.png" alt="JOLOF SYSTEM e-commerce home page" width="760">
  <br>
  <sub>Home page with the promotional carousel, brands, categories, and product cards.</sub>
</p>

### Product catalog

<p align="center">
  <img src="./assets/report/product-catalog.png" alt="Product listing with category navigation, stock indicators, and sorting" width="760">
  <br>
  <sub>Product listing with category navigation, stock indicators, and sorting.</sub>
</p>

### Product details

<p align="center">
  <img src="./assets/report/product-detail.png" alt="Product detail page with availability, price, images, and specifications" width="760">
  <br>
  <sub>Product detail page with availability, pricing, images, and specifications.</sub>
</p>

### Shopping cart

<p align="center">
  <img src="./assets/report/shopping-cart.png" alt="Shopping cart with item quantity controls and order summary" width="760">
  <br>
  <sub>Shopping cart with item controls and an order summary before checkout.</sub>
</p>

### Checkout

<p align="center">
  <img src="./frontend/screenshoots/checkout.png" alt="Checkout form with delivery details and order summary" width="760">
  <br>
  <sub>Checkout form for entering delivery information and reviewing the order before submission.</sub>
</p>

### Order details and invoice

<p align="center">
  <img src="./frontend/screenshoots/order.png" alt="Customer order details page with delivery information and order summary" width="760">
  <br>
  <sub>Order details page showing the order status, items, delivery information, and totals.</sub>
</p>

<p align="center">
  <img src="./frontend/screenshoots/invoice.png" alt="Generated customer invoice attached to the order confirmation email" width="520">
  <br>
  <sub>PDF invoice generated for the order and included with the confirmation email.</sub>
</p>

### Sanity administration

<p align="center">
  <img src="./assets/report/admin-dashboard.png" alt="Sanity Studio dashboard for order and catalog administration" width="760">
  <br>
  <sub>Sanity Studio dashboard for catalog and order administration.</sub>
</p>

## Technology Stack

### Frontend

- **Next.js 15** and **React 19** — application framework and UI.
- **TypeScript** — typed application and content models.
- **Tailwind CSS** — responsive styling.
- **Redux Toolkit** — shared client state, including the shopping cart.
- **Clerk** — customer authentication.
- **Sanity client / GROQ** — content queries and writes.

### Backend and content

- **Sanity Studio 4** — content management interface and schema.
- **Sanity Content Lake** — product, category, brand, banner, and order documents.
- **Node.js and TypeScript** — supplier API client and import modules.
- **Axios and Node crypto** — HTTP requests and Logicom request authentication.
- **Nodemailer and Puppeteer** — order confirmation email and PDF generation.

### Development and delivery tools

- **Postman** — supplier API exploration and request/response validation.
- **Git and GitHub** — version control and project hosting.
- **Vercel** — identified in the report as a deployment option for the Next.js application.

![Home Page](./frontend/screenshoots/TechStack.png)


## Validation, Scope, and Future Work

The report documents manual API validation with Postman, import output showing created/updated/skipped records, and screenshots of the storefront and administration experience. The package manifests do not provide a project-wide automated test script.

The implementation should be treated as an internship project, not as a production deployment guarantee. In particular:

- The supplier synchronization entry point is present, but a recurring scheduler is not configured by the current npm scripts.
- External services (Sanity, Logicom, Clerk, and email) require valid project credentials and configuration to exercise their related flows.
- The report proposes further work such as supporting additional suppliers, improving caching, adding richer synchronization controls, and extending search and discovery features.
