# HelloID JavaScript Playground

A specialized development tool for creating and debugging JavaScript mapping scripts used in HelloID PowerShell v2 target systems. This playground provides an interactive environment for developing complex target mappings that transform employee/contract data during identity provisioning workflows.

## About HelloID Target Mappings

HelloID uses JavaScript-based transformation scripts (ECMAScript 5.1) to process user attributes during identity provisioning. These mapping scripts:

- Transform and format user attributes dynamically
- Generate unique usernames based on employee information  
- Access person data through the `Person` object structure
- Handle data validation, cleanup, and formatting
- Support complex logic for account creation across different target systems

This playground replicates the HelloID mapping script environment, allowing you to develop and test your transformation logic before deploying to production.

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

## HelloID Mapping Script Examples

### Basic Username Generation
```javascript
// Generate username from person data
var username = '';
if (Person.Name.GivenName && Person.Name.FamilyName) {
    username = Person.Name.GivenName.toLowerCase() + '.' + Person.Name.FamilyName.toLowerCase();
    // Remove diacritical marks and special characters
    username = username.replace(/[^a-z0-9.]/g, '');
    // Truncate to 20 characters
    if (username.length > 20) {
        username = username.substring(0, 20);
    }
}
return username;
```

### Complex Data Transformation
```javascript
// Build user attributes object for target system
var userAttributes = {
    DisplayName: Person.DisplayName,
    Department: Person.PrimaryContract.Department.DisplayName,
    Title: Person.PrimaryContract.Title.Name,
    Manager: Person.PrimaryManager.DisplayName || 'No Manager',
    Location: Person.Location.Name,
    Email: Person.Contact.Business.Email || Person.Contact.Personal.Email
};

// Handle missing or null values
Object.keys(userAttributes).forEach(function(key) {
    if (!userAttributes[key]) {
        userAttributes[key] = '';
    }
});

return userAttributes;
```

### Contract Validation
```javascript
// Check contract validity and generate account status
var today = new Date();
var contractEnd = new Date(Person.PrimaryContract.EndDate);
var daysUntilExpiry = Math.ceil((contractEnd - today) / (1000 * 60 * 60 * 24));

var accountStatus = {
    isActive: !Person.PrimaryContract.Context.InConditions && daysUntilExpiry > 0,
    expirationDate: Person.PrimaryContract.EndDate,
    daysUntilExpiry: daysUntilExpiry,
    contractType: Person.PrimaryContract.Type.Description || 'Unknown'
};

console.log('Account Status:', JSON.stringify(accountStatus, null, 2));
return accountStatus;
```

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

- **HelloID Mapping Script Development** - Create and test complex target mapping scripts for identity provisioning
- **Username Generation Logic** - Develop algorithms for generating unique usernames from person data
- **Data Transformation Testing** - Validate scripts handle missing data, special characters, and formatting requirements
- **Debugging Complex Mappings** - Interactive exploration of Person object properties and nested data structures
- **Script Optimization** - Test performance and edge cases before production deployment
- **Learning HelloID Scripting** - Understand Person object structure and available properties
- **Prototype New Mappings** - Experiment with transformation logic for new target systems

## HelloID Integration

When your mapping scripts are ready:

1. **Copy Script Logic** - Transfer your tested JavaScript code to HelloID target system mappings
2. **Verify Person Properties** - Ensure all Person object properties used exist in your HelloID environment
3. **Test with Real Data** - Use HelloID's preview functionality to validate with actual employee data
4. **Deploy Gradually** - Test mappings on a small user subset before full deployment

## Best Practices for HelloID Mapping Scripts

- **Handle Null Values** - Always check for undefined or null Person properties
- **Validate Data Types** - Ensure expected data types before string operations
- **Keep Scripts Efficient** - Avoid complex loops; mapping scripts run for every user
- **Use Consistent Naming** - Follow your organization's username/attribute conventions
- **Test Edge Cases** - Verify behavior with missing names, special characters, long strings
- **Document Logic** - Comment complex transformation rules for future maintenance

## Contributing

This tool is designed for HelloID administrators and developers. Contributions welcome:
- Add new sample HR data scenarios
- Enhance IntelliSense for HelloID-specific properties  
- Add utility functions for common mapping operations
- Improve debugging capabilities
- Add more HelloID mapping script examples

## License

This project is for educational and development purposes.
