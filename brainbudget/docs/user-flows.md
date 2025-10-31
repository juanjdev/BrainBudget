# User Flow Diagrams

## 1. User Registration & Onboarding Flow

```mermaid
flowchart TD
    Start([User Visits App]) --> Landing[Landing Page]
    Landing --> SignUp{Choose Sign Up Method}
    
    SignUp -->|Email| EmailForm[Enter Email & Password]
    SignUp -->|Google| GoogleOAuth[Google OAuth]
    SignUp -->|GitHub| GitHubOAuth[GitHub OAuth]
    
    EmailForm --> Validate{Valid Input?}
    Validate -->|No| EmailForm
    Validate -->|Yes| CreateAccount[Create Account]
    
    GoogleOAuth --> CreateAccount
    GitHubOAuth --> CreateAccount
    
    CreateAccount --> SendVerification[Send Verification Email]
    SendVerification --> VerifyPrompt[Show Verification Prompt]
    
    VerifyPrompt --> CheckEmail[User Checks Email]
    CheckEmail --> ClickLink[Click Verification Link]
    ClickLink --> VerifyEmail[Verify Email Address]
    
    VerifyEmail --> Onboarding[Start Onboarding]
    
    Onboarding --> Step1[Step 1: Set Preferences<br/>- Currency<br/>- Timezone<br/>- Language]
    Step1 --> Step2[Step 2: Default Categories<br/>- Select/Customize Categories]
    Step2 --> Step3[Step 3: Setup Budget<br/>- Optional: Set Monthly Budget]
    Step3 --> Step4[Step 4: Payment Methods<br/>- Add Credit Cards/Accounts]
    Step4 --> Complete[Onboarding Complete]
    
    Complete --> Dashboard[Redirect to Dashboard]
    
    style Start fill:#e1f5e1
    style Dashboard fill:#e1f5e1
    style CreateAccount fill:#fff3cd
    style VerifyEmail fill:#fff3cd
```

## 2. Add Expense Flow (Manual Entry)

```mermaid
flowchart TD
    Start([User on Dashboard]) --> Click[Click "Add Expense"]
    Click --> Modal[Open Expense Modal]
    
    Modal --> Amount[Enter Amount]
    Amount --> Category[Select Category]
    Category --> AICheck{AI Suggestion Available?}
    
    AICheck -->|Yes| ShowSuggestion[Show AI Category Suggestion<br/>with Confidence Score]
    AICheck -->|No| Date
    
    ShowSuggestion --> AcceptAI{Accept Suggestion?}
    AcceptAI -->|Yes| Date
    AcceptAI -->|No| ManualCategory[Manually Select Category]
    ManualCategory --> Date
    
    Date[Select Date] --> Merchant[Enter Merchant Name]
    Merchant --> Description[Add Description Optional]
    Description --> Payment[Select Payment Method]
    Payment --> Tags[Add Tags Optional]
    Tags --> Receipt{Upload Receipt?}
    
    Receipt -->|Yes| UploadFile[Upload Receipt Image]
    Receipt -->|No| Split
    
    UploadFile --> ProcessReceipt[Process Receipt<br/>Background Job]
    UploadFile --> Split
    
    Split{Split Expense?}
    Split -->|Yes| SplitConfig[Configure Split<br/>- Select Users<br/>- Enter Amounts/Percentages]
    Split -->|No| Recurring
    
    SplitConfig --> Recurring{Recurring?}
    Recurring -->|Yes| RecurringConfig[Configure Recurrence<br/>- Frequency<br/>- End Date]
    Recurring -->|No| Review
    RecurringConfig --> Review
    
    Review[Review Expense Details] --> Submit[Submit Expense]
    Submit --> Save[Save to Database]
    Save --> Notify[Send Notifications]
    
    Notify --> UpdateBudget[Update Budget Tracking]
    UpdateBudget --> CheckBudget{Budget Threshold Exceeded?}
    
    CheckBudget -->|Yes| BudgetAlert[Send Budget Alert]
    CheckBudget -->|No| Success
    BudgetAlert --> Success
    
    Success[Show Success Message] --> Dashboard[Return to Dashboard]
    
    ProcessReceipt -.Background Process.-> ExtractData[Extract Data from Receipt]
    ExtractData -.-> UpdateExpense[Update Expense with Extracted Data]
    UpdateExpense -.-> NotifyUser[Notify User: Receipt Processed]
    
    style Start fill:#e1f5e1
    style Dashboard fill:#e1f5e1
    style Save fill:#fff3cd
    style BudgetAlert fill:#f8d7da
```

