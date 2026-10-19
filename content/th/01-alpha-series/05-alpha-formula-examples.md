# Alpha Series 05: หน้าตาจริงของ Alpha (ตัวอย่างอธิบาย)

วันนี้แพรวจะพาดู **“หน้าตา” จริงของ Alpha**

เป้าหมายไม่ใช่ให้ท่องสูตร แต่เพื่อเห็นโครงสร้าง ความคิด และเหตุผลที่นักวิจัยเลือกเริ่มจากสิ่งที่เรียบง่าย

> แหล่งอ้างอิงหลัก:  
> - Kakushadze, Z. (2016). 101 Formulaic Alphas. *Wilmott Magazine*. arXiv:1601.00991  
> - Tulchinsky, I. et al. (2019). *Finding Alphas: A Quantitative Approach to Building Trading Strategies* (2nd ed.). Wiley.

---

## ตัวอย่างที่ 1: Mean-Reversion อย่างง่าย

**สูตร**

`−ln(today’s open / yesterday’s close)`

**คำอธิบาย**

- สัญญาณมีเครื่องหมายตรงข้ามกับผลตอบแทนของวันก่อนหน้า  
  (เครื่องหมายลบ (−) ในหลายสูตรมีไว้เพื่อทำให้เป็นแบบ mean-reversion)
- ถ้าวานราคาขึ้นมาก (open วันนี้สูงกว่า close เมื่อวาน) → สูตรให้ค่าติดลบ → สัญญาณ short
- ถ้าวานราคาลง → สูตรให้ค่าบวก → สัญญาณ long

นี่คือ **delay-0 alpha** ที่ paper ใช้เป็นตัวอย่างแรกของแนวคิด mean-reversion

### อธิบาย “Delay” เพิ่มเติม

ในงาน Kakushadze (2016) มีการแยกประเภทของ alpha ตาม **delay** ซึ่งหมายถึงระยะห่างระหว่างข้อมูลที่ใช้คำนวณกับเวลาที่เราจะเทรดจริง

| ประเภท   | ความหมาย                                                                 | ตัวอย่างการเทรด                  |
|----------|--------------------------------------------------------------------------|----------------------------------|
| **delay-0** | ข้อมูลล่าสุดที่ใช้ในสูตร เกิดขึ้นในวันเดียวกับที่เราตั้งใจจะเทรด       | เทรดใกล้เคียงกับเวลาเปิดตลาดวันนี้ |
| **delay-1** | ข้อมูลล่าสุดที่ใช้ในสูตร เกิดขึ้นวันก่อนหน้าที่เราจะเทรด               | ใช้ข้อมูลของวานนี้ทั้งหมด แล้วเทรดวันนี้ |

**การเข้าใจ delay สำคัญมาก** เพราะถ้าใช้ข้อมูลผิดจังหวะ จะกลายเป็น **look-ahead bias** (ใช้ข้อมูลอนาคตโดยไม่ตั้งใจ)

---

## ตัวอย่างที่ 2: Momentum อย่างง่าย

**สูตร**

`ln(yesterday’s close / yesterday’s open)`

**คำอธิบาย**

- แนวคิดคือ “ถ้าหุ้นวานขึ้น (หรือลง) แนวโน้มมีโอกาสดำเนินต่อในวันนี้”
- ถ้าราคาปิดวานสูงกว่าราคาเปิดวาน → ค่าบวก → สัญญาณ long
- ถ้าราคาปิดวานต่ำกว่าราคาเปิดวาน → ค่าลบ → สัญญาณ short

เป็น **delay-1 alpha** ที่ paper ยกเป็นตัวอย่างคู่กับ mean-reversion

---

## ตัวอย่างที่ 3: Alpha#101

**สูตร**

`(close − open) / (high − low + 0.001)`

**คำอธิบาย**

- ตัวเศษ `(close − open)` = การเปลี่ยนแปลงราคาภายในวัน
- ตัวส่วน `(high − low + 0.001)` = ช่วงราคาของวัน (บวก 0.001 เพื่อกันหารด้วยศูนย์)
- ถ้า close > open → สูตรให้ค่าบวก → สัญญาณ long ในวันถัดไป
- ถ้า close < open → สูตรให้ค่าลบ → สัญญาณ short

paper ระบุชัดว่านี่คือ **delay-1 momentum alpha** ที่เรียบง่ายและถูกใช้เป็นตัวอย่างบ่อยครั้ง

---

## ตัวอย่างที่ 4: การปรับปรุงด้วย Rank (จาก Finding Alphas)

**สูตรเริ่มต้น**

`five_days_return`

**สูตรหลังปรับปรุง**

