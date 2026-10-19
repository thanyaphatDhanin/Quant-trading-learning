# Alpha Series 06 : Alpha-properties 01: Information Ratio (IR) และ Information Coefficient (IC)

สวัสดีค่ะ นี่เป็นหัวข้อที่หกของซีรีส์ **Alpha**

บทนี้อธิบายตัวชี้วัดสำคัญสองตัวที่ใช้ประเมินคุณภาพของ Alpha คือ **Information Ratio (IR)** และ **Information Coefficient (IC)**

> แหล่งอ้างอิงหลัก:  
> Tulchinsky, I. et al. (2015 & 2019). *Finding Alphas: A Quantitative Approach to Building Trading Strategies*. Wiley.  
> (บท Introduction to Alpha Design โดย Igor Tulchinsky และบท Turnover โดย Pratik Patel)

---

## 1. Information Ratio (IR) คืออะไร?

ในหนังสือ *Finding Alphas* โดย Igor Tulchinsky ระบุว่า:

> “The ratio of an alpha’s daily return to daily volatility is called the information ratio.  
> This ratio measures the strength and steadiness of the signal, and shows if a strategy is working – whether the signal is robust or weak, whether it is likely to be a true signal or largely noise.”

และในบท Turnover โดย Pratik Patel อธิบายเพิ่มเติมว่า:

> “The IR is the ratio of excess returns over a benchmark to the variability of those returns; the idea behind it is that an alpha with high excess returns and low variability consistently predicts future returns over a given time period.”

**แปลเป็นภาษาไทยให้เข้าใจง่าย**

**Information Ratio (IR)** คืออัตราส่วนที่บอกว่า Alpha ให้ผลตอบแทนได้มากแค่ไหน เมื่อเทียบกับความผันผวนของผลตอบแทนนั้น

### สูตรพื้นฐาน

IR = Average Daily Return ÷ Daily Volatility

- Average Daily Return = ผลตอบแทนเฉลี่ยรายวันของ Alpha  
- Daily Volatility = ความผันผวนรายวันของผลตอบแทน Alpha

### ความหมายของค่า IR

- **IR สูง** → Alpha ให้กำไรได้สม่ำเสมอ และความผันผวนของกำไรนั้นต่ำ  
- **IR ต่ำ** → กำไรไม่สม่ำเสมอ หรือมีความผันผวนสูงเมื่อเทียบกับกำไรที่ได้

### Annualized IR

Annualized IR = Daily IR × √N

| ค่า N | ความหมาย | ใช้ใน |
|-------|----------|-------|
| √252  | ปีหนึ่งมีประมาณ 252 วันทำการ | ตลาดหุ้นส่วนใหญ่ (นิยมที่สุด) |
| √256  | ปีหนึ่งมีประมาณ 256 วันทำการ | บางระบบ รวมถึง WebSim / WorldQuant BRAIN |

**ต้องระบุให้ชัดเจน** ว่าใช้ค่าไหน และใช้ให้สม่ำเสมอ

### ทำไมต้องคูณด้วย √252 หรือ √256?

เพราะความผันผวนสะสมตามเวลาแบบรากที่สองของเวลา ไม่ใช่แบบเส้นตรง

---

## 2. Information Coefficient (IC) คืออะไร?

จากหนังสือ *Finding Alphas* บท Turnover:

> “The IC measures the correlation between the predicted and actual values, in which a value of 1.0 represents perfect forecasting ability.”

**IC** คือค่าสหสัมพันธ์ระหว่างค่าที่ Alpha ทำนาย กับผลตอบแทนจริงที่เกิดขึ้นภายหลัง

IC = Correlation(Alpha Forecast, Realized Return)

| ค่า IC | ความหมาย |
|--------|----------|
| 1.0 | ทำนายสมบูรณ์แบบ (ยากมาก) |
| 0.02 – 0.10 | ช่วงที่ดีในงาน Quant ทั่วไป |
| ใกล้ 0 | สัญญาณแทบไม่มีพลัง |
| ติดลบ | ทำนายผิดทิศทาง |

---

## ความแตกต่างระหว่าง IR กับ IC

| ตัวชี้วัด | วัดอะไร |
|----------|---------|
| **IC** | ความแม่นยำของสัญญาณ |
| **IR** | ความสม่ำเสมอของผลตอบแทนที่ได้จากสัญญาณ |

---

## ข้อระวังสำคัญ

