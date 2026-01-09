# Smart Job Hunt Assistant - UiPath REFramework Project

## Project Overview (Layered Architecture)
This project is designed using the **Robotic Enterprise Framework (REFramework)** to ensure scalability, robustness, and proper exception handling.

### Structure
1.  **Initialization (Init)**:
    - Reads the `Config.xlsx`.
    - Opens the browser (Indeed/LinkedIn).
    - Invokes `Workflows\ScrapeJobData.xaml` to scrape job postings into a global `dt_TransactionData` (DataTable).
2.  **Get Transaction Data**:
    - Retrives the next row from `dt_TransactionData` based on the `TransactionNumber`.
    - Assigns it to `TransactionItem`.
3.  **Process Transaction**:
    - Invokes `Workflows\Process.xaml`.
    - Checks Business Rules:
        - **Blacklisted Company**: Throws `BusinessRuleException`.
        - **Salary Too Low**: Throws `BusinessRuleException`.
    - If valid, adds the job to `dt_FilteredJobs`.
4.  **End Process**:
    - Invokes `Workflows\SendEmailReport.xaml` to send the final report of `dt_FilteredJobs` to the user.
    - Closes applications.

## Workflows Included
- **`Workflows\ScrapeJobData.xaml`**:
    - Opens browser to job site.
    - Uses 'Type Into' with dynamic inputs.
    - Extracts data using 'Extract Structured Data'.
- **`Workflows\Process.xaml`**:
    - Implements the Business Logic.
    - Handles exceptions: "Company Blacklisted" and "Salary below expectation".
- **`Workflows\SendEmailReport.xaml`**:
    - Generates an HTML table from the filtered jobs.
    - Sends an email using SMTP.

## Configuration (Config.xlsx)
Ensure your `Config.xlsx` (or Dictionary) has the following keys:
- `JobRole`: e.g., "Junior Developer"
- `Location`: e.g., "New York"
- `BlacklistedCompanies`: Comma-separated list e.g., "Bad Corp,Evildoers Inc"
- `MinSalary`: Numeric e.g., 50000
- `SenderEmail`: Your email address
- `SenderPassword`: Application password
- `MailServer`: e.g., "smtp.gmail.com"
- `MailPort`: e.g., 587
- `ReceiverEmail`: The user's email

## Usage
1.  Import these workflows into your REFramework "Workflows" folder.
2.  Wire `ScrapeJobData` in the `Initialization` state.
3.  Wire `Process` in the `Process Transaction` state.
4.  Wire `SendEmailReport` in the `End Process` state.
# Business
