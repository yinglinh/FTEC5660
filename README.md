# FTEC5660 Homework 1: Receipt Chain

Build a LangChain pipeline that reads every supermarket receipt in a folder
with the vision-capable DeepSeek Flash model and answers these two questions:

1. How much money did I spend in total for these bills?
2. How much would I have had to pay without the discount?

For this homework, **amount spent** means the final payment after the receipt's
rounding line. **Without the discount** means the sum of the original positive
item prices: add back every promotion, coupon, member, app, packaging-damage,
and percentage discount, but do not add back rounding.

## Student task

Only edit the two functions in `hw1.py` that contain `### YOUR CODE HERE`:

- `build_chain()` creates your LangChain chain.
- `answer_queries()` runs the chain on the receipt images and returns one final
  response for each question.

You may use prompt chaining, routing, parallel calls, reflection, or a
combination. Your final responses should each contain one HKD amount. Do not
hard-code filenames or public answers; grading uses unseen receipt folders.

## Setup and public test

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Put your DeepSeek key after `DEEPSEEK_API_KEY=` in `.env`, then run:

```bash
python3 hw1.py --image-folder public_test
```

The program creates `results.csv` in the current directory. Its columns are
`query`, `model_response`, and `correctness`. The public answers are in
`public_test/ground_truth.json`. The starter intentionally returns the dummy
response `please design your chain to answer these two queries.` so it runs
before you add any API code.

The required model is `deepseek-v4-flash-vision-exp`, the vision-capable
DeepSeek Flash model. JPEG, PNG, GIF, and WebP inputs are accepted by the
homework runner.


## Homework 1 solution:
> to students: please fill your solution description here.
## Homework 1 solution
```mermaid
flowchart LR
    ReceiptImage[Supermarket Receipt Image] --> Prompt[System Prompt + Multimodal User Message]
    Prompt --> LLM[deepseek-v4-flash-vision-exp]
    LLM --> JsonParser[JsonOutputParser]
    JsonParser --> Loop[Loop over receipts & accumulate sums]
    Loop --> Format[Format values as HK$ strings]
    Format --> Output[Query result dictionary]


This solution builds a LangChain chain for supermarket receipt parsing. The system prompt defines strict parsing rules and provides an escaped JSON example. It reminds the LLM to capture every discount entry and explicitly exclude ROUNDING value from discount calculation. The multimodal model deepseek‑v4‑flash‑vision‑exp reads receipt images and outputs structured JSON. JsonOutputParser converts model output to Python dictionary. We iterate through each receipt, sum `final_payment` for total spending, calculate `subtotal + total_discount` for price without discount, format final answers into HK$ currency strings.
