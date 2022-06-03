# Objective
**Clean and preprocess** loan data for the data science team.
<br>They will create a Credit Risk Model that measures probability of default (not included in this project).

---

Notes from the data science team:
- Convert all currencies to Euro
- Quantify every categorical variable
- When measuring **credit worthiness** be extremely risk-adverse and distrustful of any unavailable data
- Casting directions are provided for each variable

# Summary of preprocessing

## Column name changes
- issue_d -> issue_date
- term -> term_months
- addr_state -> state_address
- loan_amnt -> loan_amnt_USD
- funded_amnt -> funded_amnt_USD 
- installment -> installment_USD
- total_paymnt -> total_paymnt_USD

## String Variables
### Quantification of categorical variables
All categorical variables were quantified in the following way:

#### issue_date
- Months 'Jan', 'Feb',...'Dec' were substituded with 1, 2,...12 
- Empty spaces = '0'

#### loan_status
All values were assigned either a:
- 1 (good loan status) = Current, Fully Paid, In Grace Period, Issued, Late (16 - 30 days) 
- 0 (bad loan status) = No data, Charged Off, Default, Late (31 - 120 days)

#### term_months
- 36 = 36 months
- 60 = 60 months (since we assume the wrost for our credit risk model all missing values were assigned the value of 60)

#### Grade and Sub-grade
- Grade column has been dropped as it contains only redundant data
- 1 - 36 numerical representation of Sub-Grades A1 to H1:
<br>
array(['A1', 'A2', 'A3', 'A4', 'A5', 'B1', 'B2', 'B3', 'B4', 'B5', 'C1', 'C2', 'C3', 'C4', 'C5',
       'D1', 'D2', 'D3', 'D4', 'D5', 'E1', 'E2', 'E3', 'E4', 'E5', 'F1', 'F2', 'F3', 'F4', 'F5',
       'G1', 'G2', 'G3', 'G4', 'G5', 'H1']
- H1 was created to replace all missing values

#### Verification Status
- 1 = verified
- 0 = not-verified

#### URL
- Was dropped as it contained only the ID which is also in the ID column

#### State Address
All states were grouped in 1 of four regions
- 0 = no data
- 1 = states_west
- 2 = states_south
- 3 = states_midwest
- 4 = states_east

## Numerical Variables

#### Funded Amount
All missing values were replaced with the minimum value for this variable

#### Loaned Amount, Interest Rate, Installment, Total Payment
All missing values were replaced with the respective maximum value of each column

#### Exchange Rate
- Avergae monthly exchange for the month in which the loan was issued
- Rate was obtained from a EUR-USD.csv file
- Where no month was specified the annual average was used

#### _USD and _EUR
The following columns were converted from USD to EUR, the results were stacked to the main datasets
- loan_amnt_USD : loan_amnt_EUR
- funded_amnt_USD : funded_amnt_EUR 
- nstallment_USD : nstallment_EUR
- total_paymnt_USD : total_paymnt_EUR

#### Interest Rate
- Values were converted from percentages to their 0 to 1 equivelants

## Final preprocessed and sorted dataset
['id', 'loan_amnt_USD', 'loan_amnt_EUR', 'funded_amnt_USD', 'funded_amnt_EUR', 'int_rate',
<br>'installment_USD', 'installment_EUR', 'total_pymnt_USD', 'total_pymnt_EUR', 'exchange_rate',
<br>'issue_date', 'loan_status', 'term_months', 'sub_grade', 'verification_status', 'state_address']

- Dataset was sorted accoring to ID
- Stored as loan-data-preprocessed.txt