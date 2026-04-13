# The Pure Price Geometry Handbook: Mastering Market Structure Reversals

### 1. Introduction: The Power of Raw Price Action

In algorithmic trading, success is a byproduct of removing "emotional bleed-through" and replacing it with mathematical certainty. The **Pure Price Geometry & Structure Break (PPG-SB)** strategy is a professional-grade, trend-reversal system designed to achieve this. Unlike retail strategies that rely on lagging indicators—which merely provide a delayed shadow of market movement—the PPG-SB utilizes raw Daily OHLC (Open, High, Low, Close) data to map volatility-adjusted structure in real-time.

Central to this system is the **"One-Bullet" rule**. You are strictly prohibited from holding more than one pending order or one active position at any time. This constraint is the bedrock of psychological discipline; it transforms the trader from a reactive participant into a precise operator, ensuring that every trade is a calculated strike rather than a desperate gamble.

**Key Insight:** The PPG-SB strategy functions as a zero-ambiguity system. By focusing on raw price action, you move away from subjective interpretation and toward a mechanical, state-driven methodology where every entry and exit is mathematically pre-determined.

Before we can exploit a reversal, we must first master the geometric language required to map the market's current boundaries.

--------------------------------------------------------------------------------

### 2. The Building Blocks: Defining Market Boundaries

To analyze price structure, we define the market's "geometry" using three fundamental components. For the PPG-SB system, we primarily target the **GBP/USD** on a Daily (D1) timeframe.

|   |   |   |
|---|---|---|
|Component|Technical Definition|The "So What?" (Learner Impact)|
|**N-Period Window**|A rolling lookback period fixed at **N=10** **days**.|Creates the "scope" of analysis; if a high or low isn't in the last 10 days, it is irrelevant to current structure.|
|**Swing High (Resistance)**|The highest price point recorded within the N-Period Window.|Serves as the mathematical "ceiling" and the primary trigger for a State Machine transition from State 0 to State 1.|
|**Swing Low (Support)**|The lowest price point recorded within the N-Period Window.|Serves as the mathematical "floor" and the foundation for calculating risk and trade invalidation.|

These static points of interest combine to form a dynamic **Trend State**, which tells us whether the market is currently controlled by buyers or sellers.

--------------------------------------------------------------------------------

### 3. Deciphering the Trend: Bullish vs. Bearish States

Using algorithmic logic, we determine the market's direction by comparing the current Swing High/Low to the previous ones. This determines the "Prior State" required for any setup.

- **Bearish State (Downward Trend):**
    - Defined by the relationship of the highs.
    - The **Current Swing High** must be numerically lower than the **Previous Swing High**.
- **Bullish State (Upward Trend):**
    - Defined by the relationship of the lows.
    - The **Current Swing Low** must be numerically higher than the **Previous Swing Low**.

You must first identify a confirmed trend before you can trade its failure. A reversal cannot exist without a prior state to reverse from.

--------------------------------------------------------------------------------

### 4. The Catalyst: The Break of Market Structure (BMS)

A Bullish Reversal requires a specific two-step sequence to move the system from a scanning phase to a setup phase.

1. **Condition 1 (Required Prior State):** The prevailing Trend State must be **Bearish**. You cannot seek a Bullish reversal in a market that is already mathematically Bullish.
2. **Condition 2 (The Trigger / BMS):** A Daily candle must **Close** strictly above the most recent Swing High.

The requirement of a _Close_ is non-negotiable. A mere "wick" or touch of the level is insufficient to prove that the prior bearish geometry has been invalidated.

**Warning:** Never chase the breakout candle. Chasing a rally leads to poor Risk/Reward ratios and exposes you to "the squeeze." The PPG-SB strategy ignores the breakout and waits for a specific retracement to a wholesale price.

--------------------------------------------------------------------------------

### 5. Precision Entry: The 50% Retracement Rule

Once a BMS is confirmed, we calculate a precise entry that solves the Risk/Reward problem inherent in breakout trading. We use a **Buy Limit Order** at the 50% retracement level between the absolute Swing Low and the new Breakout High.

**Step-by-Step Execution:**

1. Identify the **absolute Swing Low** (the floor of the move) and the **Breakout High** (the peak of the BMS candle).
2. Calculate the **Entry Price** and the **Risk in Pips**.
3. Place the Buy Limit Order.

```text
Entry Price = (Breakout High + Swing Low) / 2
Risk Pips = Entry Price - (Swing Low - 5 pips)
```

By calculating the Pip Distance immediately, you provide the necessary data for the Risk Management filter and lot-sizing logic.

--------------------------------------------------------------------------------

### 6. The Rulebook: Risk Management and Exit Strategy

The PPG-SB system is built on mathematical invalidation. If the math doesn't work, the trade doesn't exist.

