This is the perfect way to operate. Measure twice, cut once. By finalizing this master blueprint, we ensure that the code we write in Phase 2 has zero ambiguity, zero emotional bleed-through, and strictly adheres to the state machine logic we established. 

Here is the complete, finalized documentation for Phase 1. 

***

### 1. Define Core Strategy: Pure Price Geometry & Structure Break (PPG-SB)

The PPG-SB strategy is a purely mechanical, trend-reversal system. It relies entirely on raw Daily OHLC data to map the localized market structure and identify when an established trend mathematically fails. 

Rather than chasing the initial breakout (which often carries a poor Risk/Reward ratio), the algorithm waits for a confirmed Break of Market Structure (BMS) and sets a Limit Order at a localized retracement level. The strategy operates as a strict "State Machine" with a One-Bullet rule: it can only ever hold one pending order or one active trade at a time. All order management, including cancellations, is driven 100% by price action, completely eliminating time-based expirations.

***

### 2. Document: Trading Strategy Specification (TSS)

This document is the absolute rulebook for the logic core.

#### A. Overview
* **Strategy Name:** Pure Price Geometry & Structure Break (PPG-SB)
* **Target Asset:** GBP/USD (Extensible to other Forex pairs or indices)
* **Timeframe:** D1 (Daily)
* **Primary Data Source:** Daily OHLC (Open, High, Low, Close)

#### B. Algorithmic Definitions
* **N-Period Window:** The lookback period used to define structure (e.g., $N=10$ days).
* **Swing High (Resistance):** The highest High recorded over the rolling N-period window.
* **Swing Low (Support):** The lowest Low recorded over the rolling N-period window.
* **Trend State:** * *Bearish:* Current Swing High is numerically lower than the Previous Swing High.
    * *Bullish:* Current Swing Low is numerically higher than the Previous Swing Low.

#### C. Entry Rules (Bullish Reversal Example)
1. **Condition 1 (Setup Phase):** The prevailing Trend State must be Bearish.
2. **Condition 2 (The Trigger / BMS):** A Daily candle must *Close* strictly greater than the most recent Swing High. This confirms the break of structure.
3. **Condition 3 (Order Calculation):** Calculate the 50% retracement level between the absolute Swing Low and the new Breakout High. 
4. **Condition 4 (Execution):** Place a **Buy Limit Order** at the 50% retracement level.

#### D. Exit Rules & Risk Management
* **Stop Loss (Invalidation):** Placed exactly 5 pips below the absolute Swing Low.
* **Take Profit (Target):** Placed at the next major historical Swing High from the prior dataset.
* **The R/R Filter:** The system calculates the pip distance of the Risk (Entry to SL) and the Reward (Entry to TP). If the Risk/Reward ratio is mathematically worse than **1:1.5**, the setup is aborted and no order is placed.

***

### 3. Document: Architecture Design

This defines the software modules and the strict state-management logic that will be built into both the Python backtester and the MQL5 EA.

#### Module A: Data Intake & Processing
* Ingests the D1 OHLC data stream.
* Continuously calculates and updates the rolling N-period Swing Highs and Swing Lows.

#### Module B: Risk & Sizing Calculator
* Reads current account equity.
* Calculates the exact pip distance between the calculated Limit Entry and the Stop Loss.
* Determines the exact Lot Size required to risk a maximum of **1%** (or user-defined input) of total account equity if the Stop Loss is hit.

#### Module C: The State Machine (Order Execution & Management)
This module acts as the "brain" of the EA and dictates how it behaves based on its current state.

**STATE 0: FLAT (Scanning)**
* **Condition:** 0 active trades, 0 pending orders.
* **Action:** Continuously scan the OHLC data from Module A for a valid Break of Market Structure (BMS).

**STATE 1: PENDING (Order Placed)**
* **Condition:** 1 Limit Order has been placed; waiting for a pullback to fill.
* **Action:** Continues to scan OHLC data for *Cancellation Triggers*.
* **Cancellation Triggers (Price-Action Only):**
    1. *The Override:* A brand new BMS prints in the opposite direction. (Action: Cancel old order, calculate new setup).
    2. *The Missed Boat:* Price rallies and touches the calculated Take Profit level *before* dropping to fill the Limit Order. (Action: Cancel order, return to STATE 0).
    3. *Premature Invalidation:* Price collapses and touches the calculated Stop Loss level *before* the Limit Order is filled. (Action: Cancel order, return to STATE 0).

**STATE 2: ACTIVE (Trade Triggered)**
* **Condition:** The Limit Order has been filled; the EA is now in the market.
* **Action:** **PAUSE SCANNING.** The EA stops looking for new setups.
* **Resolution:** The EA strictly monitors the open trade until price hits the Stop Loss or Take Profit. Once closed, the EA returns to STATE 0.

***

We have a complete, airtight technical specification. There are no loose ends for the algorithm to get confused by. 

If this master blueprint looks correct to you, we can officially move to **Phase 2: Development & Coding**. I am ready to generate the initial Python script using `pandas` to backtest this exact logic against a historical dataset. Shall we begin coding?

