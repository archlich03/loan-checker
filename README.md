# Loan Eligibility Checker

This project is a Loan Eligibility Checker application built with Vue 3 and Vite. It allows users to input personal financial information to determine their eligibility for a loan.

The loan eligibility is determined based on the user's age, monthly income, and credit score. The calculation process is as follows:

1. **Input Validation**: Ensure that the inputs are valid numbers, with age between 1 and 100, credit score between 0 and 100, and income greater than 0 and less than or equal to 10,000. If any input is invalid, an error message is returned.

2. **Loan Amount Calculation**: The potential loan amount is calculated as 50% of the user's possible earnings until the age of 65. This is computed using the formula: 
   ```
   loanAmount = (65 - age) * income * 12 * 0.5
   ```

3. **Eligibility Criteria**:
   - If the user's age is less than 18 or greater than 64, they are considered too young or too old.
   - If the user's income is less than 400, it is deemed too low.
   - If the user's credit score exceeds the acceptable risk threshold of 10, they are considered a high-risk client.

4. **Offer Calculation**: If the user meets all criteria, an offer is made with the loan amount adjusted for risk using the formula:
   ```
   finalAmount = loanAmount * (1 + (creditScore / 100))
   ```
   The offer specifies the adjusted loan amount and the repayment period in years (until the user reaches the age of 65).

5. **Result**: The result is either a personalized loan offer or a rejection message explaining why the user is ineligible.