## 3. Receipt Scanning Flow

```mermaid
flowchart TD
    Start([User Wants to Add Receipt]) --> Method{Entry Method}
    
    Method -->|Camera| OpenCamera[Open Camera]
    Method -->|Upload| FileSelect[Select File]
    Method -->|Drag & Drop| DragDrop[Drag & Drop Area]
    
    OpenCamera --> Capture[Capture Photo]
    FileSelect --> Upload
    DragDrop --> Upload[Upload Image]
    Capture --> Upload
    
    Upload --> Validate{Valid Image?}
    Validate -->|No| Error[Show Error Message]
    Error --> Method
    Validate -->|Yes| Preview[Show Image Preview]
    
    Preview --> Confirm{Confirm Upload?}
    Confirm -->|No| Method
    Confirm -->|Yes| SendToStorage[Upload to S3]
    
    SendToStorage --> CreateRecord[Create Receipt Record]
    CreateRecord --> QueueJob[Queue OCR Job]
    QueueJob --> ShowLoading[Show Processing Status]
    
    ShowLoading --> OCRProcess[OCR Service Processes Image]
    OCRProcess --> ExtractText[Extract Text Data]
    ExtractText --> ParseData[Parse Receipt Data<br/>- Amount<br/>- Date<br/>- Merchant<br/>- Items]
    
    ParseData --> Confidence{Confidence Score > 80%?}
    
    Confidence -->|Yes| AutoFill[Auto-fill Expense Form]
    Confidence -->|No| ManualReview[Request Manual Review]
    
    AutoFill --> AICategory[AI Categorization]
    ManualReview --> UserCorrect[User Corrects Data]
    UserCorrect --> AICategory
    
    AICategory --> ShowResults[Show Extracted Data<br/>with Edit Option]
    ShowResults --> UserVerify{User Confirms?}
    
    UserVerify -->|Yes| CreateExpense[Create Expense with Receipt]
    UserVerify -->|No| EditData[Edit Extracted Data]
    EditData --> CreateExpense
    
    CreateExpense --> Success[Receipt Processed Successfully]
    Success --> UpdateML[Update ML Model<br/>with User Corrections]
    UpdateML --> Done([Complete])
    
    style Start fill:#e1f5e1
    style Done fill:#e1f5e1
    style OCRProcess fill:#fff3cd
    style AutoFill fill:#d4edda
    style ManualReview fill:#f8d7da
```

## 4. Budget Setup & Tracking Flow

```mermaid
flowchart TD
    Start([User Navigates to Budgets]) --> View[View Budget Dashboard]
    View --> Action{User Action}
    
    Action -->|Create New| CreateBudget[Click "Create Budget"]
    Action -->|Edit Existing| EditBudget[Select Budget to Edit]
    Action -->|View Progress| ViewProgress[View Budget Details]
    
    CreateBudget --> BudgetForm[Budget Creation Form]
    BudgetForm --> Name[Enter Budget Name]
    Name --> Category[Select Category<br/>or Overall Budget]
    Category --> Amount[Set Budget Amount]
    Amount --> Period[Choose Period<br/>- Weekly<br/>- Monthly<br/>- Quarterly<br/>- Yearly<br/>- Custom]
    Period --> DateRange[Set Date Range]
    DateRange --> Alerts[Configure Alerts<br/>- 50% threshold<br/>- 80% threshold<br/>- 100% exceeded]
    Alerts --> Rollover{Enable Rollover?}
    
    Rollover -->|Yes| RolloverConfig[Configure Rollover Rules]
    Rollover -->|No| Save
    RolloverConfig --> Save[Save Budget]
    
    Save --> Calculate[Calculate Current Spending]
    Calculate --> Display[Display Budget Progress]
    
    Display --> Monitor[Monitor Budget Status]
    Monitor --> CheckSpending{New Expense Added}
    
    CheckSpending --> UpdateProgress[Update Budget Progress]
    UpdateProgress --> CheckThreshold{Threshold Reached?}
    
    CheckThreshold -->|50%| Warning[Send Warning Notification]
    CheckThreshold -->|80%| Alert[Send Alert Notification]
    CheckThreshold -->|100%| Critical[Send Critical Alert]
    CheckThreshold -->|No| Continue
    
    Warning --> Continue[Continue Monitoring]
    Alert --> Continue
    Critical --> Suggest[Suggest Budget Adjustment<br/>or Spending Reduction]
    Suggest --> Continue
    
    Continue --> Monitor
    
    EditBudget --> BudgetForm
    
    ViewProgress --> Details[Show Detailed Breakdown<br/>- Total Spent<br/>- Remaining<br/>- Daily Average<br/>- Projected End]
    Details --> Insights[AI Insights<br/>- Spending Patterns<br/>- Recommendations]
    Insights --> Export{Export Report?}
    Export -->|Yes| GenerateReport[Generate Budget Report]
    Export -->|No| Done
    GenerateReport --> Done([Complete])
    
    style Start fill:#e1f5e1
    style Done fill:#e1f5e1
    style Save fill:#fff3cd
    style Critical fill:#f8d7da
    style Warning fill:#fff3cd
```

