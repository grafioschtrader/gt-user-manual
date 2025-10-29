---
title: "Transactions"
date: 2025-10-28T22:54:47+01:00
draft: false
weight: 60
archetype: "default"
---

The **Transaction Report** provides a comprehensive overview of all financial transactions within a tenant. This report enables viewing, filtering, and analyzing all security and account transactions. It is the central tool for monitoring all entries and withdrawals in the portfolio.

## Accessing the Report
The Transaction Report can be accessed at multiple levels:

1. **Tenant Level**: Via Main Menu → **Accounts & Portfolios** → **Portfolios** → **Transactions**
   - Shows all transactions across all portfolios and accounts of the tenant
   - Most comprehensive view of all financial movements

2. **Portfolio Level**: Within a single portfolio → **Transactions**
   - Shows only transactions of the selected portfolio
   - Focused view on portfolio-specific activities

3. **Account Level**: Within a cash account or securities depot
   - Shows only transactions of the selected account
   - Ideal for monitoring individual accounts

## Report Content
The Transaction Report displays all types of financial transactions in a tabular view:

### Security Transactions
- **Buy (Accumulate)**: Security purchases of all types
- **Sell (Reduce)**: Security sales
- **Dividend**: Dividend payments and interest on securities
- **Finance Cost**: Costs for margin positions (can be positive or negative)

### Account Transactions
- **Deposit**: Cash deposits to cash accounts
- **Withdrawal**: Cash withdrawals from cash accounts
- **Interest Cashaccount**: Interest income on cash accounts
- **Fee**: Account and depot management fees

### Account Transfers
- **Account Transfer**: Transfers between two cash accounts within the tenant
- Displayed as connected transactions (Withdrawal + Deposit)

## Column Overview
The table displays the following information for each transaction:

| Column | Description | Filterable |
|--------|-------------|------------|
| **Date** | Date and time of the transaction | Yes (Date range) |
| **Cashaccount** | Name of the affected cash account | Yes (Select filter) |
| **Account Currency** | Currency of the cash account | Yes (Select filter) |
| **Transaction Type** | Type of transaction (Buy, Sell, Deposit, etc.) | Yes (Select filter) |
| **Security** | Name of the security (security transactions only) | Yes (Select filter) |
| **Quantity** | Number of traded units (securities only) | Yes (Numeric) |
| **Quotation/Dividend** | Price per unit or dividend amount | Yes (Numeric) |
| **Currency** | Currency of the security/transaction | Yes (Select filter) |
| **Exchange Rate** | Conversion rate to account currency (if different) | Yes (Numeric) |
| **Tax Cost** | Taxes paid for the transaction | Yes (Numeric) |
| **Transaction Cost** | Transaction fees (order fees, etc.) | Yes (Numeric) |
| **Cashaccount Amount** | Impact on cash balance (positive/negative) | Yes (Numeric) |

**Important**:
- **Cashaccount Amount** is displayed in **Red** when negative (cash outflow)
- **Cashaccount Amount** appears in normal color when positive (cash inflow)
- Default sorting is by **Date** descending (newest first)

## Filter Options
The report offers extensive filtering capabilities:

### Column Filters
Each column can be filtered individually:

1. **Date Filter**:
   - Exact date
   - Date range (before, after, between)
   - Calendar picker for easy input

2. **Select Filters** (Dropdown):
   - Cashaccount: Filter by specific account
   - Account Currency: Filter by currency
   - Transaction Type: Filter by transaction type
   - Security: Filter by specific security
   - Currency: Filter by transaction currency

3. **Numeric Filters**:
   - Quantity: Greater, less, equal, between
   - Quotation: Define price range
   - Exchange Rate: Specific rate ranges
   - Tax Cost, Transaction Cost, Cashaccount Amount: Any numeric ranges

### Multiple Filters
- Multiple filters can be applied simultaneously
- Filters are combined with AND logic
- Filters remain active until manually removed

### Multi-Sorting
- Sortable by clicking column headers
- Multi-sorting by holding Ctrl/Cmd key
- Reverse sort direction by repeated clicking

## Editing Functions
### Edit Transaction
An existing transaction can be edited in various ways:

**Access**:
1. Right-click on transaction → **Edit Transaction**
2. Or: Select transaction → Choose "Edit Transaction" in menu

**Editable Fields** (depending on transaction type):
- Date and time of transaction
- Quantity (for securities)
- Quotation/Price
- Tax Cost
- Transaction Cost
- Note
- Ex-Date (for dividends)
- Taxable Interest (for interest/dividends)

**Important**:
- Account and security cannot be changed after creation
- For margin positions: Connected transactions are automatically updated
- For account transfers: Both sides of the transaction are edited together

### Delete Transaction
**Access**:
1. Right-click on transaction → **Delete Transaction**
2. Or: Select transaction → Choose "Delete Transaction" in menu

**Confirmation**:
- System requires confirmation before deletion
- For account transfers: Both connected transactions are deleted
- For margin positions: Check for open connected positions

**Warning**: Deleting a transaction is permanent and cannot be undone!

### Convert to Account Transfer
A single deposit or withdrawal can be converted into an account transfer retroactively.

**Prerequisites**:
- Transaction must be of type "Deposit" or "Withdrawal"
- Transaction must not already be connected to another transaction

