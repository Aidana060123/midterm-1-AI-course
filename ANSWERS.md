# Midterm 1: answers

GitHub username: aidana060123
Session code: ember-92
Fingerprint: fe569426

## AI-use declaration

I did the calculations (recall@k, MRR@10, Wilson intervals, annual costs, the Part 2 figures and the Part 3 numbers) together with Claude (Anthropic's assistant in claude.ai). Claude helped me with the Python code, including running the course generator make_my_data.py, and we went through the results together and cross-checked them against the generator's own functions. Claude also helped me draft the wording of the decision, the memo and the Why lines. I read the results, checked that the fingerprint matches my own DATA.md, and I can explain every number and every recommendation in this file. I used no other AI tools.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.6167 | 0.6833 | 0.7000 | 0.6577 |
| B | 0.6000 | 0.7000 | 0.7500 | 0.6665 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.6833 | 0.5577 | 0.7869 |
| A, k=5 | 0.7000 | 0.5749 | 0.8010 |
| B, k=3 | 0.7000 | 0.5749 | 0.8010 |
| B, k=5 | 0.7500 | 0.6277 | 0.8422 |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 28720.97 |
| A, k=5 | 37090.80 |
| B, k=3 | 19441.77 |
| B, k=5 | 21625.46 |

### 1d. Decision (at most 150 words)

Chosen candidate: B, k=3

The annual budget is 25,200, so A,3 (28,721) and A,5 (37,091) are out. Affordable: B,3 (19,442; recall 0.70, interval 0.57-0.80) and B,5 (21,625; recall 0.75, interval 0.63-0.84). The intervals overlap heavily, so B,5's 5-point gain (3 questions out of 60) is not shown to be real. I choose the cheaper B,3 and save 2,184 a year. Recall of B,3 by language (20 questions each): EN 0.60 (12/20), RU 0.80 (16/20), KK 0.70 (14/20). English is served worst, but the language intervals also overlap (EN 0.39-0.78), so with 20 questions per language I cannot claim a real gap. Risk: about 3 in 10 questions miss the right chunk in the top 3. If the bank wants more recall, B,5 is affordable.

### 1e. Next measurement (at most 60 words)

Run B,3 and B,5 on the same 300 new test questions, 100 per language, and compare them question by question. If B,5 wins clearly more questions than it loses, switch to B,5 for its extra recall; if not, keep B,3. The same data shows whether English really has lower recall than Russian.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.9167 | RU, degraded | 0.7667 | 0.5907 | 0.8821 | 33611.03 |
| L | 0.8111 | RU, degraded | 0.7000 | 0.5212 | 0.8334 | 21600 |

Personal-data pages per year sent abroad under H: 689850

### 2b. Memo (at most 300 words)

Recommendation: pipeline L, with human review of degraded scans.

Who is served worst. Overall, H is more accurate: 91.7% (interval 86.7-94.9%) against 81.1% (74.8-86.2%) for L; the intervals just fail to overlap. But the overall figure hides scan quality. Under H, degraded Russian scans reach only 76.7% (59.1-88.2%). Under L, degraded Russian and degraded Kazakh scans both reach 70.0% (52.1-83.3%), and all degraded scans 73.3% against 85.6% for H. With 30 invoices per cell these intervals are wide, so cell-by-cell gaps between H and L are not proven.

Personal data. 27% of 7,000 pages a day carry a name and ИИН: 689,850 pages a year would leave the country under H. Law No. 94-V covers the ИИН. Under H, the scan goes to a data centre abroad (known); who stores it and for how long are unknown, and a vendor's statement is not a compliance review. I would need the contract, retention period, sub-processors and training use. Under L, the scan stays on our server in Kazakhstan (known); we store it and set the retention period.

Verifiable safeguards: (1) a written retention rule for L with automatic deletion after a set number of days, and a monthly check that no scan is older; (2) a server access log reviewed monthly; (3) if H is ever used, a contract clause on retention and a monthly count of pages sent (expected 1,890 a day); (4) a monthly audit of 30 degraded RU and KK invoices.

Choice. L costs 21,600 a year against 33,611 for H and keeps personal data in Kazakhstan. The price is accuracy: about 19% of invoices wrong, 30% on degraded RU and KK scans. Residual risk: wrong extractions that staff must catch, and the security of our own server.

## Part 3: Explain your own numbers

### Q1

Number: 2.41
Why (at most 40 words): Input and output are priced differently (1 vs 6 per million). Kazakh input grows 2.96x but output only 1.63x, and output costs six times more per token, so the bill ratio (2.41) is below the token ratio (2.82).

### Q2

Number: 2.7 %
Why (at most 40 words): Only English is cut: 512 cl100k tokens become about 526 e5 tokens, over the 512 limit; Russian (256) and Kazakh (152) fit. The last 14 tokens are silently truncated and never embedded, so retrieval ignores them; no error appears.

### Q3

Number: 93.4 %
Why (at most 40 words): Lower T sharpens the distribution but keeps the ranking, because softmax preserves order. All five tokens keep non-zero probability, so the sampleable set is unchanged; the lower-ranked tokens just become much less likely.

### Q4

Number: 6
Why (at most 40 words): The public leaderboard (1 per correct answer) prefers X, 76 against 72. It rewards guessing over saying "I don't know", so models learn to guess. With the penalty, Y leads 58 to 52.

### Q5

Number: 17.80
Why (at most 40 words): 356 tokens per parameter against Chinchilla's 20, 17.8 times more: over-trained by that criterion, past compute-optimal, giving a smaller model that is cheaper to run. Chinchilla said GPT-3-like models were undertrained: too large for their data.
Why (at most 40 words):
