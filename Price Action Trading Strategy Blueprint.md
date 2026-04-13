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
