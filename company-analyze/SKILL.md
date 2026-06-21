---
name: company-analyze
description: Analyze a listed company or business using Duan Yongping style investment logic from the user's investment Q&A PDF and public case studies of Duan's investments. Use when asked to research a company, stock, A-share, Hong Kong stock, US stock, or private business through gates such as "买股票就是买公司", "看不懂不投", business model, company culture, long-term net cash flow, moat, valuation, opportunity cost, buy/hold/sell discipline, and event-time case reconstruction. Supports web research for current or historical company data, filings, financials, news, and market price when needed.
---

# Company Analyze

## Core Rule

Analyze the company as a business, not as a ticker. Treat "看不懂不投" as a hard gate, not a caveat at the end. If the available evidence does not support understanding the business model, culture, future cash flow, and major risks, say so directly and stop short of an investment conclusion.

Default output language is Chinese unless the user asks otherwise.

## Required References

Read `references/duan-principles.md` before writing the analysis. Use it as the decision standard.

Read `references/duan-case-studies.md` when using Duan's own purchases as benchmarks or when the user asks whether a company "aligns" with his actual behavior.

Read `references/report-template.md` when producing a full company report.

## Workflow

1. Clarify the target company and market if ambiguous.
2. Decide whether the task is a current analysis or an event-time reconstruction.
   - Current analysis: use current filings, current price, current news, and current alternatives.
   - Event-time reconstruction: use only information available at the decision date or shortly before it. If Duan bought in 2024, analyze with 2024 filings, 2024 price, 2024 industry context, and 2024 opportunity cost; do not use later outcomes as evidence that the decision was good.
3. Gather evidence when the user asks for a current view, a public company, a market price, or a historical case.
   - Use official filings, exchange announcements, annual/interim reports, company investor relations, reputable financial data providers, and credible news.
   - Prefer primary sources for financial statements and management commentary.
   - Record source dates and report periods. Do not mix trailing, annual, and interim numbers without labeling them.
   - For Duan case comparisons, record the claimed buy date, position type, quoted reason, and source reliability. Never invent exact trade dates.
4. Build the "first-pass exclusion" case before the positive case.
   - Identify any reason to stop: incomprehensible business, poor or untrusted culture, heavy leverage, weak cash conversion, capital black hole, accounting opacity, customer/product deterioration, regulatory existential risk, or price justified only by market mood.
5. Analyze in this order:
   - Business model: how the company makes money, why customers pay, whether competitors can take the profit pool.
   - Company culture and management behavior: long-term health, consumer/customer orientation, honesty, capital allocation, short-term market catering.
   - Financial reality: revenue quality, real profit, net cash flow, debt, cash, capex, working capital, buybacks/dividends, ROE/ROA as supporting evidence.
   - Future net cash flow: describe the long-term drivers and fragility; do not fake precision with DCF math.
   - Moat: connect it to business model, culture, differentiation, switching cost, brand, scale, network effects, or cost structure.
   - Valuation and opportunity cost: compare the current price to the investor's alternatives and required return. "机会成本必须出现".
   - Decision discipline: buy, watch, hold, reduce, or pass must follow from understanding and price, not from recent price movement.
6. Produce a clear gate result:
   - `不懂，放弃`: key parts cannot be understood or verified.
   - `懂一点，继续观察`: promising but evidence is incomplete or price is not clearly attractive.
   - `基本看懂，但价格一般`: business is understandable, price is around fair value, opportunity cost is not compelling.
   - `看懂且明显便宜`: business is understandable and price appears materially below conservative business value.

## Output Standards

- Put the conclusion first, with confidence and the main reason.
- Always include "我是否真懂这家公司" as a section.
- Always include "不买/不投理由" before "买入理由".
- Always include opportunity cost and a named alternative: cash/T-bills, index fund, another company, or the user's stated benchmark.
- Separate facts, estimates, and judgments.
- Cite all web sources with links. If data cannot be verified, label it as unverified and lower confidence.
- Mark every Duan comparison with evidence quality: `strong public record`, `credible secondary report`, `from Q&A/PDF only`, or `unverified`.
- When reconstructing a past buy, include an "当时可见数据" section and an "事后结果不得倒推" note.
- Avoid formula worship. Simple valuation math is allowed, but explain the business assumptions first.
- Never recommend leverage, shorting, or derivatives unless the user explicitly asks for a risk discussion; even then, flag them as outside the core discipline.

## Fast Answer Mode

For a short user request, answer with:

1. 一句话结论
2. 看懂程度
3. 三个最关键的不投理由
4. 三个最关键的可研究理由
5. 机会成本
6. 是否与段永平案例对齐，以及证据等级
7. 下一步需要验证的数据

## Full Report Mode

Use the structure in `references/report-template.md`. Keep the report practical and decision-oriented. The report should make it easy for the user to decide whether to keep researching, wait, or reject the company.
