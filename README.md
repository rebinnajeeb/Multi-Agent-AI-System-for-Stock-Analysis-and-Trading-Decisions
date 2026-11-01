main.py
   │
   │  👉 You run this file. It loads your secret API key (.env) and
   │     sends the stock name "APPLE" into the system using .kickoff().
   ▼
crew.py  (the manager)
   │
   │  👉 This is the boss. It holds the 2 agents and 2 tasks, and decides
   │     the ORDER they run in. It takes "APPLE" and fills every {stock}
   │     placeholder in the tasks with the word "APPLE".
   ▼
TASK 1  →  analyse_task.py
   │
   │  👉 WHAT HAPPENS HERE: This is the first work order — "Research APPLE
   │     stock: get the current price, daily change, volume, and write a
   │     summary." It's just instructions. By itself it does nothing — it
   │     must be handed to an agent who can actually do the work. ↓
   ▼
AGENT 1  →  analyst_agent.py
   │
   │  👉 WHAT HAPPENS HERE: The Analyst agent (powered by the Llama LLM)
   │     reads the task. It THINKS: "To answer this, I need live data."
   │     It realizes it has a tool available, so it decides to call it. ↓
   ▼
TOOL  →  stock_research_tool.py
   │
   │  👉 WHAT HAPPENS HERE: This tool runs real Python code — yfinance
   │     connects to Yahoo Finance and downloads APPLE's current price
   │     and daily change. It returns that as a clean text string.
   │     Then it sends that data BACK UP to the agent.
   ▼
AGENT 1 writes the REPORT
   │
   │  👉 WHAT HAPPENS HERE: Now the Analyst has real numbers. It uses the
   │     LLM to turn raw data into a neat bullet-point report:
   │     "Price: $213, Change: +1.2%, trend looks positive..." etc.
   │     This finished report becomes the OUTPUT of Task 1. ↓ 
   ▼
TASK 2  →  trade_task.py
   │
   │  👉 WHAT HAPPENS HERE: Second work order — "Based on the data for
   │     APPLE, decide Buy, Sell, or Hold." Because this task runs AFTER
   │     Task 1, CrewAI automatically attaches Task 1's report as context.
   │     So this task already carries the Analyst's findings. ↓ #instructions
   ▼
AGENT 2  →  trader_agent.py
   │
   │  👉 WHAT HAPPENS HERE: The Trader agent reads the Analyst's report.
   │     It has NO tools — it doesn't fetch anything. It just REASONS over
   │     the report with the LLM and gives a final, confident decision:
   │     "BUY — price is up, momentum is positive" (with justification).
   ▼
back to main.py  →  print(result)
   │
   👉 WHAT HAPPENS HERE: The Trader's final answer travels all the way
      back up to main.py, and print(result) shows it on your screen.
