# MortgageCalculater
Mortgage calculator for a Balloon Fixed Rate  - has Maturity Balloon Payment

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


#"Part 1   Prompting User info"

print("Please include cents when inputing the requested amounts.")
spb = int(input("Enter the starting principal Balance: "))
cpp = int(input("If different, enter the Current Principal Balance: "))
m = int(input("Enter mortgage term (in months): "))
p_ir = float(input("Enter interest rate: "))

ir = p_ir / 100 / 12

#"Part 2   Calculating Monlty payment information"


if spb == cpp:
    rate1 = ir*(1+ir)**m
    rate2 = ((1+ir)**m)-1
    total_1 = Decimal(rate1)/Decimal(rate2)  
    total_m_payment = Decimal(cpp) * Decimal(total_1)
else:
    rate1 = ir*(1+ir)**m
    rate2 = ((1+ir)**m)-1
    total_1 = Decimal(rate1)/Decimal(rate2)  
    total_m_payment = Decimal(cpp) * Decimal(total_1)
    
mtp = total_m_payment				# The total Payment due this month
intst_m = Decimal(ir)*Decimal(cpp)  # Amount Monlthy towards interest
mpp = mtp - intst_m     			# Amount Monthly towards Principal

# Calc the number of months left on the loan
m_remain = -math.log((1-Decimal(ir)*Decimal(cpp))/mtp)/math.log(1+Decimal(ir)) # Contains build error on math; prabably dev(0) or Decimal
""" Months = -(log( 1 - int*PresntBalnc/MonthlyPayment)) / (log(1+int))
This is key if: spb != cpp, so that PresentBalnce is cpp, and the output Months wont be equal to initial user input value, m 
""" 

print("Your Total Monthly payment for a {0} term mortgage at an interest rate of {1} percent is: ${2}".format(m, p_ir, mtp))
print("Payment towards Principal: ${0}  Payment from interest: ${1}".format(mpp,intst_m))
print("And it will take another, {0} Months to pay off".format(m_remain))

""" Optional Remaining months method - Testing not Complete
remain = str(input("Would you like to know how many months are left to finished off paying? '\n': y/n"))
if remain == y:
    # Calc the number of months left on the loan
    x = (Decimal(ir)*Decimal(cpp)
     
    y = math.log(1-(x/Decimal(cpp)))
     
    z = math.log(1+Decimal(ir))
     
    m_remain = ((-1)*y)/(z)
    print("And it will take another, {0} Months to pay off".format(m_remain))
elif remain == n:
    pass
else:
    continue
"""



						#" Outputing table "
"""
print("Total Monthly Payment \n Principal \n Interest")
for x in range(1,m+1):          # the total number of periods
    #print("Total  Principal Payment  Interest Payment")
    print()      
    print("Total Monthly Payment: ${0} Principal: ${1} Interest: ${2}".format(mtp,mpp,m),end="\t")  # "output information"
"""

y3 = input("Would you like to see an Amortization Table for your payments?")
if y3 == "Yes":
    z = 1
    while z <= m:
        rate1 = ir*(1+ir)**z
        rate2 = ((1+ir)**z)-1
        total_1 = Decimal(rate1)/Decimal(rate2)  
        total_m_payment = Decimal(cpp) * Decimal(total_1)
        
        mtp_3 = total_m_payment			  # The total Payment due this month
        intst_m = Decimal(ir)*Decimal(cpp)# Amount Monlthy towards interest
        mpp = mtp_3 - intst_m
        
        new_cpp = Decimal(cpp) - Decimal(mpp)
        new_intst_m = Decimal(ir)*Decimal(new_cpp)
        
        print("new_cpp = ",new_cpp)
        
        print()
        print("Total Payment: ${0} Principal Payment: ${1} Interest Payment: ${2}".format(mtp,mpp,intst_m))
        z += 1 

        """      !!!!!!!!!! Using Plotly, basic table  !!!!!!!!
        trace = go.Table(
            header=dict(values=['Total Payment', 'Payment to Principal', 'Payment from Interest']),
            cells=dict(values=[[100, 90, 80, 90],[95, 85, 75, 95]]))

        data = [trace] 
        py.iplot(data, filename = 'amortization_table')
        """
elif y3 == "No":
    print("Thank You and Goodbye.")
else:
    y3 = str(input("Sorry please re-enter your decicion, Yes or No? \tWould you like a projection that considers additional principal payments?: "))





"Part 3    Prompting user for Additional Monthly payments"
y1 = str(input("Would you like a projection that considers additional principal payments, Yes/No: "))
if y1 == 'Yes':
    add = float(input("How much would you like to add to your monthly payment?"))
    aug_mtp = mpp + intst_m + add  		# The new current monthly payment
    aug_mpp = (mtp - intst_m) + add     # Amount Monthly towards Principal
    intst_m = Decimal(ir)*Decimal(cpp)  # Amount Monlthy towards interest

    print("Your total Monthly payment is:${0} \tPayment towards Principal: ${1}  \tPayment from interest: ${2}".format(aug_mtp,aug_mpp,intst_m))
elif y1 == 'No':
	print("Thank You and Goodbye.")
else:
    y2 = str(input("Sorry please re-enter your decicion, Yes or No? \tWould you like a projection that considers additional principal payments?: "))






#"Part 4"   
#    				""" Outputing an Amortization Table --- Not using Plotly, just prints 3 columns """

"""for x in range(m):          # the total number of periods
    print()
    for y in range(1,4):      # the columns needs for pric pay, total, ints pay etc.
        print("$"(aug_mtp,aug_mpp,intst_m).format,end="\t")  # "output information"
"""

for x in range(1,m+1):          # the total number of periods
    print()      
    print("Total Monthly Payment: ${0} Principal: ${1} Interest: ${2}".format(aug_mtp,aug_mpp,intst_m),end="\t")  # "output information"
