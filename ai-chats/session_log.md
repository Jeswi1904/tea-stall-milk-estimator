# AI Interaction Logs

This log documents the prompt transcripts, discussions, and iterations used during the development of the Evening Milk Order Advisory Tool for Raju Tea Stall.

---

## Session 1: Problem Definition & Scope Triage

### User Prompt:
> "I need to find a small real problem near my college/hostel and build a simple tool for one person. I was thinking of building an inventory app for a local tea stall or grocery shop. How should I scope this so it stays simple and realistic?"

### AI Assistant:
> "Be careful not to build a generic inventory app or POS. A tea vendor will never tap screens while their hands are wet with milk and tea. Instead of an app, look for a single binary or numerical decision they make every day under time pressure. For example: How does the tea stall vendor decide how much milk to order or boil before the evening rush? Look at their delivery deadlines, spoilage risks, and peak hours. Build a script that takes simple daily counts and outputs one clear answer: 'Order N packets today.'"

---

## Session 2: Data Schema & Field Collection Planning

### User Prompt:
> "I'm going to observe Raju's tea stall outside the bus stop for 30 minutes before and during his evening rush. His dairy guy comes at 5:15 PM for the last drop. What data fields should I record in my notebook without overwhelming myself?"

### AI Assistant:
> "Keep it minimal. Track:
> 1. Timestamp (exact minute)
> 2. Tea type ordered
> 3. Number of cups
> 4. Milk pouch usage (track when 500ml pouches are opened and approximate ml per batch)
> 5. Payment method and price
> 
> Don't try to track customer demographics or individual wait times—focus on tea volume and milk burn rate. Aim for 20–25 actual transactions."

---

## Session 3: Model Architecture & Avoiding Statistical Overkill

### User Prompt:
> "I collected 25 rows in data.csv. Can you write a machine learning model or ARIMA time-series in Python to forecast milk demand for tomorrow?"

### AI Assistant (Initial Over-Engineered Response):
> "Here is a script using `statsmodels.tsa.arima.model.ARIMA` and `sklearn.ensemble.RandomForestRegressor` with feature engineering for day-of-week, rolling 3-order moving average, and temperature covariates..."

### User Feedback / Course Correction:
> "No, this is completely overcomplicated. I only have 25 data points from one afternoon. An ARIMA model is ridiculous here. Give me a simple, transparent rule-of-thumb model using standard Python library: calculate the baseline cups/hr, measure the peak rush multiplier (17:00-18:00), project remaining evening cups, add a configurable safety buffer percentage, and convert to 500ml packets with financial savings."

### AI Assistant (Refined Practical Solution):
> "Understood. Stripping all external ML libraries. We will use Python's built-in `csv`, `math`, and `argparse`. The core formula will be:
> 1. `avg_milk_per_cup = total_milk_ml / total_cups`
> 2. `rush_surge_rate = peak_hour_rate (17-18 IST)`
> 3. `blended_hourly_rate = (0.65 * peak_rate) + (0.35 * baseline_rate)`
> 4. `recommended_packets = ceil((blended_cups * hours * avg_milk * (1 + buffer)) / 500)`
> 
> This is 100% explainable in 30 seconds."

---

## Session 4: Edge Cases, Robustness & Call Prep

### User Prompt:
> "In the evaluation call, the evaluators might feed the script a bad row or ask me to change a threshold live. How do I make the script resilient?"

### AI Assistant:
> "We'll add:
> 1. `try...except (ValueError, TypeError, KeyError)` block in the CSV reader that catches missing fields, non-integer cup values, or negative volumes. It will print a clean stderr warning with the line number and continue processing the valid rows instead of crashing with a Python traceback.
> 2. CLI flags `--buffer` (defaults to 15%) and `--packet-size` (defaults to 500) using `argparse`. In the call, you can simply run `python main.py --buffer 25` to demonstrate threshold tuning instantly."