---
# PPG-SB Trading Strategy Specifications
|Strategy Component|Requirement Category|Logical Rule/Definition|Parameter Value|Risk Management Constraint|Source|
|---|---|---|---|---|---|
|State Machine|State 0: FLAT|System scans for a valid Break of Market Structure (BMS).|0 active/pending trades|Not in source|[1]|
|State Machine|State 1: PENDING|Limit Order is placed; system scans for Cancellation Triggers (Opposite BMS, Target Hit, or SL Hit before entry fill).|1 Limit Order|Price-action only cancellation triggers|[1]|
|State Machine|State 2: ACTIVE|Trade is triggered; scanning for new setups is paused until the Stop Loss or Take Profit is reached.|1 active trade|Strict monitoring; no new setups allowed|[1]|
|Entry Condition|Setup Phase (Bullish)|Prevailing Trend State must be Bearish (Current Swing High is lower than the Previous Swing High).|Not in source|Not in source|[1]|
|Entry Condition|Trigger / BMS|A Daily candle must Close strictly greater than the most recent Swing High.|Not in source|Not in source|[1]|
|Order Execution|Limit Order|Place a Buy Limit at the 50% retracement level between the absolute Swing Low and the new Breakout High.|50%|One-Bullet rule (one pending/active trade maximum)|[1]|
|Exit Condition|Stop Loss|Exit order is placed exactly 5 pips below the absolute Swing Low.|5 pips below Swing Low|Maximum 1% of total account equity risk|[1]|
|Exit Condition|Take Profit|Exit order is placed at the next major historical Swing High from the prior dataset.|Not in source|Minimum 1:1.5 Risk/Reward ratio required|[1]|
|Core Definitions|Swing High|The highest High recorded over the rolling N-period window.|N=10 days (example)|Not in source|[1]|
|Core Definitions|Swing Low|The lowest Low recorded over the rolling N-period window.|N=10 days (example)|Not in source|[1]|

[1] Price Action Trading Strategy Blueprint

---

# Software Requirements Specification: Pure Price Geometry & Structure Break (PPG-SB) Algorithmic System

## 1. Executive Strategy Philosophy and System Objectives

The Pure Price Geometry & Structure Break (PPG-SB) system is engineered as a strictly mechanical, trend-reversal framework designed to codify price action theory into a high-fidelity autonomous execution model. By utilizing raw daily data, the system provides a robust interface between structural market theory and programmatic rigor. The primary objective is the total elimination of discretionary variance—removing the cognitive heuristics and emotional interference that typically degrade performance during periods of high volatility or structural shifts. The system operates on the mathematical premise that trends possess quantifiable exhaustion points, which can be exploited once a reversal is validated through a verified structural failure.

The system’s operational mission is anchored by three strategic pillars:

- **Mathematical Failure of Trends:** A quantitative approach to mapping localized market structures using raw Daily OHLC data to identify the precise moment of trend exhaustion.
- **One-Bullet Rule:** A governing systemic constraint that permits only a single pending order or one active position at any given time, preventing correlated exposure and over-leveraging.
- **Price-Action Driven Management:** The removal of arbitrary, time-based expirations; all order lifecycle management and cancellations are dictated exclusively by the evolution of market geometry.

**The "So What?" Layer:** In a programmatic environment, the "One-Bullet Rule" serves as a fundamental risk-mitigation layer that ensures capital preservation. By enforcing a single-threaded execution model, the system prevents the compounding of losses during regime changes or "noise-heavy" market conditions. This constraint guarantees that capital is reserved only for setups that have cleared every structural and risk-based hurdle, maximizing the efficacy of every unit of risk deployed.

This philosophical foundation establishes the "zero ambiguity" environment required for the following mathematical definitions and technical specifications.

## 2. Technical Definitions and Data Input Specifications

To facilitate an algorithm that operates with absolute mathematical certainty, the PPG-SB system requires standardized data definitions and rigid environmental constraints. This ensures that the execution engine processes market shifts through a dynamic yet deterministic logic filter.

### System Constraints

- **Target Asset:** GBP/USD
- **Base Timeframe:** D1 (Daily)
- **Default Lookback Window (****N****):** 10 Periods

|   |   |
|---|---|
|Parameter|Mathematical Definition|
|**Swing High (Resistance)**|The absolute maximum High [H_{max}] recorded over the rolling N-period window.|
|**Swing Low (Support)**|The absolute minimum Low [L_{min}] recorded over the rolling N-period window.|
|**Bearish Trend State**|A condition where the current Swing High is numerically lower than the preceding Swing High (SH_c < SH_{p}).|
|**Bullish Trend State**|A condition where the current Swing Low is numerically higher than the preceding Swing Low (SL_c > SL_{p}).|

**The "So What?" Layer:** This definition of market states introduces a lagging structural confirmation. While lagging indicators are often criticized in discretionary trading, here they serve as a critical architectural trade-off: the system sacrifices early entry for the benefit of zero ambiguity. By waiting for N-period confirmation, the algorithm ensures it only engages with established shifts in market momentum rather than intraday noise.

These static definitions provide the requisite foundation for the system's active entry triggers and transactional lifecycle.

