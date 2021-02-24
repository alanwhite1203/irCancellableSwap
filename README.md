# Interest Rate Cancelable Swap Valuation and Risk

Overview
A cancelable swap provides the right but not the obligation to cancel the interest rate swap at predefined dates. Most commonly traded cancelable swaps have multiple exercise dates. Given its Bermudan style optionality, a cancelable swap can be represented as a vanilla swap embedded with a Bermudan swaption. Therefore, it can be decomposed into a swap and a Bermudan swaption. Most Bermudan swaptions in a bank book actually come from cancelable swaps. This presentation provides practical details for pricing cancelable swaps.

	Keywords
Cancelable swap, interest rate swap, Bermudan swaption, swaption, LGM model, valuation, pricing model

	Cancelable Swap Definition
	A cancelable swap gives the holder the right but not the obligation to cancel the swap at predetermined dates prior to maturity.
	It can be decomposed into a vanilla swap and a Bermudan swaption. A vanilla swap is well understood. Hence we focus on Bermudan swaption for the rest of the presentation.
	A Bermudan swaption is an option on an interest rate swap with a predefined exercise schedules.
	A Bermudan swaption gives the holder the right but not the obligation to enter an interest rate swap at predefined dates.
	Bermudan swaptions give the holders several some flexibility to enter swaps.
	A comparison of European swaption, American swaption and Bermudan swaption
	European swaption has only one exercise date at the maturity.
	American swaption has multiple exercise dates (daily)
	Bermudan swaption has multiple exercise dates (but not daily): such as quarterly, monthly, etc.

	A Cancelable Swap Example

〖PV〗_CancellablePayerSwap=〖PV〗_PayerSwap-〖PV〗_ReceiverBermudanSwaption
〖PV〗_CancellableReceiverSwap=〖PV〗_ReceiverSwap-〖PV〗_PayerBermudanSwaption



	Bermudan Swaption Payoffs
	At the maturity T, the payoff of a Bermudan swaption is given by
Payoff(T)=max⁡(0,V_swap (T))
	where V_swap (T) is the value of the underlying swap at T.
	At any exercise date T_i, the payoff of the Bermudan swaption is given by
Payoff(T_i )=max(V_swap (T_i ),I(T_i))
Where V_swap (T_i) is the exercise value of the Bermudan swap and I(T_i) is the intrinsic value.

	Model Selection Criteria
	Given the complexity of Bermudan swaption valuation, there is no closed form solution. Therefore, we need to select an interest rate term structure model and a numeric solution to price Bermudan swaptions.
	The selection of interest rate term structure models
	Popular IR term structure models: 
Hull-White, Linear Gaussian Model (LGM), Quadratic Gaussian Model (QGM), Heath Jarrow Morton (HJM), Libor Market Model (LMM).
	HJM and LMM are too complex.
	Hull-White is inaccurate for computing sensitivities.
	Therefore, we choose either LGM or QGM.
	 The selection of numeric approaches
	After selecting a term structure model, we need to choose a numeric approach to approximate the underlying stochastic process of the model.
	Commonly used numeric approaches are tree, partial differential equation (PDE), lattice, and Monte Carlo simulation.
	Tree and Monte Carlo are notorious for inaccuracy in sensitivity calculation.
	Therefore, we choose either PDE or lattice.
	Our decision is to use LGM plus lattice. 

	LGM Model
	The dynamics
dX(t)=α(t)dW
	Where X is the single state variable; W is the Wiener process.
	The numeraire is given by
N(t,X)=(H(t)X+0.5H^2 (t)ζ(t))/D(t)
	The zero coupon bond price is
B(t,X;T)=D(T)exp(-H(t)X-0.5H^2 (t)ζ(t))

	LGM Assumption
	The LGM model is mathematically equivalent to the Hull-White model but offers
	Significant improvements in calibration stability and accuracy.
	More accurate and stable in sensitivity calculation.
	The state variable is normally distributed under the appropriate measure.
	The LGM model has only one stochastic driver (one-factor), thus changes in rates are perfected correlated.

	LGM calibration
	Match today’s curve
At time t, X(0)=0 and H(0)=0. Thus Z(0,0;T)=D(T). In other words, the LGM automatically fits today’s discount curve.
	Select a group of market swaptions.
	Solve parameters by minimizing the relative error between the market swaption prices and the LGM model swaption prices.

	Valuation Implementation
	Calibrate the LGM model.
	Create the lattice based on the LGM: the grid range should cover at least 3 standard deviations.
	Find the underlying swap value at each final note.
	Conduct backward induction process iteratively rolling back from final dates until reaching the valuation date.
	Compare exercise values with intrinsic values at each exercise date.
	The value at the valuation date is the price of the Bermudan swaption.
	The present value of the cancelable swap is given by



Cancelable swap definition
Counterparty 	xxx
Buy or sell	Buy
Payer or receiver	Payer
Currency	USD
Settlement	Physical
Trade date	9/12/2012
Underlying swap definition	Leg 1	Leg2
Day Count	dcAct360	dcAct360
Leg Type	Fixed	Float
Notional	250000	250000
Payment Frequency	1	1
Pay Receive	Receive	Pay
Start Date	9/14/2012	9/14/2012
End Date	9/14/2022	9/14/2022
Fix rate	0.0398	NA
Index Type	NA	LIBOR
Index Tenor	NA	1M
Index Day Count	NA	dcAct360
Exercise Schedules
Exercise Type	Notification Date	Settlement Date
Call	1/12/2017	1/14/2017
Call	1/10/2018	1/14/2018




You can find more details at
https://finpricing.com/lib/IrCurveIntroduction.html