## 5. Bank Integration & Sync Flow

```mermaid
flowchart TD
    Start([User Navigates to Integrations]) --> IntegrationPage[View Integration Options]
    IntegrationPage --> SelectBank[Select Bank/Financial Institution]
    
    SelectBank --> InitiatePlaid[Initiate Plaid Link]
    InitiatePlaid --> PlaidModal[Open Plaid Connection Modal]
    
    PlaidModal --> SearchBank[Search for Bank]
    SearchBank --> SelectInstitution[Select Institution]
    SelectInstitution --> BankLogin[Bank Login Page]
    
    BankLogin --> Authenticate[Enter Bank Credentials]
    Authenticate --> MFA{MFA Required?}
    
    MFA -->|Yes| EnterMFA[Enter MFA Code]
    MFA -->|No| SelectAccounts
    EnterMFA --> SelectAccounts[Select Accounts to Connect]
    
    SelectAccounts --> GrantPermission[Grant Access Permission]
    GrantPermission --> PlaidCallback[Plaid Callback]
    
    PlaidCallback --> ExchangeToken[Exchange Public Token<br/>for Access Token]
    ExchangeToken --> SaveIntegration[Save Integration Record]
    SaveIntegration --> InitialSync[Trigger Initial Sync]
    
    InitialSync --> FetchTransactions[Fetch Last 90 Days<br/>of Transactions]
    FetchTransactions --> ProcessTransactions[Process Each Transaction]
    
    ProcessTransactions --> AutoMatch{Auto-Match<br/>with Existing Expense?}
    
    AutoMatch -->|Yes| LinkTransaction[Link Transaction to Expense]
    AutoMatch -->|No| CreateDraft[Create Draft Expense]
    
    LinkTransaction --> AICategory
    CreateDraft --> AICategory[AI Categorization]
    
    AICategory --> Queue[Add to Review Queue]
    Queue --> NotifyUser[Notify User: New Transactions]
    
    NotifyUser --> ReviewPage[User Reviews Transactions]
    ReviewPage --> UserAction{User Action}
    
    UserAction -->|Approve| ApproveExpense[Create/Confirm Expense]
    UserAction -->|Edit| EditExpense[Edit Transaction Details]
    UserAction -->|Ignore| IgnoreTransaction[Mark as Ignored]
    UserAction -->|Split| SplitExpense[Configure Split]
    
    ApproveExpense --> UpdateRecords
    EditExpense --> UpdateRecords
    IgnoreTransaction --> UpdateRecords
    SplitExpense --> UpdateRecords[Update Records]
    
    UpdateRecords --> ScheduleSync[Schedule Daily Auto-Sync]
    ScheduleSync --> Done([Integration Complete])
    
    ScheduleSync -.Daily Job.-> DailySync[Run Daily Sync]
    DailySync -.-> FetchTransactions
    
    style Start fill:#e1f5e1
    style Done fill:#e1f5e1
    style SaveIntegration fill:#fff3cd
    style NotifyUser fill:#d4edda
```

