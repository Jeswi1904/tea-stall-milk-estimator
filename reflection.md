# Project Reflection

### What I Got Wrong at the Start
When I first picked a tea stall, I assumed the main operational problem would be queue management or tracking UPI payments versus cash. I had vague ideas about building a quick billing app or a digital calculator. Within ten minutes of standing next to Raju's cart, I realized how detached that was from reality. Raju's hands are constantly soaked with water, milk foam, and tea stains. He operates two boiling pans simultaneously while yelling change amounts across the counter. Any tool requiring him to tap a smartphone screen during a rush would be discarded immediately. The true headache wasn't payment speed—it was the non-negotiable 5:15 PM milk delivery cutoff and his fear of unsold packets souring in the evening heat.

### What I Would Cut
If I were re-doing this in less time, I would immediately cut the `payment_method` and `customer_group_size` columns from `data.csv`. I initially hypothesized that group sizes would explain sudden surges, but keeping track of whether four college students standing together were paying as one group or four individuals created frantic note-taking for zero predictive benefit. All the model actually needed was the timestamped cup count and total milk volume emptied. Pruning those extraneous fields would have simplified both the observation and the script.

### Where AI Helped
AI was particularly useful as an accelerator for boilerplate code and error hygiene. It quickly set up Python's `argparse`, formatted the terminal dashboard cleanly, and generated defensive exception handling so malformed CSV rows wouldn't cause a fatal traceback during execution. It also helped calculate the financial margin trade-off between wholesale distributor pricing (₹29) and emergency grocery spot-buying (₹34).

### Where AI Wasted Time
AI consistently tried to over-engineer the core problem. When I first described estimating milk demand, it generated code proposing 14-day rolling ARIMA time-series models, Scikit-learn regressions, and automated weather API calls. None of that makes sense for a single roadside vendor operating with a notebook and a single afternoon's observations. I wasted roughly 25 minutes steering the AI away from statistical overkill back to a clean, practical weighted surge calculation that someone can explain in two sentences during a live phone call.
