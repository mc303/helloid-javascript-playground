# HelloID JavaScript Playground

A web-based JavaScript playground for exploring and scripting against employee/contract data structures. This tool provides an interactive environment for HR data analysis, script development, and learning how to work with complex employee data.

## Features

- **Monaco Editor Integration** - Full-featured code editor with syntax highlighting and custom IntelliSense
- **Smart Code Completion** - Custom autocomplete for navigating Person object properties with circular reference prevention
- **Real HR Data** - Work with realistic employee, contract, and organizational data structures
- **Interactive Person Selection** - Searchable dropdown (400px wide) to select specific employees
- **Live Code Execution** - Execute button integrated into JavaScript Editor header for immediate code execution
- **JSON Data Viewer** - Modal for inspecting raw employee data
- **Output Management** - Dedicated execution output panel with clear functionality
- **Modern UI Design** - Clean, compact interface with glass morphism effects and gradient buttons
- **Responsive Layout** - Optimized header layout with centered controls and proper height management

## Getting Started

1. **Open the Application**
   ```bash
   # Simply open index.html in your web browser
   open index.html
   ```

2. **Load Data**
   - The application automatically loads sample data from `vault.json`
   - Alternatively, use "Choose File" to select a custom vault.json file, then click "Load"

3. **Select a Person**
   - Use the searchable dropdown (400px wide) to find and select an employee
   - The first person is automatically selected when data loads
   - Search filters employees by display name

4. **Write JavaScript Code**
   - Use the Monaco editor to write JavaScript code
   - Access the selected person's data via the `Person` object
   - Leverage custom IntelliSense for property discovery (circular references filtered)

5. **Execute and View Results**
   - Click "Execute" (located in JavaScript Editor header) to run your code
   - View output in the dedicated execution output panel
   - Use "Clear" button to clear output results
   - Click "JSON" to inspect the raw person data in a modal

## Data Structure

The playground works with HR data containing:

### Person Object
```javascript
Person: {
  PersonId: "uuid",
  DisplayName: "Full Name (ID)",
  ExternalId: "employee-id",
  UserName: "username",
  Contracts: [...],
  PrimaryContract: {...},
  Location: {...},
  Details: {...},
  Name: {...},
  Contact: {...},
  // ... and more
}
```

### Key Properties
- **Contracts** - Array of employment contracts
- **PrimaryContract** - Main contract details
- **Location** - Work location information
- **Details** - Personal details (gender, birth date, etc.)
- **Name** - Name components (given, family, prefixes)
- **Contact** - Personal and business contact information

## Example Scripts

### Basic Information
```javascript
// Get employee's full name and ID
console.log(Person.DisplayName);
console.log(Person.ExternalId);
```

### Contract Analysis
```javascript
// Check contract end date
const endDate = new Date(Person.PrimaryContract.EndDate);
const today = new Date();
const daysUntilExpiry = Math.ceil((endDate - today) / (1000 * 60 * 60 * 24));
console.log(`Contract expires in ${daysUntilExpiry} days`);
```

### Department Information
```javascript
// Get department and manager details
console.log(`Department: ${Person.PrimaryContract.Department.DisplayName}`);
console.log(`Manager: ${Person.PrimaryContract.Department.Manager.ExternalId}`);
```

### Contact Information
```javascript
// Extract contact details
const personal = Person.Contact.Personal;
const business = Person.Contact.Business;
console.log(`Personal Email: ${personal.Email}`);
console.log(`Business Phone: ${business.Phone.Mobile}`);
```

## Technical Details

### Dependencies
- **Monaco Editor v0.33.0** - Code editor with custom completion providers (loaded via CDN)
- **TailwindCSS** - Styling framework with custom configuration (loaded via CDN)
- **Google Fonts** - Inter and JetBrains Mono fonts for modern typography

### Browser Support
- Modern browsers with ES6+ support
- No server required - runs entirely in the browser
- Optimized for full-screen usage with proper viewport height management

### Security Note
⚠️ **Warning**: This playground executes arbitrary JavaScript using `new Function()`. Only use with trusted code and data.

## File Structure

```
helloid-javascript-test/
├── index.html          # Main application file
├── vault.json          # Sample HR data
└── README.md           # This file
```

## Use Cases

- **HR Data Analysis** - Explore employee data and generate reports
- **Script Development** - Prototype JavaScript for HR systems
- **Learning** - Understand complex nested data structures
- **Data Validation** - Test data integrity and completeness
- **Reporting** - Create custom employee reports and summaries

## Contributing

This is a development tool for exploring HR data structures. Feel free to:
- Add new sample data
- Enhance the IntelliSense capabilities
- Add utility functions for common operations
- Improve the user interface

## License

This project is for educational and development purposes.