## 6. Analytics & Insights Flow

```mermaid
flowchart TD
    Start([User Opens Analytics]) --> Dashboard[Analytics Dashboard]
    
    Dashboard --> Load[Load Default View<br/>Current Month]
    Load --> DisplayCharts[Display Charts<br/>- Spending by Category<br/>- Trend Line<br/>- Top Merchants]
    
    DisplayCharts --> Filters[Apply Filters]
    Filters --> TimeRange{Select Time Range}
    
    TimeRange -->|This Week| FilterData
    TimeRange -->|This Month| FilterData
    TimeRange -->|Last 3 Months| FilterData
    TimeRange -->|This Year| FilterData
    TimeRange -->|Custom Range| CustomRange[Select Custom Dates]
    CustomRange --> FilterData
    
    FilterData[Filter Data] --> CategoryFilter{Filter by Category?}
    CategoryFilter -->|Yes| SelectCategories[Select Categories]
    CategoryFilter -->|No| MerchantFilter
    SelectCategories --> MerchantFilter
    
    MerchantFilter{Filter by Merchant?} -->|Yes| SelectMerchants[Select Merchants]
    MerchantFilter -->|No| TagFilter
    SelectMerchants --> TagFilter
    
    TagFilter{Filter by Tags?} -->|Yes| SelectTags[Select Tags]
    TagFilter -->|No| Aggregate
    SelectTags --> Aggregate
    
    Aggregate[Aggregate Data] --> Visualize[Generate Visualizations]
    Visualize --> AIAnalysis[Run AI Analysis]
    
    AIAnalysis --> DetectPatterns[Detect Patterns<br/>- Recurring Expenses<br/>- Unusual Spending<br/>- Trends]
    DetectPatterns --> GenerateInsights[Generate Insights]
    
    GenerateInsights --> ShowInsights[Display Insights<br/>- Spending Patterns<br/>- Anomalies<br/>- Recommendations]
    
    ShowInsights --> UserInteraction{User Interaction}
    
    UserInteraction -->|View Details| DrillDown[Drill Down into Category]
    UserInteraction -->|Compare Periods| CompareView[Show Comparison View]
    UserInteraction -->|Export Data| ExportFlow
    UserInteraction -->|Schedule Report| ScheduleReport
    
    DrillDown --> TransactionList[Show Transaction List]
    TransactionList --> Dashboard
    
    CompareView --> SelectPeriods[Select Periods to Compare]
    SelectPeriods --> GenerateComparison[Generate Comparison Charts]
    GenerateComparison --> Dashboard
    
    ExportFlow[Export Options] --> Format{Select Format}
    Format -->|PDF| GeneratePDF[Generate PDF Report]
    Format -->|CSV| GenerateCSV[Generate CSV File]
    Format -->|Excel| GenerateExcel[Generate Excel File]
    
    GeneratePDF --> Download
    GenerateCSV --> Download
    GenerateExcel --> Download[Download File]
    Download --> Dashboard
    
    ScheduleReport[Schedule Report] --> ReportConfig[Configure Report<br/>- Frequency<br/>- Email Recipients<br/>- Content]
    ReportConfig --> SaveSchedule[Save Schedule]
    SaveSchedule --> ConfirmSchedule[Confirm Scheduled]
    ConfirmSchedule --> Dashboard
    
    style Start fill:#e1f5e1
    style Dashboard fill:#e1f5e1
    style AIAnalysis fill:#fff3cd
    style GenerateInsights fill:#d4edda
```

## 7. Expense Sharing & Splitting Flow

