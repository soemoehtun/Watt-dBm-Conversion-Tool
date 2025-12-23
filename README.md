# Watt-dBm Conversion Tool

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/license/mit/)
[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Glossary/HTML5)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

A professional web-based utility for RF engineering power unit conversions between **Watts (W)** and **dBm (decibels relative to 1 milliwatt)**. This tool processes CSV data files, allowing engineers to efficiently convert large datasets with an intuitive interface and comprehensive user guide.

---

## 📌 Table of Contents

- [Features](#-features)
- [Live Demo](#-live-demo)
- [Installation](#-installation)
- [Usage Guide](#-usage-guide)
- [Technical Implementation](#-technical-implementation)
- [Conversion Formulas](#-conversion-formulas)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🎯 Features

- **Drag & Drop CSV Upload**  
  Supports CSV files up to 5MB with instant parsing and validation.

- **Column Selection**  
  Choose which columns contain sector references and power values.

- **Dual Conversion Modes**  
  - `Watt → dBm`  
  - `dBm → Watt`

- **Instant Preview**  
  Displays first 100 rows of results in a responsive table (full data available via export).

- **CSV Export**  
  Download complete conversion results with timestamps.

- **Comprehensive User Guide**  
  Built-in tutorial covering:
  - Unit definitions
  - Conversion formulas
  - Step-by-step instructions
  - Use cases
  - Troubleshooting

- **Responsive Design**  
  Works on mobile, tablet, and desktop devices.

- **Modern UI**  
  Clean, professional interface with real-time feedback and error handling.

---

## 🌐 Live Demo

Access the tool directly from GitHub Pages:

[![Deploy with GitHub Pages](https://img.shields.io/github/deployments/soemoehtun/watt-dbm-converter/github-pages?label=Live%20Demo)](https://soemoehtun.github.io/watt-dbm-converter/)

> **Note**: The tool runs entirely client-side – your data never leaves your browser!

---

## ⬇️ Installation

Since this is a **static web application**, no server or build process is required.

### Method 1: Direct Use from GitHub
1. Visit the [Live Demo](https://soemoehtun.github.io/watt-dbm-converter/)
2. Start using the tool immediately!

### Method 2: Local Setup
1. Clone the repository:
   ```bash
   git clone https://github.com/soemoehtun/watt-dbm-converter.git
   cd watt-dbm-converter
   ```

2. Open `index.html` in any modern web browser:
   - Double-click the file
   - Or run with a local server (recommended for older browsers):
     ```bash
     # Using Python (comes pre-installed on most systems)
     python -m http.server
     # Then open http://localhost:8000
     ```

---

## 📚 Usage Guide

### Step 1: Load Your CSV Data
- Click the drop zone **or** drag & drop your CSV file.
- Supported formats:
  - Comma-separated values (`.csv`)
  - First row must contain column headers
  - Maximum file size: **5MB**

### Step 2: Configure Columns & Operation
1. **Sector Column**: Select the column used for identification (e.g., `SectorID`, `SiteName`).
2. **KPI Column**: Choose the column containing power values (e.g., `TxPower`, `SignalStrength`).
3. **Conversion Type**:  
   - `Watt → dBm`: Convert power in watts to dBm  
   - `dBm → Watt`: Convert dBm values to watts

### Step 3: Generate Results
- Click **Calculate & Generate Result**  
- Preview results in the table (first 100 rows shown).

### Step 4: Export Data
- Click **Export CSV** to download full results as a CSV file.  
- Filename format: `conversion_results_<timestamp>.csv`

### Example CSV Format
```csv
SectorID,TxPower
Sector01,5
Sector02,10
Sector03,20
```

---

## ⚙️ Technical Implementation

### Core Technologies
| Technology | Version | Purpose |
|------------|---------|---------|
| **HTML5**  | 5.0     | Semantic structure and form handling |
| **CSS3**    | 3.0     | Styling and animations |
| **JavaScript** | ES6+   | Core logic, file parsing, calculations |
| **Tailwind CSS** | v4 (via CDN) | Utility-first styling |
| **Font Awesome** | v6.4.0 | Icons for UI elements |

### Key Implementation Details

#### File Parsing
```javascript
// Simplified CSV parsing logic
function parseCSV(text) {
  const lines = text.split(/\r?\n/).filter(line => line.trim() !== "");
  const headers = lines[0].split(',').map(h => h.trim());
  
  return lines.slice(1).map(line => {
    const values = line.split(',');
    let obj = {};
    headers.forEach((h, i) => {
      obj[h] = values[i] ? values[i].trim() : "";
    });
    return obj;
  });
}
```

#### Conversion Logic
```javascript
// Watt to dBm: 10 * log10(W * 1000)
// dBm to Watt: 10^(dBm/10) / 1000

if (mode === 'w2dbm') {
  result = val > 0 ? (10 * Math.log10(val * 1000)) : 0;
} else {
  result = Math.pow(10, val / 10) / 1000;
}
```

#### Responsive Design
Uses Tailwind CSS breakpoints for mobile-first responsiveness:
```html
<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
  <!-- Two-column layout on medium screens and up -->
</div>
```

---

## 📐 Conversion Formulas

### 1. Watt → dBm
\[
P(\text{dBm}) = 10 \times \log_{10}(P(\text{W}) \times 1000)
\]

**Example**:  
10W → 10 × log₁₀(10 × 1000) = 10 × log₁₀(10,000) = **40 dBm**

### 2. dBm → Watt
\[
P(\text{W}) = \frac{10^{(P(\text{dBm}) / 10)}}{1000}
\]

**Example**:  
30 dBm → 10^(30/10) / 1000 = 1000 / 1000 = **1 W**

### Quick Reference Table

| Watts (W) | dBm  | Watts (W) | dBm  |
|-----------|------|-----------|------|
| 0.001     | 0    | 1         | 30   |
| 0.01      | 10   | 2         | 33   |
| 0.1       | 20   | 10        | 40   |
| 0.5       | 27   | 100       | 50   |

> 📌 **Reference**: 1 mW = 0 dBm, 1 W = 30 dBm

---

## 🛠️ Troubleshooting

### Common Issues & Solutions

| Issue                          | Solution                                                                 |
|--------------------------------|--------------------------------------------------------------------------|
| **File not loading**           | Ensure CSV is properly formatted (comma-delimited, headers in first row) |
| **NaN/Invalid values in results** | Check for non-numeric data in KPI column. Remove text/symbols.          |
| **Columns missing in dropdown**| Verify headers exist in first row of CSV. No merged cells allowed.      |
| **Nothing happens on calculate**| Ensure both Sector and KPI columns are selected.                         |
| **Large files slow performance**| Preview shows 100 rows. Download full CSV for complete data.            |

> 💡 **Tip**: Use the built-in **Tools Guide Line** button (top-right of interface) for detailed help!

---

## 🤝 Contributing

We welcome contributions to improve this tool! Follow these steps:

1. **Fork the repository**  
   Click the *Fork* button at the top right of this GitHub repository.

2. **Create a feature branch**  
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes**  
   - Add new features **or**
   - Fix bugs reported in [Issues](https://github.com/soemoehtun/watt-dbm-converter/issues)

4. **Test thoroughly**  
   Ensure all existing functionality remains intact.

5. **Commit with conventional messages**  
   ```bash
   git commit -m "feat: add dark mode support"
   ```

6. **Push to your fork**  
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Open a Pull Request**  
   Provide a clear description of changes.

### Development Guidelines
- **HTML/CSS**: Use Tailwind classes where possible. Custom CSS only for specific needs.
- **JavaScript**: ES6+ syntax, no external libraries.
- **Testing**: Verify on Chrome, Firefox, Safari, and Edge.

---

## 📄 License

This project is licensed under the **MIT License** – see the [LICENSE](LICENSE) file for details.

```text
MIT License

Copyright (c) 2024 Soe Moe Tun

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software...
```

---

## 💬 Contact

Developed by **Soe Moe Tun**  
- 📧 Email: [soemoehtun@example.com](mailto:soemoehtun@example.com)  
- 💼 Portfolio: [soemoehtun.github.io/profolio](https://soemoehtun.github.io/profolio/)  
- 🐙 GitHub: [github.com/soemoehtun](https://github.com/soemoehtun)

---

> **💡 Pro Tip**: Bookmark this tool if you work with RF power measurements regularly – it’s designed to save you time during network planning, optimization, and troubleshooting!
