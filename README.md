# Evening Milk Order Advisory Tool for Raju Tea Stall

A lightweight, empirical decision-support script designed to help a street tea vendor determine the exact quantity of 500ml milk pouches to order at the daily 5:15 PM delivery cutoff.

---

## Fieldwork & Context

- **Person:** Raju Kumar (Sole Chai Maker & Stall Owner)
- **Place:** Raju Tea Stall, Corner of 4th Main Road (Opposite City Bus Stop)
- **Date:** November 20, 2026
- **Walkthrough Demo Video:** [3-Minute Screen Recording & Live Run](https://youtu.be/demo-placeholder) *(Replace with your recorded video link)*

---

## The Problem

Raju runs an outdoor, single-burner roadside tea cart. He experiences an aggressive customer surge between 4:30 PM and 7:00 PM as commuters and students exit the bus stop. 

His milk distributor makes a final delivery drop at **5:15 PM**. Because Raju does not own refrigeration facilities:
- **Over-ordering** results in unboiled milk souring overnight at ambient temperatures (a 100% financial write-off).
- **Under-ordering** forces him to turn customers away or make emergency spot purchases at the adjacent grocery store at a steep retail markup (₹34 vs ₹29 wholesale), eradicating his daily profit margin.

This tool reads field-collected transaction data, isolates the rush-hour surge multiplier, and produces a single actionable packet recommendation with financial justification.

---

## How to Run

### Prerequisites
- Python 3.8+ (Uses standard library only; no `pip install` required)

### 1. Default Run (Recommended)
Reads [data.csv](file:///c:/Users/jeswi/OneDrive/Desktop/Assignment/data.csv) with default 15% safety buffer:
```bash
python main.py
```

### 2. Live Threshold Modification (Safety Buffer Tuning)
Adjust the buffer percentage to account for cloudy weather or exam days:
```bash
python main.py --buffer 25
```

### 3. Custom File or Parameters
```bash
python main.py --file data.csv --buffer 20 --evening-hours 2.5 --packet-size 500
```

### 4. Resilient Bad Row Testing
Verify that corrupted rows or negative quantities are safely quarantined without crashing:
```bash
python -c "import subprocess; print(subprocess.run(['python', 'main.py'], capture_output=True, text=True).stdout)"
```

---

## Real Terminal Output

```text
================================================================
   RAJU TEA STALL - 5:15 PM MILK ORDER ADVISORY
================================================================
Stakeholder: Raju Kumar | Location: 4th Main Road Corner
Evaluation Time: 17:15 IST Delivery Cutoff | Date: 2026-11-20
----------------------------------------------------------------
OBSERVED LOG METRICS (from data.csv):
  * Valid Transactions Logged : 25
  * Total Tea Cups Served     : 66 cups
  * Total Milk Emptied        : 5045 ml (5.04 Liters)
  * Measured Milk per Cup     : 76.4 ml/cup
  * Observation Span          : 2.32 hrs (16:03 to 18:22)
  * Baseline Sales Velocity   : 28.5 cups/hr
  * Peak Rush Velocity (17-18): 37 cups/hr (Surge: +29.9%)
----------------------------------------------------------------
EVENING SHIFT FORECAST (17:15 - 19:30 IST):
  * Estimated Remaining Demand: 76.5 cups
  * Baseline Milk Needed      : 5851 ml
  * Safety Buffer Applied     : +15% (878 ml safety margin)
  * Total Milk Requirement   : 6729 ml
================================================================
  >>> ACTIONABLE ORDER: ORDER EXACTLY 14 PACKETS (500ml each) <<<
================================================================
FINANCIAL JUSTIFICATION:
  * Wholesale Cost @ Rs.29/packet : Rs. 406.00
  * Prevents Spot-Buying Premium : Saves Rs. 70.00 vs retail grocery purchase
  * Over-Order Spoilage Risk     : 0 packets risk (demand fully absorbs order by 19:30)
================================================================
```

---

## What the Tool Gets Wrong (Limitations & Failure Modes)

1. **Weather Volatility (Sudden Downpours):**  
   The model projects demand based on preceding 1-hour rush momentum. If a sudden rainstorm hits at 5:45 PM, footfall at the open-air bus stop plummets to near zero instantly. The tool has no live meteorological inputs and would over-predict by 3–4 packets.
2. **Recipe Elasticity Under Queue Pressure:**  
   The tool assumes a constant weighted average of ~76.4 ml milk per cup. In reality, Raju stretches the tea during peak queues by simmering longer and adding water, dropping actual consumption to ~65 ml per cup. The tool therefore slightly overestimates milk burn during high queue density.
3. **Weekly & Academic Seasonality:**  
   The script does not know if today is a college exam day, a festival holiday, or a weekend where evening student traffic drops significantly.
4. **Manual Data Entry Burden:**  
   Raju cannot operate a terminal while brewing chai. The tool currently depends on either an observer logging data or an end-of-week review rather than automated vision/IoT sensors.

---

## Repository Structure

```text
├── research.md         # Step 1: Problem formulation, existing solutions, annotated sources
├── fieldwork.md        # Step 2: Observation log, interview statements, surprising insight
├── data.csv            # 25 field-collected transaction entries (timings, milk, prices)
├── main.py             # Step 3: Decision-support script with surge modeling & CLI flags
├── reflection.md       # Step 4: Half-page candid project reflection
├── photos/             # Timestamped photos from field observation
│   ├── stall_setup.jpg
│   └── tea_preparation.jpg
└── ai-chats/           # Exported AI prompt logs and architectural discussions
    └── session_log.md
```
