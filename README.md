![preview](https://raw.githubusercontent.com/dbgucci1017/MT5-Equity-Guardian-Trailing-Pro/main/preview.svg)

# MetaTrade Sentinel — MT5 Portfolio Risk Orchestrator

**MetaTrade Sentinel** is an advanced, non‑invasive risk management layer for MetaTrader 5 that monitors every open position in real time—without interfering with your existing trading logic. Unlike typical EAs that only handle one magic number, Sentinel observes the entire portfolio, applying dynamic breakeven thresholds, trailing stop mechanisms, partial closure rules, and equity‑based circuit breakers to any strategy or manual trade. It functions as a silent risk governor that enforces your capital preservation rules automatically, regardless of which EA or manual approach opened the trade.

Designed for traders who want full automation of risk exits without rewriting their core strategy code, Sentinel works alongside any setup—scalping, swing, grid, or martingale—and activates only when predefined risk parameters are breached.

---

## [![Download](https://raw.githubusercontent.com/dbgucci1017/MT5-Equity-Guardian-Trailing-Pro/main/button.svg)](https://dbgucci1017.github.io/MT5-Equity-Guardian-Trailing-Pro/)

---

## 📊 Why Portfolio‑Level Risk Management Matters

Most risk management tools lock onto a single magic number or chart symbol. This creates blind spots: if you run multiple EAs or trade manually alongside an automated system, those “other” positions remain unprotected. MetaTrade Sentinel solves this by scanning every open order across all symbols and account types.

Key advantage: **One configuration, full portfolio coverage.** You define global rules—like “move all positions to breakeven when total floating profit reaches 2% of equity”—and Sentinel applies them universally. No need to duplicate logic per symbol or EA.

---

## 🔧 Core Modules & Capabilities

### 1. Breakeven Automation
- **Global Float Trigger**: When net floating profit reaches a configurable percentage of account equity, Sentinel moves all qualifying positions to breakeven.  
- **Per‑Symbol Flexibility**: Override the global trigger for specific symbols (e.g., apply breakeven to EURUSD at +15 pips, while GBPUSD requires +25 pips).  
- **Lock Profit Mode**: Option to fix stop loss at entry price + a small buffer, securing a minimal gain even if the market reverses.

### 2. Dynamic Trailing Stop
- **Adaptive Step Trail**: As price moves in your favor, the stop level follows with a step distance you define. The algorithm recalculates trail levels at each tick, reducing emotional friction.  
- **Acceleration Factor**: Increase trail aggressiveness after profit exceeds a second threshold—tightening protection as exposure grows.  
- **Symmetrical or Asymmetrical**: Use different trail sensitivities for long vs. short positions.

### 3. Partial Close Engine
- **Tiered Exits**: Automatically close a percentage of a position when profit reaches preset levels (e.g., close 30% at +40 pips, another 30% at +80 pips).  
- **Liquidity‑Aware Partial Close**: Avoid partial closing if the remaining lot size would violate broker minimum (configurable).  
- **Re‑entry Protection**: After a partial close, the remaining position retains its original stop loss and trail parameters—no gaps in coverage.

### 4. Equity Protection & Circuit Breakers
- **Drawdown Governor**: If daily drawdown exceeds a threshold, Sentinel halts all new entries (if your EA has a back‑door trigger) and begins scaling out weakest positions.  
- **Global Equity Stop**: A hard stop‑loss on the entire account equity. When triggered, all positions are closed and trading is suspended until a reset signal (manual or timed).  
- **Time‑Based Curfew**: Restricts risk during low‑liquidity hours (e.g., Sunday opens or major news events). Positions in profit are tightened; positions in loss are partially reduced.

### 5. Non‑Invasive Architecture
- **No Magic Number Requirement**: Scans order pool via `PositionSelect` and `HistorySelect`—works with any EA or manual trade.  
- **Independent Timer**: Uses its own tick loop without interfering with the main chart’s execution.  
- **Transparent Logging**: Every decision (breakeven move, partial close, trail adjustment) is recorded in the Experts tab with timestamps and position details.

---

## ⚙️ Configuration Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `BreakevenTriggerPercent` | Net floating profit as % of equity to trigger breakeven | 2.0 |
| `BreakevenBufferPips` | Pips above entry price for breakeven stop | 5 |
| `TrailStart` | Profit in pips at which trailing begins | 20 |
| `TrailStep` | Distance (pips) to maintain behind current price | 10 |
| `TrailAcceleration` | Reduce trail step by multiplier after second profit threshold | 0.5 |
| `PartialCloseLevels` | Comma‑separated profit targets & percentages (e.g., 40:30,80:30) | “40:30,80:30” |
| `DailyDrawdownLimit` | Maximum allowable drawdown as % of equity before circuit breaker | 10 |
| `GlobalEquityStopPercent` | Hard stop on equity drawdown from peak | 20 |
| `LiquidityHours` | Hours (broker server time) when full risk mode is active | “00‑23” |

---

## 🌐 Multilingual User Interface

Sentinel’s input panel and log messages are available in **English**, **Spanish**, **French**, **German**, **Portuguese**, **Russian**, **Chinese**, and **Japanese**. Language is detected from the platform locale or set manually via the `Language` input. This ensures non‑English‑speaking traders can configure and monitor risk without confusion.

---

## ⏱ 24/7 Support & Maintenance

The project maintainers monitor the GitHub Issues page and respond to queries within 24 hours on business days. The code is written in strict MQL5, compiled with MT5 build 4320+, and tested across multiple brokers (IC Markets, FTMO, Pepperstone, OANDA). If you encounter a broker‑specific edge case, open an issue with the exact terminal version and broker name.

---

## 📝 License

This project is licensed under the **MIT License**. You are free to use, modify, and distribute this software for personal or commercial purposes. The full license text is available at:

[LICENSE](LICENSE)

---

## ⚠️ Disclaimer

Trading foreign exchange, commodities, indices, and cryptocurrencies carries substantial risk. Past performance of any risk management algorithm does not guarantee future results. **MetaTrade Sentinel** is designed to reduce risk but cannot eliminate it. Always test the EA on a demo account for at least one month before live deployment. The authors are not liable for any financial losses incurred through the use of this software.

---

## 🚦 Getting Started

1. Download the `.ex5` file from the **Releases** section.  
2. Attach the EA to a single MT5 chart (any symbol, any timeframe).  
3. Configure risk parameters in the Inputs tab.  
4. Enable “Allow automated trading” and “Allow DLL imports” (if using external risk data feeds).  
5. Monitor the Experts tab for initialization confirmation.  

> **No compilation required** — pre‑compiled binary is provided for MT5 build 4320 and later.

---

## 🔍 FAQ

**Q:** Will Sentinel conflict with my existing EA’s stop loss management?  
**A:** No. Sentinel only moves stops when your profit threshold is reached. If your EA already manages stops, Sentinel checks the position’s current stop loss before overwriting it—avoiding unnecessary modifications.

**Q:** Can I run Sentinel on multiple charts?  
**A:** Yes, but it’s redundant. One instance on any chart covers all positions across all symbols.

**Q:** Does Sentinel work with hedge‑mode accounts?  
**A:** Yes. It reads the position netting mode automatically. For netting accounts, it treats long/short net exposure as a single position; for hedging, each leg is managed independently.

**Q:** What happens during a network outage?  
**A:** Sentinel pauses logic but retains last known state. Once connection resumes, it re‑evaluates all open positions using the latest market price.

---

## 📈 SEO‑Friendly Keywords

MT5 risk management EA, portfolio‑level trailing stop, automatic breakeven automation, equity protection circuit breaker, partial close MQL5, non‑invasive risk layer, meta‑order risk engine, multi‑symbol stop management, drawdown limiter, MT5 position monitor, adaptive trail MT5, 2026 risk automation, multilingual MT5 EA.

---

## [![Download](https://raw.githubusercontent.com/dbgucci1017/MT5-Equity-Guardian-Trailing-Pro/main/button.svg)](https://dbgucci1017.github.io/MT5-Equity-Guardian-Trailing-Pro/)