```mermaid
flowchart TD
    Start([User Has Expense to Split]) --> CreateExpense[Create/Select Expense]
    CreateExpense --> EnableSplit[Enable "Split Expense"]
    
    EnableSplit --> SplitMethod{Choose Split Method}
    
    SplitMethod -->|Equal Split| EqualSplit[Divide Equally Among Users]
    SplitMethod -->|Percentage| PercentageSplit[Assign Percentages]
    SplitMethod -->|Custom Amount| CustomAmount[Enter Custom Amounts]
    SplitMethod -->|By Item| ItemSplit[Split by Receipt Items]
    
    EqualSplit --> SelectUsers[Select Users to Split With]
    PercentageSplit --> SelectUsers
    CustomAmount --> SelectUsers
    ItemSplit --> AssignItems[Assign Items to Users]
    AssignItems --> SelectUsers
    
    SelectUsers --> Household{From Household?}
    Household -->|Yes| SelectMembers[Select Household Members]
    Household -->|No| EnterEmails[Enter Email Addresses]
    
    SelectMembers --> Calculate
    EnterEmails --> Calculate[Calculate Split Amounts]
    
    Calculate --> Review[Review Split Details<br/>- Each User's Share<br/>- Payment Status]
    Review --> AddNote[Add Note/Description Optional]
    AddNote --> Confirm{Confirm Split?}
    
    Confirm -->|No| SplitMethod
    Confirm -->|Yes| CreateSplit[Create Expense Splits]
    
    CreateSplit --> SaveExpense[Save Expense with Splits]
    SaveExpense --> SendInvitations[Send Split Invitations]
    
    SendInvitations --> NotifyUsers[Notify Each User<br/>- Email<br/>- Push Notification<br/>- In-App]
    
    NotifyUsers --> UserReceives[Users Receive Invitation]
    UserReceives --> UserAction{User Response}
    
    UserAction -->|Accept| AcceptSplit[Accept Split]
    UserAction -->|Reject| RejectSplit[Reject Split]
    UserAction -->|Request Change| RequestModification[Request Modification]
    
    AcceptSplit --> RecordAcceptance[Record Acceptance]
    RecordAcceptance --> PaymentStatus{Mark as Paid?}
    
    PaymentStatus -->|Yes| MarkPaid[Update Payment Status]
    PaymentStatus -->|No| PendingPayment[Keep Status as Pending]
    
    MarkPaid --> UpdateExpense
    PendingPayment --> SendReminder[Schedule Payment Reminder]
    SendReminder --> UpdateExpense
    
    RejectSplit --> NotifyCreator[Notify Expense Creator]
    NotifyCreator --> CreatorAction{Creator Response}
    CreatorAction -->|Modify| ModifySplit[Modify Split Details]
    CreatorAction -->|Remove User| RemoveFromSplit[Remove User from Split]
    ModifySplit --> SendInvitations
    RemoveFromSplit --> Recalculate[Recalculate Remaining Splits]
    Recalculate --> UpdateExpense
    
    RequestModification --> ReviewRequest[Creator Reviews Request]
    ReviewRequest --> ApproveChange{Approve Change?}
    ApproveChange -->|Yes| ModifySplit
    ApproveChange -->|No| DenyChange[Deny Modification]
    DenyChange --> NotifyRequester[Notify Requester]
    NotifyRequester --> UserReceives
    
    UpdateExpense[Update Expense Status] --> CheckComplete{All Users Paid?}
    CheckComplete -->|Yes| MarkComplete[Mark Expense as Settled]
    CheckComplete -->|No| TrackPayments[Continue Tracking Payments]
    
    MarkComplete --> SendConfirmation[Send Confirmation to All]
    TrackPayments --> MonitorStatus[Monitor Payment Status]
    MonitorStatus --> SendReminders[Send Periodic Reminders]
    
    SendConfirmation --> Done([Split Complete])
    SendReminders -.Periodic Check.-> CheckComplete
    
    style Start fill:#e1f5e1
    style Done fill:#e1f5e1
    style SaveExpense fill:#fff3cd
    style MarkComplete fill:#d4edda
```

## 8. AI Chatbot Interaction Flow