`rank(five_days_return)`

### ทำไมต้องใช้ Rank?

เกิดจากปัญหาของสูตรแบบเดิม (ก่อนใช้ Rank):

1. หุ้นบางตัวมีราคาสูงมาก ผลตอบแทน 5% อาจหมายถึงกำไรเป็นเงินจำนวนมาก
2. หุ้นบางตัวมีราคาต่ำ ผลตอบแทน 5% อาจหมายถึงกำไรเป็นเงินน้อย
3. ขนาดของตัวเลข (magnitude) จึงไม่สะท้อนความแข็งแกร่งของสัญญาณได้ดีเท่าที่ควร

*Finding Alphas* ชี้ไว้ว่า **relative size มักแม่นยำกว่า magnitude** ของ raw return

จึงใช้วิธี **Cross-sectional Rank** คือการ “จัดอันดับ” ค่าของทุกหุ้นในวันเดียวกัน

### ตัวอย่างตาราง Rank

สมมติมีหุ้น 5 ตัวในวันหนึ่ง:

| หุ้น | ผลตอบแทน 5 วัน | Rank |
|------|----------------|------|
| A    | +5.2%          | 1    |
| B    | +3.1%          | 2    |
| C    | +0.8%          | 3    |
| D    | –1.2%          | 4    |
| E    | –4.5%          | 5    |

- หุ้น A ได้ Rank = 1 (สูงสุด)
- หุ้น E ได้ Rank = 5 (ต่ำสุด)

**สูตรหลังปรับปรุง** จึงกลายเป็น:

`Alpha ใหม่ = rank(five_days_return)`

### ข้อดีของการใช้ Rank

1. ให้ความสำคัญระหว่าง “หุ้นตัวไหนดีกว่าหุ้นตัวอื่น” มากกว่า “ผลตอบแทนเป็นกี่เปอร์เซ็นต์”
2. ช่วยตัดปัญหาเรื่องขนาดของราคาที่ต่างกันมาก
3. ทำให้สัญญาณเปรียบเทียบข้ามหุ้นได้ชัดเจนและเสถียรกว่า

หนังสือเรียกวิธีนี้ว่า **Cross-sectional Rank** และแนะนำให้ใช้เพื่อทำให้ Alpha แม่นยำขึ้น

---

## ทำไมต้องเริ่มจากสูตรง่าย?

ทุกสูตรด้านบนแพรวเรียงลำดับจากความเรียบง่ายจากบนลงล่าง โดยอ้างอิงจาก Kakushadze (2016) และ *Finding Alphas*

หนังสือ *Finding Alphas* ระบุไว้ว่า:

> “The idea and expression is simple”

หรือ **Alpha ที่ดี ควรมีแนวคิดและการแสดงออกมีความเรียบง่าย**

Kakushadze (2016) ก็เริ่มอธิบายด้วยสูตร mean-reversion และ momentum ง่าย ๆ ก่อน เพราะเหมาะสำหรับเริ่มต้นทำความเข้าใจ

---

## สรุป

การคิดหรือหา Alpha ควรเริ่มจากความเรียบง่าย และความสะอาดในการเขียนสูตร  

จากที่แพรวเขียนเรื่อง Alpha มาทุกหัวข้อ ก็จะมีเรื่อง **simplicity** เข้ามาเกี่ยวข้องเสมอ

เริ่มจากสูตรง่าย ๆ ทำความเข้าใจโครงสร้างและความคิดเบื้องหลัง  
แล้วค่อยปรับปรุงให้แข็งแรงขึ้นทีละขั้น โดยไม่หลงเชื่อผล backtest มากเกินไป

---

## แหล่งอ้างอิงหลัก

1. Kakushadze, Z. (2016). 101 Formulaic Alphas. *Wilmott Magazine*, 2016(84), 72–80.  
   https://arxiv.org/abs/1601.00991

2. Tulchinsky, I. et al. (2019). *Finding Alphas: A Quantitative Approach to Building Trading Strategies* (2nd ed.). Wiley.


---

# Alpha Series 05: ALPHA FORMULA INVENTORY (20 สูตร)

> แหล่งอ้างอิงหลัก:  
> - **K** = Kakushadze, Z. (2016). 101 Formulaic Alphas. *Wilmott Magazine*. arXiv:1601.00991  
> - **FA** = Tulchinsky, I. et al. (2019). *Finding Alphas: A Quantitative Approach to Building Trading Strategies* (2nd ed.). Wiley.

---

# Alpha Series 05: ALPHA FORMULA INVENTORY (20 สูตร)