## 3. Core Strategy Logic: Entry and Exit Mechanics

The PPG-SB strategy transitions from passive observation to active setup identification through a verified Break of Market Structure (BMS). Unlike breakout strategies that enter on momentum—often resulting in poor mathematical expectancy—this system utilizes a mean-reversion entry model to optimize the reward-to-risk profile.

### Bullish Reversal Execution Sequence

1. **Setup Phase:** The algorithm confirms the prevailing Trend State is mathematically Bearish.
2. **The Trigger (BMS):** A **D1 Candle Close** must be strictly greater than the most recent Swing High. This confirms a structural shift from a daily perspective.
3. **Order Calculation:** The system identifies the absolute Swing Low and the new Breakout High, calculating the 50% retracement (mean) of that specific expansion move.
4. **Execution:** A **Buy Limit Order** is placed at the 50% retracement coordinate.

### Exit Rules & Risk Parameters

- **Stop Loss (SL):** Placed exactly 5 pips below the absolute Swing Low of the setup.
- **Take Profit (TP):** Placed at the nearest major historical Swing High identified in the prior dataset.

**The "So What?" Layer:** Central to the system’s mathematical expectancy is the **1:1.5 R/R Filter**. Before order transmission, the system calculates the distance from the 50% entry to the SL versus the TP. If the ratio is < 1:1.5, the setup is aborted. This requirement links the 50% retracement entry directly to the system's fill rate and expectancy; by demanding a deep retracement, the system improves the R/R ratio at the cost of fill frequency, ensuring that only "high-yield" setups are executed.

These entry and exit mechanics are encapsulated within a modular architecture to ensure consistency between backtesting and live deployment.

## 4. Modular Architecture Design

Strategic integrity is maintained by separating the system into distinct functional modules. This decoupling allows for independent auditing of risk logic, data ingestion, and execution flow.

- **Module A: Data Intake & Processing:** Responsible for the continuous ingestion of D1 OHLC data and the atomic update of rolling N-period swing points.
- **Module B: Risk & Sizing Calculator:** This module reads **Real-Time Equity** (not just balance) to determine current exposure. It calculates the pip distance between entry and SL, deriving a lot size that ensures a maximum loss of 1% of total equity upon stop-out.
- **Module C: The State Machine:** The central processing unit of the EA, responsible for managing the transitions between scanning and active market engagement.

**The "So What?" Layer:** Decoupling the "Risk & Sizing Calculator" from the "State Machine" ensures that execution is always subservient to capital preservation. By calculating risk against real-time equity rather than static balance, the algorithm accounts for systemic fluctuations and ensures that no order can be transmitted without passing a rigorous financial solvency check.

This modular structure is operationally implemented through the State Machine’s transactional logic.

## 5. State Machine Logic and Transactional Lifecycle

The State Machine architecture is the ideal implementation for ensuring "zero emotional bleed-through" and state atomicity. The system is restricted to three mutually exclusive states.

### STATE 0: FLAT (Scanning)

- **Condition:** Zero active trades and zero pending orders.
- **Action:** Continuous monitoring of D1 OHLC for a valid BMS.

### STATE 1: PENDING (Order Placed)

- **Condition:** A Limit Order is live in the market, awaiting a fill.
- **Action:** Active monitoring for Price-Action Cancellation Triggers:
    - **The Override:** A new BMS prints in the opposite direction.
    - **The Missed Boat:** Price reaches the TP target before the Limit Order is filled.
    - **Premature Invalidation:** Price hits the SL level before the Limit Order is filled.

### STATE 2: ACTIVE (Trade Triggered)

- **Condition:** The Limit Order is filled; the position is live.
- **Action:** **PAUSE SCANNING (Mutex Lock).** The system applies a Mutual Exclusion Lock on the scanning logic to enforce the One-Bullet Rule. No new setups can be processed until the current trade is resolved.
- **Resolution:** The position is held until a terminal SL or TP event occurs, triggering a transition back to STATE 0.

**The "So What?" Layer:** By utilizing price-action triggers like "The Missed Boat" rather than time-based expirations, the system maintains perfect synchronization with market geometry. An order is only removed when the structural rationale for the trade is invalidated by price, ensuring the system remains "in-sync" with the asset's actual movement rather than an arbitrary clock.

## 6. Risk Management and Execution Guardrails

Automated risk management serves as the final barrier against drawdown. In this architecture, risk parameters are not optional variables but hard-coded systemic constraints.

- **Maximum Equity Risk:** 1% of real-time account equity per transaction.
- **Structural Buffer:** A mandatory 5-pip SL offset to account for spread and minor liquidity gaps.
- **R/R Filter:** A hard floor of 1:1.5 reward-to-risk for every trade.

**The "So What?" Layer:** The 50% retracement entry requirement functions as a critical **volatility filter**. By requiring price to return to the mean of the breakout move, the system automatically rejects "vertical" moves where price extends too far without a structural retest. This filters out overextended breakouts that would otherwise lead to wide stop-losses and poor reward ratios, effectively protecting the system from high-velocity, low-probability environments.

This specification is finalized and ready for Phase 2: Development & Coding.