IR และ IC ที่คำนวณในขั้นตอนวิจัยมักอยู่ภายใต้สมมติฐานอุดมคติ (ไม่มีต้นทุน ไม่มีข้อจำกัดสภาพคล่อง)  
ดังนั้นค่าที่สูงใน backtest **ยังไม่ได้รับประกัน** ผลลัพธ์จริง ควรดูควบคู่กับ Turnover และต้นทุนเสมอ

---

## สรุปสั้น ๆ

- **IC** = วัดความแม่นยำของสัญญาณ  
- **IR** = วัดความแข็งแรงและความสม่ำเสมอของผลตอบแทน  

ทั้งสองเป็นตัวชี้วัดพื้นฐานที่สำคัญ แต่ส่วนใหญ่ยังเป็นตัวเลข “ก่อนหักต้นทุน”

---

## ตัวอย่างการคำนวณ (เพื่อการศึกษา)

> ตัวอย่างทั้งหมดเป็นตัวเลขสมมติเพื่อให้เห็นภาพเท่านั้น

### 1. ตัวอย่าง Information Coefficient (IC)

| หุ้น | Alpha Forecast | Realized Return (%) |
|------|----------------|---------------------|
| A    | 0.85           | +1.60               |
| B    | 0.65           | +0.90               |
| C    | 0.40           | +0.20               |
| D    | 0.25           | -0.30               |
| E    | 0.10           | -0.80               |

Pearson Correlation ≈ 0.97  
Spearman Rank Correlation = 1.0  

ในโลกจริง ค่า IC ที่ดีและเสถียรมักอยู่แค่ 0.02–0.10 เท่านั้น

### 2. ตัวอย่าง Information Ratio (IR)

สมมติผลตอบแทนรายวัน 5 วัน:

| วัน | ผลตอบแทนรายวัน (%) |
|-----|--------------------|
| 1   | +0.45              |
| 2   | +0.30              |
| 3   | -0.10              |
| 4   | +0.55              |
| 5   | +0.25              |

Average Daily Return ≈ 0.29%  
Daily Volatility (ตัวอย่าง) ≈ 0.25%  
Daily IR ≈ 1.16  
Annualized IR ≈ 1.16 × √252 ≈ 18.4  

ตัวเลขสูงมากเพราะข้อมูลสั้นมาก ในงานจริง IR รายปีของ Alpha ที่ดีมักอยู่ประมาณ 0.5–2.0

### 3. Fundamental Law of Active Management

IR ≈ IC × √Breadth

- IC = ความแม่นยำของสัญญาณ  
- Breadth = จำนวนการเดิมพันที่เป็นอิสระต่อกันต่อปี  

ตัวอย่าง: IC = 0.04, Breadth = 150 → √150 ≈ 12.25 → IR ≈ 0.49

เวอร์ชันที่สมบูรณ์ขึ้น:

IR ≈ TC × IC × √Breadth

(TC = Transfer Coefficient)

---

## วิธีใช้จริงในงาน Quant

- Rank IC (Spearman) ใช้มากกว่า Pearson เพราะสนใจลำดับมากกว่าขนาด และทนต่อ outlier ได้ดีกว่า  
- IC เป็น Time Series → คำนวณข้ามหน้าตัดทุกวัน แล้วดู Mean IC, Std(IC), ICIR = Mean IC / Std(IC)  
- Horizon สำคัญมาก ต้องเลือกให้สอดคล้องกับ holding period

---

## ข้อควรระวัง

- ตัวอย่างสั้น ๆ ทำให้ IC และ IR สูงเกินจริงได้ง่าย  
- IC ที่คำนวณจากข้อมูลน้อย หรือมี overlapping returns จะมี bias  
- Breadth ที่นับจากจำนวนหุ้นล้วน ๆ มักสูงเกินจริง  
- IC สูงใน in-sample ไม่ได้รับประกัน out-of-sample  
- IR ที่รายงานโดยไม่บอก cost และ constraint จะดูดีเกินจริง

---

## สรุปภาพรวม

- **IC** บอก “สัญญาณแม่นแค่ไหน”  
- **Breadth** บอก “เราเอาไปใช้ได้บ่อยและกว้างแค่ไหน”  
- **IR** คือผลลัพธ์ที่เราสนใจจริงในระดับพอร์ต  

---

## แหล่งอ้างอิงหลัก

1. Tulchinsky, I. (2015/2019). Introduction to Alpha Design. In *Finding Alphas*. Wiley.  
2. Patel, P. (2015/2019). Turnover. In *Finding Alphas*. Wiley.