```mermaid
flowchart TD
    Start([User Opens Chatbot]) --> Welcome[Show Welcome Message<br/>& Suggested Queries]
    
    Welcome --> Input[User Enters Query<br/>Text or Voice]
    Input --> ProcessInput[Process Natural Language]
    
    ProcessInput --> Intent{Detect Intent}
    
    Intent -->|Query Expenses| QueryFlow[Query Expenses Flow]
    Intent -->|Add Expense| AddExpenseFlow[Quick Add Expense]
    Intent -->|Budget Question| BudgetFlow[Budget Query Flow]
    Intent -->|Get Insights| InsightsFlow[Insights Flow]
    Intent -->|General Question| GeneralFlow[General Q&A]
    Intent -->|Unclear| Clarify[Ask for Clarification]
    
    QueryFlow --> ExtractParams[Extract Parameters<br/>- Date Range<br/>- Category<br/>- Amount Range]
    ExtractParams --> SearchExpenses[Search Expenses]
    SearchExpenses --> FormatResults[Format Results]
    FormatResults --> DisplayChat[Display in Chat]
    
    AddExpenseFlow --> ExtractExpenseData[Extract Expense Details<br/>- Amount<br/>- Category<br/>- Merchant]
    ExtractExpenseData --> ConfirmData{All Required Data?}
    ConfirmData -->|No| AskMissing[Ask for Missing Info]
    ConfirmData -->|Yes| ShowPreview[Show Expense Preview]
    AskMissing --> Input
    ShowPreview --> UserConfirm{User Confirms?}
    UserConfirm -->|Yes| CreateExpense[Create Expense]
    UserConfirm -->|No| EditDetails[Edit Details]
    EditDetails --> ShowPreview
    CreateExpense --> DisplayChat
    
    BudgetFlow --> IdentifyBudget[Identify Budget Question<br/>- Remaining Budget<br/>- Spending Rate<br/>- Projections]
    IdentifyBudget --> FetchBudgetData[Fetch Budget Data]
    FetchBudgetData --> Calculate[Calculate Response]
    Calculate --> GenerateAnswer[Generate Natural Language Answer]
    GenerateAnswer --> DisplayChat
    
    InsightsFlow --> DetermineInsight[Determine Insight Type<br/>- Spending Patterns<br/>- Anomalies<br/>- Recommendations]
    DetermineInsight --> RunAnalysis[Run AI Analysis]
    RunAnalysis --> GenerateInsight[Generate Insights]
    GenerateInsight --> FormatInsight[Format as Conversation]
    FormatInsight --> DisplayChat
    
    GeneralFlow --> CheckKnowledge{In Knowledge Base?}
    CheckKnowledge -->|Yes| RetrieveAnswer[Retrieve Answer]
    CheckKnowledge -->|No| FallbackResponse[Provide Fallback Response]
    RetrieveAnswer --> DisplayChat
    FallbackResponse --> DisplayChat
    
    Clarify --> SuggestQuestions[Suggest Example Questions]
    SuggestQuestions --> Input
    
    DisplayChat --> FollowUp{Follow-up Question?}
    FollowUp -->|Yes| MaintainContext[Maintain Conversation Context]
    FollowUp -->|No| ActionButtons[Show Action Buttons<br/>- View Details<br/>- Add Similar<br/>- Set Reminder]
    
    MaintainContext --> Input
    
    ActionButtons --> UserAction{User Clicks Action}
    UserAction -->|View Details| NavigateTo[Navigate to Relevant Page]
    UserAction -->|Add Similar| PreFillForm[Pre-fill Expense Form]
    UserAction -->|Set Reminder| CreateReminder[Create Reminder]
    UserAction -->|None| SaveHistory[Save Chat History]
    
    NavigateTo --> End
    PreFillForm --> End
    CreateReminder --> End
    SaveHistory --> End([End Session])
    
    style Start fill:#e1f5e1
    style End fill:#e1f5e1
    style ProcessInput fill:#fff3cd
    style RunAnalysis fill:#fff3cd
    style CreateExpense fill:#d4edda
```

## 9. Notification System Flow

