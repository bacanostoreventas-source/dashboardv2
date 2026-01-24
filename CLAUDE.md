# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Dashboard Dropshipping PRO - A single-page analytics dashboard for Bacano Store's dropshipping operations. The entire application is contained in a single `index.html` file (~270KB) with embedded CSS and JavaScript.

**Live URL:** https://bacanostoreventas-source.github.io/dashboardv2/

## Architecture

### Single-File Structure
The application follows a monolithic single-file architecture:
- **Lines 1-1777**: CSS styles (variables, components, responsive design, animations)
- **Lines 1778-5800+**: JavaScript application logic

### Key Global Variables
- `dropiData` - Array holding all order data from uploaded Excel files
- `filteredData` - Currently filtered subset of orders
- `currentPage` - Pagination state for tables
- `itemsPerPage` - Fixed at 20 records per page
- `STORAGE_KEY = 'bacano_dropi_data'` - LocalStorage key for data persistence
- `AUTH_STORAGE_KEY = 'bacano_auth_user'` - LocalStorage key for authentication

### External Dependencies (CDN)
- **xlsx.js** - Excel file parsing (SheetJS)
- **Chart.js** - Data visualization charts
- **html2pdf.js** - PDF export functionality
- **Google Identity Services** - OAuth authentication

### Core Function Groups

**Authentication:**
- `checkAuth()`, `handleGoogleLogin()`, `handleCredentialResponse()`, `loginUser()`, `handleLogout()`

**Data Management:**
- `loadFile()`, `loadDroppedFile()` - Excel file import
- `processDropiData()` - Transforms raw Excel data into order objects
- `calculateStats()` - Aggregates orders into statistics by status, city, carrier, product
- `saveToLocalStorage()`, `loadFromLocalStorage()`, `clearDropiData()` - Persistence

**UI Rendering (Tab-based):**
- `renderContent()` - Main dispatcher based on active tab
- `renderResumen()` - Overview KPIs and charts
- `renderPedidos()` - Orders table with pagination
- `renderEstatus()` - Status breakdown
- `renderProductos()` - Product performance
- `renderCiudades()` - City analytics
- `renderTransportadoras()` - Carrier metrics
- `renderRentabilidad()` - Profitability analysis
- `renderNovedades()` - Issues/incidents tracking
- `renderBilletera()` - Wallet/financial dashboard

**Filtering & Search:**
- `applyFilters()`, `clearFilters()`, `setDatePeriod()`
- `searchTable()`, `searchByField()`, `filterTableRows()`

**Export:**
- `exportToExcelAdvanced()` - Multi-sheet Excel export
- `exportPDF()`, `showPDFPreview()`, `downloadPDF()` - PDF generation

### Data Flow
1. User uploads Excel file (drag-drop or file picker)
2. `loadDroppedFile()` / `loadFile()` reads file via FileReader
3. `processDropiData()` normalizes data, calculates derived fields (ganancia, margen)
4. Data stored in `dropiData` and persisted to LocalStorage
5. `calculateStats()` generates aggregations on demand
6. Tab renderers display data using template literals for HTML generation

### Order Object Schema
```javascript
{
  id, guia, estatus, fecha, ciudad, departamento,
  cliente, telefono, direccion, producto, cantidad,
  total, costo, ganancia, margen, transportadora,
  observaciones, novedad, novedadTexto
}
```

### CSS Theming
Uses CSS custom properties for dark/light theme support:
- Dark theme (default): `--bg-primary: #0f0f14`
- Light theme: `[data-theme="light"]` selector
- Color palette: emerald, blue, amber, red, violet, pink with `-bg` variants

## Development

This is a static site with no build process. To develop:
1. Edit `index.html` directly
2. Open in browser or use a local server
3. Changes deploy automatically via GitHub Pages on push to `main`

## Google OAuth Configuration
Client ID is hardcoded at line 1778. The OAuth consent screen must allow the GitHub Pages domain.