- **Stop Loss (The Invalidation):** Placed exactly **5 pips below** the absolute Swing Low.
- **Take Profit (The Target):** Placed at the next major historical Swing High found in the **prior dataset** (outside the current 10-day window).
- **1% Risk Rule:** You must use a lot-sizing calculator to ensure that if the Stop Loss is hit, you lose a maximum of **1% of total account equity**.
- **The R/R Filter:** The Reward (Entry to TP) must be at least **1.5 times** the Risk (Entry to SL).

**Trade Pre-Flight Checklist:**

- [ ] Was the prior state Bearish? (Condition 1)
- [ ] Did a Daily candle close above the Swing High? (Condition 2)
- [ ] Is the Stop Loss exactly 5 pips below the low?
- [ ] Is the Risk/Reward ratio **1:1.5 or better**?
- [ ] Is the lot size calculated to risk only 1% of equity?

**If the Risk/Reward ratio is mathematically inferior (less than 1:1.5), the setup is aborted immediately. No exceptions.**

--------------------------------------------------------------------------------

### 7. The Logic Engine: Understanding the State Machine

The strategy operates as a "State Machine," a logical flow that prevents emotional interference by dictating exactly what the trader's "brain" should be doing at any moment.

#### STATE 0: FLAT (The Scan Phase)

You have zero active trades and zero pending orders. Your only objective is to scan the Daily OHLC data for a valid BMS that occurs after a confirmed Bearish state.

#### STATE 1: PENDING (The Cancellation Triggers)

A Buy Limit Order has been placed. You are waiting for price to drop and "fill" your order. The order is cancelled and you return to STATE 0 if any of these **Price-Action Only** triggers occur:

- **The Override:** A new BMS prints in the opposite direction.
- **The Missed Boat:** Price rallies and **touches** the Take Profit target _before_ filling your entry. (Unlike the BMS, no close is required; a touch invalidates the setup).
- **Premature Invalidation:** Price collapses and touches the Stop Loss level _before_ the order is filled.

#### STATE 2: ACTIVE (The Execution Phase)

Your order is filled. **PAUSE SCANNING.** To enforce the One-Bullet Rule, the system stops looking for new setups entirely. You are now a passive observer. The trade is managed strictly until it hits either the Stop Loss or the Take Profit. Once closed, the system resets to STATE 0.

By adhering to this mechanical cycle, you remain a disciplined architect of price geometry rather than a victim of market noise.

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

***

# Strategy Investment Prospectus: Pure Price Geometry & Structure Break (PPG-SB)

### 1. The Strategic Mandate: Mechanical Precision in Global Markets

In the current landscape of institutional finance, the transition from discretionary oversight to a purely mechanical, state-machine-driven architecture is a prerequisite for sustainable alpha generation. The Pure Price Geometry & Structure Break (PPG-SB) strategy is engineered to neutralize the volatility inherent in the GBP/USD and other major currency pairs by prioritizing deterministic execution over human intuition. By codifying market behavior into binary, mathematical states, this framework eliminates the "emotional bleed-through" that frequently compromises portfolio integrity in high-stakes environments.

The following table delineates the core parameters of the strategic mandate:

|                         |                                                     |
| ----------------------- | --------------------------------------------------- |
| Parameter               | Specification                                       |
| **Strategy Name**       | Pure Price Geometry & Structure Break (PPG-SB)      |
| **Target Asset**        | GBP/USD (Extensible to major G10 pairs and indices) |
| **Timeframe**           | D1 (Daily)                                          |
| **Primary Data Source** | Raw Daily OHLC (Ground Truth)                       |
| **Execution Style**     | Deterministic State-Machine / Limit Order Entry     |

The strategic reliance on raw Daily OHLC data as the "ground truth" for market structure ensures architectural robustness. By utilizing the Daily Close as the primary signal for structural shifts, the system filters out intra-day noise and reactionary impulses. This objective foundation allows for a scalable model that maintains its logic across varying market regimes, providing the rigorous clarity required for institutional deployment.

The subsequent sections define the geometric and state-transition protocols that form the core of the PPG-SB framework.

--------------------------------------------------------------------------------

### 2. Methodological Framework: Defining Market Structure

To establish a repeatable and scalable investment model, market structure must be mapped through objective, mathematical definitions. The PPG-SB strategy utilizes an **N-Period Window**—a variable input parameter (e.g., N=10 days) used for backtesting—to identify localized levels of resistance and support.

The following formal notations govern the system’s structural analysis:

- **Swing High (Resistance):** Defined as Max(High_{t-N}, \dots, High_t) within the rolling window.
- **Swing Low (Support):** Defined as Min(Low_{t-N}, \dots, Low_t) within the rolling window.

By distilling price action into these numerical constraints, the system distinguishes between bullish and bearish environments with zero ambiguity. The "Trend State" is determined by the following symmetrical logic:

|   |   |
|---|---|
|Trend State|Mathematical Requirement|
|**Bullish State**|Current Swing Low is numerically higher than the Previous Swing Low.|
|**Bearish State**|Current Swing High is numerically lower than the Previous Swing High.|

These structural definitions serve as the immutable foundation upon which all trade execution protocols are built.

--------------------------------------------------------------------------------