```mermaid
flowchart TD
    Start([System Event Occurs]) --> EventType{Event Type}
    
    EventType -->|New Expense| ExpenseEvent[Expense Created Event]
    EventType -->|Budget Alert| BudgetEvent[Budget Threshold Reached]
    EventType -->|Split Request| SplitEvent[Split Invitation Received]
    EventType -->|Receipt Processed| ReceiptEvent[Receipt Processing Complete]
    EventType -->|Payment Due| PaymentEvent[Payment Reminder]
    EventType -->|Insight Generated| InsightEvent[New AI Insight]
    EventType -->|Bank Sync| SyncEvent[Bank Sync Complete]
    
    ExpenseEvent --> LoadUser
    BudgetEvent --> LoadUser
    SplitEvent --> LoadUser
    ReceiptEvent --> LoadUser
    PaymentEvent --> LoadUser
    InsightEvent --> LoadUser
    SyncEvent --> LoadUser
    
    LoadUser[Load User Preferences] --> CheckEnabled{Notifications Enabled?}
    
    CheckEnabled -->|No| Log[Log Event Only]
    CheckEnabled -->|Yes| CheckChannels[Check Enabled Channels]
    
    CheckChannels --> ChannelPref{Which Channels?}
    
    ChannelPref -->|Email| EmailQueue[Add to Email Queue]
    ChannelPref -->|Push| PushQueue[Add to Push Notification Queue]
    ChannelPref -->|SMS| SMSQueue[Add to SMS Queue]
    ChannelPref -->|In-App| InAppQueue[Create In-App Notification]
    
    EmailQueue --> EmailWorker[Email Worker Process]
    PushQueue --> PushWorker[Push Notification Worker]
    SMSQueue --> SMSWorker[SMS Worker]
    InAppQueue --> SaveInApp[Save to Notifications Table]
    
    EmailWorker --> Template[Load Email Template]
    Template --> Personalize[Personalize Content]
    Personalize --> SendEmail[Send via Email Service]
    SendEmail --> TrackEmail[Track Email Status]
    
    PushWorker --> BuildPayload[Build Push Payload]
    BuildPayload --> SendPush[Send to Device]
    SendPush --> TrackPush[Track Delivery Status]
    
    SMSWorker --> FormatSMS[Format SMS Message]
    FormatSMS --> SendSMS[Send via SMS Service]
    SendSMS --> TrackSMS[Track SMS Status]
    
    SaveInApp --> UpdateBadge[Update Notification Badge]
    UpdateBadge --> WebSocket[Send via WebSocket]
    WebSocket --> RealTimeUpdate[Real-time UI Update]
    
    TrackEmail --> RecordDelivery
    TrackPush --> RecordDelivery
    TrackSMS --> RecordDelivery
    RealTimeUpdate --> RecordDelivery[Record Delivery Status]
    
    RecordDelivery --> CheckFailure{Delivery Failed?}
    CheckFailure -->|Yes| RetryQueue[Add to Retry Queue]
    CheckFailure -->|No| MarkSent[Mark as Sent]
    
    RetryQueue --> RetryLogic[Apply Retry Logic<br/>Max 3 Attempts]
    RetryLogic --> EmailWorker
    
    MarkSent --> UpdateAnalytics[Update Notification Analytics]
    UpdateAnalytics --> Done([Complete])
    
    Log --> Done
    
    style Start fill:#e1f5e1
    style Done fill:#e1f5e1
    style SendEmail fill:#fff3cd
    style SendPush fill:#fff3cd
    style SendSMS fill:#fff3cd
    style RealTimeUpdate fill:#d4edda
```

## 10. Security & Authentication Flow