**Access**:
1. Right-click on transaction → **Convert to Account Transfer**
2. Dialog opens to select target/source account
3. System automatically creates connected counter-entry

**Use Case**: When a withdrawal was originally recorded as a normal withdrawal, but was actually an internal transfer.

## Context Menu
The context menu (right-click on a transaction) offers the following options:

1. **Edit Transaction**
   - Available: Always for existing transactions
   - Opens appropriate edit dialog

2. **Delete Transaction**
   - Available: Always for existing transactions
   - Deletes transaction after confirmation

3. **Convert to Account Transfer**
   - Available: Only for unconnected deposits/withdrawals
   - Creates connected counter-entry on another account

## Table Settings
### Pagination
- Default display: **50 transactions** per page
- Selectable: 20, 30, 50, or 100 rows per page
- Navigation via pagination controls at bottom of table

### Column Widths
- Columns have predefined widths for optimal readability
- Widths are optimized for data length
- Fixed widths for consistent display

### Selection
- Single row can be selected by clicking
- Selected row is highlighted
- Context menu and main menu respond to selection

## Data Refresh
The report is automatically updated when:
- A new transaction is created
- An existing transaction is edited
- A transaction is deleted
- An account transfer is created

Updates occur immediately after the action without page reload.

## Special Transaction Types
### Margin Positions
**Opening**:
- Transaction type "Buy" (Accumulate) or "Sell" (Reduce)
- Negative quantity for short positions

**Finance Costs**:
- Separate transaction of type "Finance Cost"
- Connected to opening transaction
- Can be positive (credit) or negative (debit)

**Closing**:
- Counter-transaction to opening
- System automatically calculates profit/loss
- Connection to opening transaction is maintained

### Dividends After Sale
**Special Case**: Dividend is paid after security has been sold

**Recording**:
- Transaction type "Dividend"
- Ex-Date must be specified (record date for dividend entitlement)
- Security reference remains, even if position already sold

### Account Transfers
**Structure**:
- Consist of two connected transactions
- Withdrawal on source account
- Deposit on target account
- Both transactions reference each other

**Currency Conversion**:
- For different account currencies
- Exchange rate is used for both transactions
- Amount is converted accordingly

## Technical Details
### REST API Endpoints
**Get all transactions of a tenant**:
```
GET /api/transaction
```

**Get all transactions of a portfolio**:
```
GET /api/transaction/portfolio/{idPortfolio}
```

**Get single transaction**:
```
GET /api/transaction/{idTransaction}
```

**Create transaction (security)**:
```
POST /api/transaction/securitytrans
```

**Update transaction (security)**:
```
PUT /api/transaction/securitytrans
```

**Create transaction (account)**:
```
POST /api/transaction/singlecashtrans
```

**Create/update account transfer**:
```
POST /api/transaction/cashaccounttransfer
```

**Delete transaction**:
```
DELETE /api/transaction/{idTransaction}
```

### Data Validation
**Backend Validation**:
- Transaction date: Must be after 01/01/1900
- Quantity: Must be positive for security transactions
- Quotation: Must be specified for security transactions
- Cashaccount Amount: Must always be specified
- Tenant membership: Automatically validated

**Frontend Validation**:
- Required fields are checked before saving
- Numeric fields only allow valid numbers
- Date fields use calendar picker
- Invalid inputs are marked in red

### Performance
**Data Load Time**:
- Transactions are loaded when opening the report
- Sorting occurs server-side (by transaction time descending)
- Filtering occurs client-side (fast response)

**Memory Usage**:
- All transactions are kept in memory
- Performance may be affected with many transactions (>1000)
- Using filters reduces displayed data amount

## Typical Use Cases
1. **Verify a transaction**: Use filter by security or date
2. **Check fees**: Set filter to transaction type "Fee"
3. **Dividend overview**: Filter by transaction type "Dividend"
4. **Correct an error**: Find transaction, edit or delete
5. **Trace an account transfer**: Filter by date, check connected transactions
6. **Analyze trading activity**: Filter by portfolio or time period

## Limitations and Notes
- **Retroactive changes**: Account and security cannot be changed after creation
- **Deletion**: For open margin positions, position must be closed first
- **Performance**: Table may slow down with many transactions (>5000)
- **Currencies**: Conversions based on historical rates at transaction time
- **Time zone**: Transaction times are stored in server's system time zone
- **Sorting**: Transaction order is important for position calculations (FIFO principle)

## Tips for Effective Use
1. **Regular checks**: Review new transactions after import/manual entry
2. **Save filters**: Bookmark frequently used filter combinations in browser
3. **Use notes**: Add notes to special transactions (e.g., "Stock split 2:1")
4. **Connected transactions**: Pay attention to "Connected to" field for margin positions
5. **Export function**: Use Excel export for extensive analysis (if available)
6. **Data backup**: Create backup before bulk changes

## Troubleshooting
**Problem**: Transaction cannot be deleted
- **Solution**: Check if an open margin position exists that must be closed first

**Problem**: Filter doesn't work
- **Solution**: Check if other filters are active that contradict

**Problem**: Cashaccount amount is incorrect
- **Solution**: Check exchange rate and rounding in currency conversions

**Problem**: Transaction is not displayed
- **Solution**: Check pagination and active filters

**Problem**: Sorting doesn't change
- **Solution**: Click column header again or remove other sortings
