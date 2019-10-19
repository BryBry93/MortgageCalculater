# Mortgage calculator for a Balloon Fixed Rate  - has Maturity Balloon Payment

"""
My Mortage calculator

NOTE: The Monthy Payment(mp) stays the same throughout the life of the loan; due to the monthly princal payment(mpp) increasing over time(m) due to monthly interest paid(intst_m) decreasing as its vaule comes from total Prinical due decreases monthly.

Goal: Mortgage calculator for Balloon Fixed Rate  - has Maturity Balloon Payment(mbp)
		1) Prompt user for account details: Starting Principal balance(spb), Current Pincipal balance(cpp), Interest rate(ir), How many months are left till Maturity(m)
		2) Calc monthly payments -  show Monthy Payment Total(mtp), Monthly Interest paid(intst_m), monthly princal payment(mpp), 
		new Principal balance(npb), Projected months to pay off(M_left), 
		and mbp - call these Account Details
		3) Prompt user for additional Principal payment (Y/N) (Q: Would you like to see how an additional princial payment will 			affect..) and output modified 
			a)Prompt user: How much would you like to pay monlthy? (mp) --- must be larger than Monthly Interest paid(intst_m) 				/ if not ask for a higher value
			b) Project the new montly payment and corrsp details

		4) Project Next month's Account details - output as chart
"""


from decimal import *   "https://docs.python.org/2/library/decimal.html / set getcontext().prec = 5 & getcontext().rounding = ROUND_FLOOR"
import plotly.plotly as py
import plotly.graph_objs as go
import math                     # for the log base equ.

getcontext().prec = 5   # most results are given as 5 digits: $xxx.xx ; need to use .prec()=6 if values are $xxxx.xx
getcontext().rounding = ROUND_FLOOR


#Part 1   Prompting User info

print("Please include cents when inputing the requested amounts.")
spb = int(input("Enter the starting principal Balance: "))
cpp = int(input("If different, enter the Current Principal Balance: "))
m = int(input("Enter mortgage term (in months): "))
p_ir = float(input("Enter interest rate: "))

ir = p_ir / 100 / 12

#Part 2   Calculating Monlty payment information


try:
    numer = float(percent_ir)*current_pp
    denom = 12*(1-(1+percent_ir/12)**(-12*m))
except DivisionByZero:
    print("Sorry division by zero")

amort = Decimal(numer) / Decimal(denom)# The total Payment due this month
   
intst_per_M = Decimal(i_r)*Decimal(current_pp)  #Amount Monlthy towards interest
mpp = Decimal(amort) - intstperM    		# Amount Monthly towards Principal

print("Your Total Monthly payment for a ",m," term mortgage at an interest rate of ",p_ir," percent is: $",amort)
print("Payment towards Principal: ",mpp,"\nPayment from interest: $",intst_per_M)


# Calc the number of months left on the loan
"""
Months = -(log( 1 - int*PresntBalnc/MonthlyPayment)) / (log(1+int))
This is key if: spb != cpp, so that PresentBalnce is cpp, and the output Months wont be equal to initial user input value, m 
""" 
def remainingMonths():
    remain = input("Would you like to know how many months are left to finished off paying? \n: y/n")
    if remain != 'y' and remain != 'n':
            remain = input("Invalid input.\nWould you like to know how many months are left to finished off paying? \n y/n:")
            remainingMonths()
    elif remain == 'y':
        intst_per_M = Decimal(i_r)*Decimal(current_pp)
        mpp = Decimal(amort) - intstperM    # Amount Monthly towards Principal
        m_remain_t = math.log((1-i_r*(float(current_pp)/float(amort))))
        m_remain_b = math.log(1+i_r)
        m_remain = -m_remain_t / m_remain_b
        print("It will take another",str(m_remain)[:4]," Months to pay off")
    elif remain == 'n':
        print('bye then')
        
remainingMonths()
