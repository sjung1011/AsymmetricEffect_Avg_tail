=======================================================
README
=======================================================
Project : Asymmetric Capital Flows and Portfolio Reallocation to SOEs
Author  : Seojin Jung
Date    : Spring 2026
-------------------------------------------------------

OVERVIEW
--------
[Brief description of the project]

-------------------------------------------------------

FOLDER STRUCTURE
----------------
/Data           - Raw and processed data files
/FU             - Construct Financial Uncertainty
/Emp1 		- external IV, TVAR, GIRF
/Emp2

-------------------------------------------------------

DATA
----
FU_work.xlsx
  - Sheet FU_q : Quarterly FU series (1980Q2–2025Q4)

-------------------------------------------------------


-------------------------------------------------------
FU CONSTRUCTION: FU folder 
---------------
Main running file: CreateFU.m 
		- Sheet FU_q : Quarterly FU series (1980Q2–2025Q4) is constructed)
		- other results are stored in FU/FU.mat
Data    : Data/FU_work.xlsx 
Model   : Local Level Model with Stochastic Volatility
Method  : Bayesian MCMC (Gibbs sampler)
         10-component normal mixture (Omori et al.)
nIter   : 20,000
burnIn  : 15,000
Keep    : 5,000 draws
Prior   : psi_mu ~ IG(2, 0.01), psi_sigma ~ IG(2, 0.10)
Input   : Monthly S&P 500 log returns (1980m2–2026m2)
Output  : FU_t = sigma_t^{mkt} = exp(h_t/2)
Freq    : Monthly → averaged to Quarterly
-------------------------------------------------------

-------------------------------------------------------
STUDY 1 GIRF: EmpI folder
Main running file: run_IRF.m
		 - Results are stored in Results_EmpI
		 - The code is adapted from the methodology in Alessandri and Mumtaz (2017).
Data    : Data/TVAR.xlsx
Sheet IRF : Endogenous variables (GFC_d1, FU, EBP, GFU, GFCI, GCA, GRA, WIP)
Sheet IV  : Auxiliary equation variables (UI, IP, CPI, FF, UER, TYM)
Model   : Threshold VAR (TVAR), two regimes (risk-off/risk-on)
Regime  : S_t = 1{GFC_{t-1} <= z*}, z* = 25th percentile of GFC_{t-1}
Shock   : US financial uncertainty (FU), identified via SVAR-IV (Mertens & Ravn, 2013)
Method  : Bayesian MCMC (Gibbs sampler) + GIRF (Koop, Pesaran & Potter, 1996)
nIter   : 20,000
burnIn  : 15,000
Keep    : 5,000 draws
Lags    : p = 2 (baseline), p = 4 (robustness)
GIRF    : h = 0:12, MC draws per history = 500
Output  : fsave1 (risk-off), fsave2 (risk-on) — posterior GIRF draws [5000 x 13 x 7]
-------------------------------------------------------


NOTES
-----
- [Any additional notes, data sources, or caveats]

-------------------------------------------------------