```mermaid
flowchart TD
    Start([User Attempts Access]) --> CheckAuth{Has Valid Session?}
    
    CheckAuth -->|Yes| ValidateToken[Validate JWT Token]
    CheckAuth -->|No| LoginPage[Redirect to Login]
    
    ValidateToken --> TokenCheck{Token Valid?}
    TokenCheck -->|Yes| CheckExpiry{Token Expired?}
    TokenCheck -->|No| LoginPage
    
    CheckExpiry -->|No| AuthorizeRequest[Authorize Request]
    CheckExpiry -->|Yes| RefreshFlow[Attempt Token Refresh]
    
    RefreshFlow --> HasRefreshToken{Has Refresh Token?}
    HasRefreshToken -->|No| LoginPage
    HasRefreshToken -->|Yes| ValidateRefresh[Validate Refresh Token]
    
    ValidateRefresh --> RefreshValid{Valid?}
    RefreshValid -->|No| LoginPage
    RefreshValid -->|Yes| IssueNewTokens[Issue New Access Token<br/>& Refresh Token]
    
    IssueNewTokens --> RotateRefresh[Rotate Refresh Token]
    RotateRefresh --> AuthorizeRequest
    
    LoginPage --> EnterCredentials[Enter Email/Password]
    EnterCredentials --> VerifyCredentials[Verify Credentials]
    
    VerifyCredentials --> CredsValid{Valid?}
    CredsValid -->|No| RateLimit[Check Rate Limit]
    RateLimit --> Attempts{Too Many Attempts?}
    Attempts -->|Yes| Lockout[Temporary Account Lockout]
    Attempts -->|No| ErrorMessage[Show Error Message]
    ErrorMessage --> EnterCredentials
    Lockout --> NotifyUser[Send Security Alert]
    NotifyUser --> WaitPeriod[Wait Period 15 min]
    WaitPeriod --> EnterCredentials
    
    CredsValid -->|Yes| MFAEnabled{MFA Enabled?}
    MFAEnabled -->|No| CreateSession
    MFAEnabled -->|Yes| SendMFACode[Send MFA Code]
    
    SendMFACode --> MFAPrompt[Prompt for MFA Code]
    MFAPrompt --> EnterMFA[User Enters Code]
    EnterMFA --> VerifyMFA[Verify MFA Code]
    
    VerifyMFA --> MFAValid{Valid?}
    MFAValid -->|No| MFARetry{Retry Count < 3?}
    MFARetry -->|Yes| MFAPrompt
    MFARetry -->|No| MFAFailed[MFA Failed]
    MFAFailed --> NotifySecurity[Send Security Notification]
    NotifySecurity --> LoginPage
    
    MFAValid -->|Yes| CreateSession[Create User Session]
    
    CreateSession --> GenerateTokens[Generate JWT Access Token<br/>& Refresh Token]
    GenerateTokens --> StoreSession[Store Session in Database]
    StoreSession --> SetCookie[Set Secure HTTP-Only Cookie]
    SetCookie --> LogLogin[Log Login Activity]
    LogLogin --> CheckDevice{New Device?}
    
    CheckDevice -->|Yes| DeviceAlert[Send New Device Alert]
    CheckDevice -->|No| GrantAccess
    DeviceAlert --> GrantAccess[Grant Access to Application]
    
    AuthorizeRequest --> CheckPermissions{Has Required Permissions?}
    CheckPermissions -->|No| Forbidden[Return 403 Forbidden]
    CheckPermissions -->|Yes| CheckRBAC[Check RBAC Rules]
    
    CheckRBAC --> RBACValid{Authorized?}
    RBACValid -->|No| Forbidden
    RBACValid -->|Yes| CheckDataIsolation[Check Data Isolation]
    
    CheckDataIsolation --> DataAccess{Can Access Resource?}
    DataAccess -->|No| Forbidden
    DataAccess -->|Yes| AuditLog[Log Access in Audit Trail]
    
    AuditLog --> ProcessRequest[Process Request]
    ProcessRequest --> Success[Return Response]
    
    Success --> UpdateActivity[Update Last Activity Timestamp]
    UpdateActivity --> Done([Complete])
    
    Forbidden --> LogUnauthorized[Log Unauthorized Attempt]
    LogUnauthorized --> Done
    
    GrantAccess --> Done
    
    style Start fill:#e1f5e1
    style Done fill:#e1f5e1
    style CreateSession fill:#fff3cd
    style AuditLog fill:#fff3cd
    style Lockout fill:#f8d7da
    style Forbidden fill:#f8d7da
```

---

## User Journey Summary

### New User Journey
1. **Discovery** → Landing page with features
2. **Sign Up** → Quick registration (email/OAuth)
3. **Onboarding** → Setup preferences and first budget
4. **First Expense** → Add expense manually or scan receipt
5. **Exploration** → Discover features (analytics, budgets, AI)
6. **Engagement** → Regular expense tracking
7. **Retention** → Insights, notifications, value realization

### Power User Journey
1. **Bulk Import** → Connect bank accounts
2. **Automation** → Set up recurring expenses
3. **Collaboration** → Create household, invite members
4. **Advanced Analytics** → Custom reports, export data
5. **Integration** → Connect to accounting software
6. **Optimization** → Use AI insights for savings

### Mobile User Journey
1. **Quick Capture** → Scan receipt on the go
2. **Voice Entry** → Add expense via voice
3. **Push Notifications** → Stay updated on budgets
4. **Widget Access** → Quick view of spending
5. **Offline Mode** → Queue actions for sync

---

These flows cover the main user interactions and provide a clear understanding of how users will navigate through the application.
