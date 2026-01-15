# Road Trip Planner RPA

## Team Information
**Team Name:** Hobby Hunters  
**Team Members:**
- Lucian Popescu
- Nandor Kadar

**Course:** Information Engineering, Group 1331

---

## Project Description

This RPA bot automates the process of finding and recommending activities for travelers. It searches GetYourGuide based on user input (hobby and destination city), filters results by rating, and sends personalized Excel reports via email.

### What the Bot Does

- Searches GetYourGuide for activities based on hobby and city
- Scrapes activity data (name, rating, price, link)
- Filters activities by minimum rating requirement
- Validates that results match the target city
- Generates Excel reports with filtered activities
- Sends reports to clients via email
- Handles errors and retries failed transactions
- Takes screenshots when errors occur

---

## Architecture

The project follows the **Transactional Business Process** pattern with a **State Machine** workflow:

### Main Workflow States

1. **Initialization**
   - Kill existing Chrome processes
   - Load configuration from `Data/Config.xlsx`
   - Initialize GetYourGuide browser session

2. **Get Transaction Data**
   - Read input data from `Data/Input/InputData.xlsx`
   - Extract transaction details (City, Hobby, MinRating, ClientEmail)
   - Set transaction ID and item

3. **Process Transaction**
   - Navigate to GetYourGuide
   - Search for "[Hobby] in [TargetCity]"
   - Extract activity data (name, rating, price, link)
   - Filter by minimum rating
   - Validate city name in results
   - Generate Excel report
   - Send email with report attachment

4. **Set Transaction Status**
   - Update transaction status (Success/Business Exception/System Exception)
   - Log results with custom fields
   - Handle retries for system exceptions

5. **End Process**
   - Close all applications
   - Clean up resources

---

## Project Structure

```
TripPlanner_Final/
│
├── Main.xaml                          # Main workflow (State Machine)
├── Process.xaml                       # Core processing logic
│
├── Framework/                         # REFramework components
│   ├── InitAllSettings.xaml          # Configuration loader
│   ├── InitAllApplications.xaml      # Browser initialization
│   ├── GetTransactionData.xaml       # Transaction data retrieval
│   ├── SetTransactionStatus.xaml     # Status management
│   ├── RetryCurrentTransaction.xaml  # Retry logic
│   ├── CloseAllApplications.xaml     # Graceful shutdown
│   ├── KillAllProcesses.xaml         # Force close processes
│   └── TakeScreenshot.xaml           # Error screenshots
│
├── Data/
│   ├── Input/
│   │   └── InputData.xlsx            # Transaction input data
│   ├── Output/                        # Generated reports
│   ├── Temp/                          # Temporary files
│   └── Config.xlsx                    # Configuration settings
│
└── Tests/                             # Test cases
```

---

## Technical Highlights

### Web Scraping Features
- ✅ Modern UiPath UI Automation (UIAutomationNext)
- ✅ Structured data extraction from search results
- ✅ Dynamic selector generation for flexible element targeting
- ✅ Visual verification with element highlighting

### Data Processing
- ✅ DataTable manipulation for filtering
- ✅ Regular expression-based text cleaning
- ✅ Numeric validation for ratings
- ✅ Case-insensitive city name matching

### Email Integration
- ✅ SMTP protocol with SSL/TLS
- ✅ Attachment handling
- ✅ HTML/Plain text body support
- ✅ Gmail app password authentication

---

## Dependencies

- **UiPath.Excel.Activities:** [2.20.2]
- **UiPath.Mail.Activities:** [1.20.3]
- **UiPath.System.Activities:** [23.10.3]
- **UiPath.Testing.Activities:** [23.10.1]
- **UiPath.UIAutomation.Activities:** [23.10.8]