> แหล่งอ้างอิงหลัก:  
> - **K** = Kakushadze, Z. (2016). 101 Formulaic Alphas. *Wilmott Magazine*. arXiv:1601.00991  
> - **FA** = Tulchinsky, I. et al. (2019). *Finding Alphas: A Quantitative Approach to Building Trading Strategies* (2nd ed.). Wiley.

---

## รายการสูตร 20 ตัว

| #  | สูตร | สมมติฐานโดยย่อ | แหล่ง |
|----|------|----------------|-------|
| 1  | `−ln(today's open / yesterday's close)` | Mean-reversion (delay-0) | K สมการ (2) |
| 2  | `ln(yesterday's close / yesterday's open)` | Momentum (delay-1) | K สมการ (3) |
| 3  | `(close − open) / ((high − low) + .001)` | Alpha#101 – delay-1 momentum | K Alpha#101 |
| 4  | `1/price` | Invest more if price is low | FA บทที่ 1 Table 1.2 |
| 5  | `price − delay(price, 3)` | Price moves in the direction of 3-day change | FA บทที่ 1 Table 1.2 |
| 6  | `−1 * correlation(open, volume, 10)` | Alpha#6 | K Alpha#6 |
| 7  | `−1 * correlation(rank(open), rank(volume), 10)` | Alpha#3 (ใส่ rank) | K Alpha#3 |
| 8  | `−1 * Ts_Rank(rank(low), 9)` | Alpha#4 | K Alpha#4 |
| 9  | `(price/delay(price, 3)) * rank(volume)` | Trending stocks with increasing volume outperform | FA บทที่ 1 Table 1.2 |
| 10 | `−1 * correlation(rank(delta(log(volume), 2)), rank(((close − open)/open)), 6)` | Alpha#2 | K Alpha#2 |
| 11 | `rank((open − (sum(vwap, 10)/10))) * (−1 * abs(rank((close − vwap))))` | Alpha#5 | K Alpha#5 |
| 12 | `rank(−(close − delay(close, 5)) / delay(close, 5))` | Cross-sectional Rank + Industry Neutral | FA บทที่ 5 |
| 13 | `rank(((((−1 * returns) * adv20) * vwap) * (high − close)))` | Alpha#25 | K Alpha#25 |
| 14 | `(rank(Ts_ArgMax(SignedPower(((returns < 0) ? stddev(returns, 20) : close), 2.), 5)) − 0.5)` | Alpha#1 | K Alpha#1 |
| 15 | `((adv20 < volume) ? ((−1 * ts_rank(abs(delta(close, 7)), 60)) * sign(delta(close, 7))) : (−1 * 1))` | Alpha#7 | K Alpha#7 |
| 16 | `−1 * ts_max(correlation(ts_rank(volume, 5), ts_rank(high, 5), 5), 3)` | Alpha#26 | K Alpha#26 |
| 17 | `scale(((correlation(adv20, low, 5) + ((high + low)/2)) − close))` | Alpha#28 | K Alpha#28 |
| 18 | `((0.5 < rank((sum(correlation(rank(volume), rank(vwap), 6), 2)/2.0))) ? (−1 * 1) : 1)` | Alpha#27 | K Alpha#27 |
| 19 | `min(product(rank(rank(scale(log(sum(ts_min(rank(rank((−1 * rank(delta((close − 1), 5))))), 2), 1))))), 1), 5) + ts_rank(delay((−1 * returns), 6), 5)` | Alpha#29 | K Alpha#29 |
| 20 | Industry Neutralization (operator) | demean แบบ cross-sectional ภายในกลุ่มอุตสาหกรรม | K Appendix A.2 |

---

## หมายเหตุสำคัญ

> **สูตรทั้งหมดด้านบนเป็นตัวอย่างจากเอกสารต้นฉบับเพื่อการศึกษาเท่านั้น**  
> **ไม่ใช่คำแนะนำในการเทรดจริง** และยังไม่ได้ผ่านการทดสอบในบริบทของเรา

- สูตรส่วนใหญ่เป็น **price-volume based**
- หลายสูตรใช้ `rank`, `delay`, `correlation`, `ts_rank` เป็น building blocks
- การเข้าใจ **delay** สำคัญมาก เพื่อหลีกเลี่ยง look-ahead bias

---

## แหล่งอ้างอิงหลัก

1. Kakushadze, Z. (2016). 101 Formulaic Alphas. *Wilmott Magazine*, 2016(84), 72–80.  
   https://arxiv.org/abs/1601.00991

2. Tulchinsky, I. et al. (2019). *Finding Alphas: A Quantitative Approach to Building Trading Strategies* (2nd ed.). Wiley.