### 3. Execution Protocol: The Trigger and the Entry

The PPG-SB strategy operates on the principle of "measure twice, cut once," prioritizing capital efficiency over breakout participation. Rather than chasing price momentum—a practice that yields suboptimal risk-adjusted parity—the strategy utilizes "measured entries" via Limit Orders. This ensures that capital is only deployed when price returns to a mathematically favorable equilibrium.

The technical protocol for a **Bullish Reversal** is governed by the following sequence:

1. **Setup Phase:** The prevailing Trend State must be mathematically confirmed as Bearish.
2. **The Trigger (Break of Market Structure):** A Daily candle must **Close** strictly higher than the most recent Swing High. This _Close_ is the only data point capable of triggering a state transition.
3. **Order Calculation:** The system identifies the 50% retracement level (Equilibrium) between the absolute Swing Low and the new Breakout High.
4. **Execution:** A Buy Limit Order is placed precisely at this 50% level.

**Order Calculation Phase:** The 50% retracement is the absolute Limit Order execution point. The system calculates the mid-point between the most recent structural low and the peak of the breakout candle to ensure entry occurs at a localized discount, maximizing the probability of a high-expectancy outcome.

This rigorous sequence ensures the transition from observation to deployment is entirely governed by confirmed structural shifts.

--------------------------------------------------------------------------------

### 4. Quantitative Risk Mitigation & The "One-Bullet" Rule

Strict risk constraints are mandatory for the preservation of institutional capital. The PPG-SB strategy enforces a "One-Bullet" rule: the system is permitted to hold only one pending order or one active trade at any given time. This constraint is designed to manage idiosyncratic risk and prevent portfolio bloat during periods of high localized volatility.

The following mandated filters enforce global risk-adjusted parity:

- **Risk/Reward (R/R) Filter:** Every setup must pass a mathematical expectancy test. If the projected Reward-to-Risk ratio is lower than **1:1.5**, the setup is discarded and no order is placed.
- **Capital Allocation:** Risk is strictly capped at a maximum of **1% of total account equity** per trade.
- **Stop Loss (Invalidation):** The invalidation point is set exactly **5 pips below the absolute Swing Low** (for longs). This buffer accounts for spread and slippage while providing a consistent structural exit.
- **Take Profit (Target):** Targets are identified using the next major historical Swing High from the prior dataset, ensuring exits are grounded in verified historical structure.

These rules are non-discretionary and are enforced by the system’s internal state machine.

--------------------------------------------------------------------------------

### 5. Architectural Integrity: The State Machine Logic

The "brain" of the PPG-SB strategy is a strict State Machine. Unlike traditional algorithmic models that lack rigorous state-management, this architecture ensures the system is always in one of three mutually exclusive operational states, eliminating signal ambiguity.

#### STATE 0: FLAT (Scanning)

The system is in a neutral state with zero exposure. It continuously scans Daily OHLC data for a valid Break of Market Structure (BMS) as defined in the execution protocol.

#### STATE 1: PENDING (Order Placed)

A Buy/Sell Limit Order has been placed. The system monitors for three "Price-Action Only" cancellation triggers, which dictate state transitions without the need for arbitrary time-based expirations:

1. **The Override:** A new BMS occurs in the opposite direction. (Action: **Cancel current order and return to STATE 0** to recalculate new setup).
2. **The Missed Boat:** Price reaches the Take Profit target before filling the Limit Order. (Action: **Cancel order and return to STATE 0**).
3. **Premature Invalidation:** Price hits the Stop Loss level before filling the Limit Order. (Action: **Cancel order and return to STATE 0**).

#### STATE 2: ACTIVE (Trade Triggered)

The Limit Order has been filled and the system is in the market. Upon entering this state, the system triggers a **PAUSE SCANNING** directive. This is the primary constraint of the logic core, ensuring no new setups are sought until the current position is resolved by the Stop Loss or Take Profit.

--------------------------------------------------------------------------------

### 6. Operational Scalability & Institutional Readiness

The modular nature of the PPG-SB strategy ensures it is prepared for high-fidelity backtesting and technical deployment in MQL5 or Python environments.

|   |   |   |
|---|---|---|
|Module|Primary Function|Strategic Value to the Investor|
|**Module A: Data Intake**|Ingests D1 OHLC data and updates rolling N-period Highs/Lows.|Ensures the strategy reacts to "Ground Truth" price action only.|
|**Module B: Risk Calculator**|Determines lot sizes based on equity and pip distance to Stop Loss.|Guarantees absolute adherence to the 1% risk-per-trade mandate.|
|**Module C: State Machine**|Manages transitions between Flat, Pending, and Active states.|Eliminates human error and ensures zero ambiguity in order management.|

The PPG-SB master blueprint is now airtight and mathematically sound. By establishing these deterministic execution protocols, we have successfully mitigated discretionary risk. The system is now ready for **Phase 2: Development & Coding**, where it will undergo rigorous historical backtesting across multiple market cycles to validate its long-term expectancy.
