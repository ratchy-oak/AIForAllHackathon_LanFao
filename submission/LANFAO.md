
# หลานเฝ้า (Laan Fao) — ระบบเฝ้าระวังสุขภาพผู้สูงอายุด้วยเรดาร์คลื่นมิลลิเมตร

AI for All Thailand Innovation Sprint · เสาหลัก Health & Aging Society · ทีม หยินหยาง หมางเมิน · สร้างด้วย Amazon Quick AI · 2026-11-04 · v7

---

## 1. ภาพรวมโครงการ

หลานเฝ้ามุ่งหวังเปลี่ยนการดูแลผู้สูงอายุในชนบทไทยจากระบบ "รู้เมื่อล้มแล้ว" ไปสู่ "เห็นแนวโน้มก่อนล้ม" ด้วยเรดาร์คลื่นมิลลิเมตรที่ไม่ใช้กล้องติดตั้งภายในบ้าน ตรวจวัดข้อมูลสุขภาพเชิงพฤติกรรมอย่างต่อเนื่อง แปลงเป็นสัญญาณจัดลำดับความเร่งด่วน แล้วส่งเข้าคิวให้อาสาสมัครสาธารณสุขประจำหมู่บ้าน (อสม.) เยี่ยมบ้านเชิงรุก [source: participant]

ทีม 4 คน (วิศวกรรมคอมพิวเตอร์ 3 หุ่นยนต์ 1) สถานะ: แนวคิด ไม่มีต้นแบบ สมมติฐานอนุมัติ 2026-11-04 เพื่อทดสอบ [source: participant]

*Concept-stage ambient radar system routing health risk scores to community volunteer visit queues — no prototype exists.*

---

## 2. คำแถลงปัญหา

> "ระบบสุขภาพไทยจะรู้จักผู้สูงอายุคนหนึ่ง ก็ต่อเมื่อเขาล้มแล้ว ป่วยแล้ว หรือมาถึงโรงพยาบาลแล้ว" — ผู้เข้าร่วม [source: participant, verbatim]

ปัญหาที่ผู้เข้าร่วมอธิบายมีสามด้านหลัก [source: participant]

1. **ภาพนิ่งแทนแนวโน้ม** — การตรวจ ADL และ Timed Up and Go ปีละ 1–2 ครั้ง เห็นแค่ภาพชั่วขณะ ไม่เห็นแนวโน้มการเสื่อมถอย
2. **เยี่ยมแบบหว่านแห** — อสม. เยี่ยมทุกบ้านเท่ากัน ไม่มีข้อมูลจัดลำดับว่าบ้านไหนเร่งด่วน
3. **อุปสรรคเทคโนโลยี** — Smartwatch หรือ Smart Home อาจมีอุปสรรคด้านการชาร์จ สมาร์ตโฟน Wi-Fi และทักษะดิจิทัล

*Participant observes that elderly decline is often detected only after adverse events; existing tools may face rural access barriers.*

---

## 3. โซลูชันและโมเดลธุรกิจ

**คำศัพท์ที่ใช้ในเอกสาร:** ความเร็วการเดิน (gait speed) หน่วย m/s · เวลาลุกจากที่นั่ง (sit-to-stand, STS) หน่วยวินาที · การเคลื่อนไหวกลางคืน (night movement) หน่วยครั้งต่อคืน · ค่าพื้นฐาน (baseline) คือค่าอ้างอิงเฉพาะบุคคลจากข้อมูลช่วงแรก [source: reviewer]

**วงจรการทำงาน 4 ชั้น (Sense → Predict → Act → Connect):** เรดาร์ 60 GHz ในบ้าน ประมวลผลบนตัวเครื่อง ส่งค่าสรุป (ความเร็วการเดิน เวลาลุกจากที่นั่ง รูปแบบกลางคืน ตรวจจับการล้ม) ผ่าน NB-IoT → cloud สร้างค่าพื้นฐาน วิเคราะห์แนวโน้ม → สัญญาณ 🟢🟡🔴 → อสม. คิวเยี่ยม → ครอบครัว LINE OA [source: participant]

**ข้อกำหนดสำคัญ:** ไม่มีระบบสั่งการฉุกเฉินอัตโนมัติ (C7) · สัญญาณชีพไม่อยู่ใน MVP (C6) · ค่าขีดแบ่งทุกค่าเป็นข้อเสนอที่ยังไม่ผ่านการพิสูจน์ (C8) · การเคลื่อนไหวกลางคืนเป็นรูปแบบเชิงพฤติกรรม ไม่ใช่การวินิจฉัย (C19) [source: participant + reviewer]

**ทางเลือกฮาร์ดแวร์ (ข้อแลกเปลี่ยน ไม่ใช่คำแนะนำ):**

| ทางเลือก | จุดเด่น | ข้อแลกเปลี่ยน |
|---|---|---|
| TI IWR6843 Eval Kit | SDK พร้อม เริ่มเร็ว | ขนาดใหญ่ |
| OEM Module | ขนาดเล็กกว่า | SDK ขึ้นกับผู้ผลิต |
| Custom PCB | เต็มรูปแบบ | ต้อง RF design นาน เหมาะผลิตจำนวนมาก |

เริ่ม Eval Kit เฟส 0 [source: participant]

**โมเดลธุรกิจ (ทุกช่องทางเป็นสมมติฐาน):**

| ช่องทาง | ลูกค้า/ผู้จ่าย | มูลค่าที่เสนอ | สถานะ |
|---|---|---|---|
| B2G | รพ.สต./อปท. → งบท้องถิ่น | 150–250 บาท/เดือน/บ้าน | สมมติฐาน (A11) |
| B2C Family | ครอบครัว | ดูแลทางไกล | ยังไม่ทราบความเต็มใจจ่าย |
| B2B Enterprise | ประกัน/senior living | ลดเคลม | ยังไม่เจรจา |
| กองทุน LTC | สปสช. | เฝ้าระวังต่อเนื่อง | ไม่ยืนยัน (A5) |
| True partnership | Revenue sharing | NB-IoT ร่วม | โอกาส (A10, C4) |

ต้นทุนวัสดุประมาณ 2,000–3,000 บาทต่อหน่วย ค่า NB-IoT/cloud/ติดตั้ง/certification ไม่ทราบ จุดคุ้มทุนคำนวณไม่ได้ ต้องผ่าน gate ทุกเฟส รวมแผนฝึก อสม. และบำรุงรักษา (R8) [source: assumption]

*Four-layer sense-predict-act-connect loop with all pricing as untested hypotheses.*

---

## 4. พารามิเตอร์ที่ยืนยัน

คำว่า "ยืนยัน" หมายถึงผู้เข้าร่วมระบุไว้ ไม่ใช่การพิสูจน์ทางวิทยาศาสตร์ [source: participant]

| พารามิเตอร์ | ค่า | หมายเหตุ |
|---|---|---|
| เรดาร์ | 60 GHz (TI IWR6843) | ผู้เข้าร่วมเลือก |
| ตัวชี้วัด MVP | ความเร็วการเดิน, เวลาลุกจากที่นั่ง, กลางคืน, ล้ม | สัญญาณชีพไม่อยู่ใน MVP (C6) |
| สัญญาณสี | 🟢🟡🔴 | ค่าขีดแบ่งยังไม่พิสูจน์ (C8) |
| การเชื่อมต่อ | NB-IoT เป็น transport | ความครอบคลุมไม่ยืนยัน (A2) |
| ฉุกเฉิน | มนุษย์ตัดสินใจเท่านั้น | ไม่มีระบบอัตโนมัติ (C7) |
| Pilot | เฟส 0: 2–3 บ้านจำลอง → เฟส 1: 15–25 บ้าน | เป้า 100 บ้านระยะยาว |
| False alarm | ไม่เกิน 1 ครั้ง/บ้าน/สัปดาห์ | proposed |
| Sensitivity | อย่างน้อย 90% | proposed |
| เยี่ยมเหลือง | ภายใน 72 ชั่วโมง เป้า 80% | proposed |

*All thresholds are proposed targets — none have been tested or validated.*

---

## 5. ภูมิทัศน์การแข่งขัน

| ผลิตภัณฑ์ | ประเภท | หมายเหตุ |
|---|---|---|
| Vayyar Care | เรดาร์ 60 GHz ตรวจล้ม+presence | ใช้ใน senior living ต่างประเทศ ราคาไม่ทราบ |
| Walabot Home | เรดาร์ 3D ครอบคลุม 1 ห้อง | ตัวอย่างในอดีต ความพร้อมจำหน่ายปัจจุบันไม่ทราบ |
| SEMAS (สิงคโปร์) | เซ็นเซอร์แวดล้อม PIR/ประตู/เตียง | ที่มาหน่วยงานไม่ยืนยัน แยกจาก True |
| True/Intel | นำร่องดูแลผู้สูงอายุไทย | รายละเอียดจากแหล่งสาธารณะไม่ยืนยัน (C11) |
| Apple Watch | Wearable ตรวจล้ม | อุปสรรคชาร์จ ราคา ทักษะ |
| Garmin | Wearable สุขภาพ | ผู้เข้าร่วมกล่าวถึง spec ไม่ทราบ |

ความแตกต่างที่เสนอคือการเชื่อมสัญญาณเรดาร์เข้าคิวจัดลำดับเยี่ยมบ้านของ อสม. แบบครบวงจร ในการค้นหาไม่พบตัวอย่าง แต่ขอบเขตการค้นหามีจำกัด [source: research report, participant]

*Six competitors mapped; proposed differentiation is the radar-to-visit-queue closed loop.*

---

## 6. การประเมินความเป็นไปได้

| มิติ | ระดับ | สรุป |
|---|---|---|
| เทคนิค | 🟡 | SDK มี แต่ยังไม่ได้ยืนยันความแม่นของระบบนี้ในบ้านจริง; multi-person ยาก (C10) [source: research report] |
| ตลาด | 🟡 | ปัญหาชัด ยังไม่พิสูจน์ความต้องการจริง [source: participant] |
| การเงิน | 🔴 | ต้นทุนเป็นประมาณการ รายได้เป็นสมมติฐาน LTC ไม่ยืนยัน [source: assumption] |
| ปฏิบัติการ | 🟡 | ทีม 4 คน เฟส 0 ได้ เฟส 1 ต้องพันธมิตร True เป็นโอกาส (C4) [source: assumption] |
| กฎหมาย | 🟡 | อย. ไม่ชัดเจน (C5) PDPA consent ต้อง privacy review [source: assumption] |
| ทรัพยากร | 🟡 | เฟส 0 ประเมินทักษะ เฟส 1 ต้องที่ปรึกษา (มีเงื่อนไขตามผลเฟส 0) [source: assumption] |
| งบประมาณ | 🔴 | ไม่ระบุทุกเฟส [source: assumption] |

ORCATECH (Oregon) เป็นงานเชิงสังเกตใช้ PIR สนับสนุนแนวคิดวัดความเร็วการเดินต่อเนื่อง แต่ไม่ validate mmWave DOI ไม่ตรวจสอบ (C21) เกณฑ์ 0.1 m/s มาจากวรรณกรรมวัดระยะหลายเดือน การใช้ใน 3–4 สัปดาห์เป็น extrapolation ไม่ validated (C8) [source: research report]

*Seven dimensions: two red (finance, budget), five yellow; ORCATECH supports concept but does not validate our technology.*

---

## 7. หน้าจอ ขั้นตอน และ Demo

**คำศัพท์เพิ่มเติม:** ความเร็วการเดิน แสดงเป็น m/s · การเคลื่อนไหวกลางคืน แสดงเป็นครั้งต่อคืน · เวลาลุกจากที่นั่ง แสดงเป็นวินาที [source: reviewer]

**การออกแบบ:** editorial magazine, SVG ต้นฉบับ, navy/ivory + teal/coral, จังหวะกว้าง เมนูย่อได้ progress bar ก่อน/ถัดไป 375px+ focus state prefers-reduced-motion ตารางขยาย/ยุบ [source: reviewer]

**แดชบอร์ด อสม. (4 หน้าจอ):** [source: participant]

- **(A1) คิวจัดลำดับ** — แสดง 6 เคสเรียงจาก 🔴 → 🟡 → 🟢 กรองด้วย 4 ปุ่ม ข้อมูลทั้งหมดเป็นข้อมูลจำลอง
- **(A2) โปรไฟล์ผู้สูงอายุ** — กราฟ 28 วัน (LF-006 = 7 วัน ค่าพื้นฐานตนเอง) 3 ตัวชี้วัด แกนมีหน่วยและ legend ค่าพื้นฐาน = mean 7 จุดแรก ± ค่าเผื่อคงที่ กล่อง "ทำไมจัดลำดับนี้?" factors เป็นตัวอย่าง ป้าย "สีไม่ใช่การรับประกันทางคลินิก"
- **(A3) ขั้นตอนเยี่ยมบ้าน** — ยืนยัน/ยกเลิก → checklist 6 รายการ (ประเมินสภาพ · รวบรวมรายการยาเพื่อส่งต่อ · ความปลอดภัยในบ้าน · แนะนำออกกำลังกายตามแผนผู้เชี่ยวชาญ · ส่งต่อ รพ.สต. · อื่นๆ) + บันทึกบังคับ → validation (เลือกอย่างน้อย 1 รายการ + ข้อความไม่ว่าง) → สรุปประวัติ อสม. รวบรวมข้อมูล บุคลากรคลินิกตัดสินใจการรักษา draft ล้างเมื่อเปลี่ยนเคส
- **(A4) ประวัติการเยี่ยม** — การบันทึกเยี่ยมไม่เปลี่ยนสถานะสี สีเปลี่ยนจากข้อมูลเซ็นเซอร์เท่านั้น

**ครอบครัว LINE OA:** สถานะผูกกับเคสที่เลือก (ไม่แสดง 🟢 เมื่อจริงเป็น 🔴) ไม่มีปุ่มฉุกเฉิน (C7) [source: participant]

**พยาบาล รพ.สต.:** aggregate จากชุดข้อมูลเดียวกัน เช่น 1🔴 3🟡 2🟢 สอดคล้องกับข้อมูลที่ อสม. และครอบครัวเห็น [source: participant]

**ข้อมูลจำลอง 6 เคส** (ไม่มี network, persistence, ข้อมูลจริง หรือสัญญาณฉุกเฉิน):

| รหัส | นามแฝง | อายุ | สี | ความเร็วการเดิน (m/s) | กลางคืน (ครั้ง/คืน) | ลุกจากที่นั่ง (วินาที) | เหตุการณ์ |
|---|---|---|---|---|---|---|---|
| LF-001 | ยายสมศรี | 78 | 🔴 | 0.72 → 0.58 | 3 → 5 | 12 → 15 | ล้มวันที่ 27 |
| LF-002 | ตาประสิทธิ์ | 82 | 🟡 | 0.65 → 0.51 | 2 → 3 | 14 → 17 | ความเร็วการเดินลดลง |
| LF-003 | ยายบุญมี | 71 | 🟡 | 0.80 → 0.76 | 4 → 7 | 11 → 12 | กลางคืนเพิ่มกะทันหัน |
| LF-004 | ตาสมชาย | 75 | 🟢 | 0.85 → 0.83 | 2 → 2 | 10 → 10 | ปกติ |
| LF-005 | ยายนวล | 88 | 🟡 | 0.60 → 0.57 | 3 → 4 | 14 → 19 | เวลาลุกจากที่นั่งเพิ่ม |
| LF-006 | ตาเฉลิม | 69 | 🟢 | 0.90 → 0.89 | 1 → 1 | 9 → 9 | ค่าพื้นฐาน 7 วัน |

**สูตร:** value(d) = start + (end−start) × (d−1)/27 สำหรับ d=1 ถึง 28 ยกเว้น LF-001 ล้มเป็นค่าไม่ต่อเนื่องวันที่ 27 · LF-003 กลางคืนเป็นแบบขั้น (วันที่ 1–14 = 4, วันที่ 15–28 = 7) · LF-006 แสดงเฉพาะวันที่ 1–7 endpoint = start+(end−start)×6/27 · ค่าพื้นฐาน = mean จาก d1 ถึง d7 ± ค่าเผื่อคงที่ (ความเร็วการเดิน ±0.05 m/s · กลางคืน ±1 ครั้ง · ลุกจากที่นั่ง ±2 วินาที) ข้อมูลจำลอง ไม่ใช่เกณฑ์คลินิก

**การควบคุม Demo:** ปุ่มลูกศร ↑↓←→ เลื่อนดูข้อมูล · Esc ปิด dialog และยกเลิก draft (ไม่ล้างประวัติที่บันทึกแล้ว) · ปุ่ม reset เฉพาะล้าง demo · ไม่มี persistence เมื่อรีเฟรชข้อมูลจะล้าง

**จริยธรรม AI 8 ด้าน:** [source: participant + reviewer]

1. **Attribution** — ทุก claim มี source tag กำกับ เนื้อหาที่สร้างด้วย AI ระบุชัดเจน
2. **Truthfulness** — แยก claims ที่มีหลักฐาน สมมติฐาน และข้อเสนอออกจากกันชัดเจน
3. **Misinformation** — ข้อมูลที่เป็น proposed หรือ simulated มีป้ายกำกับทุกจุด
4. **Privacy** — ประมวลผลบนตัวเครื่อง ไม่มีกล้องหรือไมโครโฟน มีแผน privacy review
5. **Fairness** — ออกแบบเพื่อลดอุปสรรคการเข้าถึงสำหรับผู้สูงอายุชนบท
6. **Explainability** — กล่อง "ทำไมจัดลำดับนี้?" แสดง factors อธิบายได้
7. **Intellectual Property** — ภาพประกอบเป็นงานต้นฉบับ ไม่คัดลอก
8. **Human Responsibility** — ระบบอัตโนมัติมีเฉพาะชั้นตรวจจับ (sensing) การตัดสินใจดูแลรักษาเป็นของมนุษย์ทั้งหมด

*Full 6-case interactive demo with editorial design, validation workflow, three user views, and eight practical ethics actions.*

---

## 8. สถาปัตยกรรม

**Sense:** TI IWR6843 60 GHz → point cloud → MCU → ค่าสรุป 4 รายการ (ความเร็วการเดิน เวลาลุกจากที่นั่ง กลางคืน ล้ม) → NB-IoT/LTE-M เป็นช่องทาง transport [source: participant]

**Predict:** cloud สร้างค่าพื้นฐานเฉพาะบุคคล 2–3 สัปดาห์ (ระยะจริงต้องพิสูจน์ A6) → แนวโน้มเทียบค่าพื้นฐาน → 🟢🟡🔴 ค่าขีดแบ่งข้อเสนอ (C8) [source: participant]

**Act:** แดชบอร์ด อสม. → คิวจัดลำดับ → เยี่ยมบ้าน → บันทึกผล → รพ.สต. มนุษย์ตัดสินใจทุกขั้นตอน ไม่มีระบบสั่งการอัตโนมัติ (C7) [source: participant]

**Connect:** ครอบครัว LINE OA สถานะและแจ้งเตือน ไม่มีปุ่มฉุกเฉิน [source: participant]

*On-device radar processing → cellular transport → cloud risk scoring → human-operated visit queue → family notifications.*

---

## 9. ขอบเขต (MoSCoW)

| ระดับ | รายการ |
|---|---|
| Must | ตรวจจับการล้ม · ความเร็วการเดิน · เวลาลุกจากที่นั่ง · กลางคืน · สัญญาณ 🟢🟡🔴 · เยี่ยมบ้านโดยมนุษย์ · NB-IoT · demo พร้อมป้ายกำกับ |
| Should | แสดงแนวโน้มข้อมูล · LINE OA ครอบครัว · แดชบอร์ดพยาบาล |
| Could | สัญญาณชีพในอนาคต (C6) · หลายห้อง · เชื่อม HIS |
| Won't | ระบบสั่งการอัตโนมัติ (C7) · วินิจฉัยโรค · กล้อง/ไมโครโฟน · Custom PCB เฟส 0 · ทดสอบล้มจริงกับผู้สูงอายุ |

[source: participant]

*MoSCoW scope preserves human decision-making at every stage.*

---

## 10. เกณฑ์ความสำเร็จ

| เฟส | ตัวชี้วัด | หน่วย | เป้าหมาย | สถานะ |
|---|---|---|---|---|
| เฟส 0 (เดือน 0–4) | Fall sensitivity | ร้อยละ | อย่างน้อย 90% | proposed |
| เฟส 0 (เดือน 0–4) | False alarm | ครั้ง/บ้าน/สัปดาห์ | ไม่เกิน 1 | proposed |
| เฟส 1a (เดือน 4–8) | Baseline สำเร็จ 2–3 สัปดาห์ | บ้าน | TBD | proposed |
| เฟส 1a (เดือน 4–8) | Gait trend stability | เทียบ baseline | TBD | proposed |
| เฟส 1b (เดือน 8–12) | เยี่ยมเหลืองภายใน 72 ชั่วโมง | ร้อยละ | 80% | proposed |
| เฟส 1b (เดือน 8–12) | สัดส่วนเยี่ยมที่นำไปสู่การดำเนินการ | ร้อยละ (เยี่ยมที่มีการดำเนินการ / เยี่ยมทั้งหมด) | TBD | ต้องมีข้อมูลจริง |
| เฟส 1b (เดือน 8–12) | ภาวะผิดปกติใหม่ที่แพทย์ยืนยัน | จำนวนภาวะ (แพทย์ยืนยัน ไม่ใช่เซ็นเซอร์วินิจฉัย) | TBD | ต้องมีข้อมูลจริง |

ต้องผ่าน gate ทุกเฟส (clinical/consent/funding/NB-IoT/safety/regulatory) [source: participant]

*Seven KPIs across three phases — all targets are proposed and subject to validation.*

---

## 11. องค์ประกอบทีม

**เสาหลัก:** Health & Aging Society — เทคโนโลยีเพื่อสังคมสูงวัยที่ดีขึ้น [source: participant]

**การศึกษา:** นักศึกษาปริญญาตรี 4 คน สาขาวิศวกรรมคอมพิวเตอร์ 3 คนและหุ่นยนต์ 1 คน มีพื้นฐาน signal processing ทักษะจริงต้องประเมินเมื่อเริ่มเฟส 0 [source: participant]

**บทบาทที่เสนอ 4 ตำแหน่ง** (ยังไม่กำหนดรายบุคคล):

| บทบาท | ขอบเขตงาน |
|---|---|
| Radar/Firmware | ประมวลผล point cloud จาก IWR6843 พัฒนา TinyML |
| Backend/Edge-Cloud | เชื่อมต่อ NB-IoT พัฒนา risk engine และ API |
| Frontend/UX | แดชบอร์ด อสม. PWA, LINE OA, demo |
| Systems/Test | สถาปัตยกรรม ทดสอบ เอกสาร บูรณาการ |

**ตาราง RACI** (ตัวเอียง = ยังไม่มีตัวบุคคล ต้องหาก่อนเฟส 1) [source: participant + reviewer]:

| กิจกรรม | R (รับผิดชอบ) | A (อนุมัติ) | C (ปรึกษา) | I (รับทราบ) |
|---|---|---|---|---|
| Radar signal processing | Radar | Radar | Systems | Backend |
| Fall detection model | Radar | Systems | *ที่ปรึกษาคลินิก* | Backend |
| Risk engine | Backend | Backend | *คลินิก, สาธารณสุข* | Frontend |
| Dashboard อสม. | Frontend | Frontend | *สาธารณสุข* | Backend |
| Threshold ค่าขีดแบ่ง | *ที่ปรึกษาคลินิก* | *ที่ปรึกษาคลินิก* | Systems | ทุกคน |
| PDPA และ consent | Systems | Systems | *ที่ปรึกษากฎหมาย* | ทุกคน |
| เอกสารโครงการ | Frontend | Systems | ทุกคน | — |

**ช่องว่างบุคลากร:** 🔴 ที่ปรึกษาคลินิก (ไม่มี) · 🔴 ผู้เชี่ยวชาญสาธารณสุข (ไม่มี) · 🟡 ที่ปรึกษา PDPA (ไม่มี) — ทั้งหมดต้องหาก่อนเฟส 1 [source: participant + reviewer]

*Four proposed roles with seven RACI activities; three critical advisory gaps must be filled before Phase 1.*

---

## 12. ไทม์ไลน์

| เฟส | ช่วงเวลา | กิจกรรมหลัก | ทรัพยากร | ความพร้อม |
|---|---|---|---|---|
| เฟส 0 | เดือน 0–4 | ต้นแบบ eval kit 2–3 บ้านจำลอง ประเมินทักษะทีม | ทีม 4 คน งบไม่ระบุ | เริ่มได้แบบมีเงื่อนไข |
| เฟส 1a | เดือน 4–8 | ติดตั้งจริง 15–25 บ้าน 1 ตำบล สร้างค่าพื้นฐาน | ที่ปรึกษา+รพ.สต.+NB-IoT+consent+safety+approvals+งบ | ต้องผ่าน gate เฟส 0 |
| เฟส 1b | เดือน 8–12 | Act layer เปิดใช้ วัดตัวชี้วัด 3 รายการ | Act metrics operational | ต้องผ่าน gate เฟส 1a |

เฟส 0 เริ่มแบบมีเงื่อนไข งบประมาณไม่ระบุทุกเฟส [source: participant, assumption]

*Twelve-month conditional timeline; every phase transition requires passing all gates.*

---

## 13. ความเสี่ยงหลัก

| ลำดับ | ความเสี่ยง | ระดับ | แนวทางบรรเทา |
|---|---|---|---|
| R1 | ความแม่นยำจริงต่ำกว่า lab → false alarm บ่อย | 🔴 | เฟส 0 วัดจริง gate ไม่เกิน 1/สัปดาห์ |
| R2 | ไม่มี clinician → threshold ไม่เหมาะ | 🔴 | ที่ปรึกษาก่อนเฟส 1 |
| R3 | อย. จัดเป็นเครื่องมือแพทย์ | 🟡 | สอบถาม อย. (C5) |
| R4 | NB-IoT ไม่ครอบคลุม | 🟡 | สำรวจก่อนเลือกตำบล |
| R5 | ผู้สูงอายุไม่ยินยอม | 🟡 | consent ร่วมผู้เชี่ยวชาญ |
| R6 | LTC ซื้อ IoT ไม่ได้ | 🟡 | ยืนยัน สปสช. |
| R7 | Multi-person ล้มเหลว | 🟡 | เริ่มบ้าน 1 คน |
| R8 | ทีมเล็ก ไม่มีแผนฝึก/บำรุงรักษา | 🟡 | จำกัด scope วางแผน |

Pilot gates ยังไม่พร้อม: clinical ✗ · consent ✗ · funding ✗ · NB-IoT ✗ · safety ✗ · regulatory ✗ [source: participant + research report]

*Eight risks: two critical (accuracy, clinical advisor), six moderate; all pilot gates currently unmet.*

---

## 14. สมมติฐานเปิด

สมมติฐานทั้งหมดยอมรับเพื่อทดสอบ ไม่ใช่ข้อเท็จจริง [source: participant final approval]

| ลำดับ | สมมติฐาน | วิธีทดสอบ | กำหนดเวลา | สถานะ |
|---|---|---|---|---|
| A1 | เรดาร์ mmWave วัด 4 ตัวชี้วัด (ความเร็วการเดิน ลุกจากที่นั่ง กลางคืน ล้ม) ได้ในบ้านจริง | eval kit วัด 4 ตัวชี้วัด เทียบ reference observations และ timed walk test ไม่ใช้กล้อง | เฟส 0 | [source: assumption] [status: unconfirmed] |
| A2 | NB-IoT ครอบคลุมพื้นที่ชนบทเป้าหมาย | สำรวจสัญญาณจริงในตำบลเป้าหมาย | ก่อนเฟส 1 | [source: assumption] [status: unconfirmed] |
| A3 | อสม. ยอมรับและใช้ระบบคิวจัดลำดับ | สังเกตพฤติกรรมใช้งานจริงและสัมภาษณ์ | เฟส 1b | [source: assumption] [status: unconfirmed] |
| A4 | ต้นทุนวัสดุ 2,000–3,000 บาทต่อหน่วย | ใบเสนอราคาจริงจากผู้จัดจำหน่าย | เฟส 0 | [source: assumption] [status: unconfirmed] |
| A5 | กองทุน LTC จัดซื้อบริการเฝ้าระวังได้ | สอบถามระเบียบ สปสช. | ก่อน B2G | [source: assumption] [status: unconfirmed] |
| A6 | ค่าพื้นฐาน 2–3 สัปดาห์เพียงพอ | เทียบค่าพื้นฐานต่างระยะ | เฟส 1a | [source: assumption] [status: unconfirmed] |
| A7 | ตรวจจับการลดลงของความเร็วการเดินมากกว่า 0.1 m/s ภายใน 3–4 สัปดาห์ — extrapolation จากวรรณกรรมระยะหลายเดือนที่ยังไม่พิสูจน์ | clinical validation ร่วมที่ปรึกษาคลินิก | กำหนดร่วมที่ปรึกษา | [source: assumption] [status: unconfirmed] |
| A8 | ผู้สูงอายุยินยอมติดตั้งเรดาร์ในบ้าน | กระบวนการ informed consent | ก่อนเฟส 1 | [source: assumption] [status: unconfirmed] |
| A9 | TinyML บนตัวเครื่องแม่นยำพอ | ทดสอบเทียบ labelled reference observations; เปรียบเทียบ edge กับ cloud เสริมแต่ความแม่นยำต้องวัดจากข้อมูลอ้างอิงที่มี label | เฟส 0 | [source: assumption] [status: unconfirmed] |
| A10 | MOU True–กระทรวงสาธารณสุข ใช้ประโยชน์ได้ (ไม่เท่ากับได้อนุญาต) | เจรจา True โดยตรง | ต้องเจรจา | [source: assumption] [status: unconfirmed] |
| A11 | B2G ราคา 150–250 บาท/เดือน อปท. ยอมจ่าย | สอบถาม อปท. เป้าหมาย | ต้องตรวจสอบ | [source: assumption] [status: unconfirmed] |
| A12 | สถานะ อย. ยังไม่ชัดเจน มีแผน privacy review | สอบถาม อย. อย่างเป็นทางการ | ต้องสอบถาม | [source: assumption] [status: unconfirmed] |

*Twelve open assumptions accepted as hypotheses — A7 is an unvalidated extrapolation; A9 requires labelled reference data.*

---

## 15. บันทึกการตัดสินใจ

การตัดสินใจ 9 รายการและการแก้ไข 18 รายการ [source: participant + reviewer]

| ลำดับ | การตัดสินใจ | เหตุผล |
|---|---|---|
| D1 | เลือกเสาหลัก Health & Aging | ตรงทักษะและความสนใจทีม |
| D2 | 12 เดือน จาก 2–3 ไปสู่ 15–25 บ้าน | พิสูจน์แนวคิดได้ |
| D3 | Act metrics เป้า TBD | ต้องข้อมูลจริง |
| D4 | อนุมัติ scope | ครบมิติ |
| D5 | สัญญาณชีพออก MVP (C6) | จำกัดขอบเขต |
| D6 | ไม่มี auto dispatch (C7) | ความปลอดภัย |
| D7 | รัน Stage 2–7 ทั้งหมด | โครงการใหม่ |
| D8 | Deep Research + corrections | ตรวจสอบหลักฐาน |
| D9 | ปรับปรุงล่าสุดเป็น v7 | แก้ไขความสอดคล้องเชิงข้อเท็จจริง |

| ลำดับ | การแก้ไข | เหตุผล |
|---|---|---|
| C4 | True เป็นโอกาส ไม่ใช่พันธมิตร | ไม่อ้างเกินจริง [source: reviewer] |
| C5 | อย. ไม่ชัดเจนไม่ใช่ยกเว้น | ป้องกันอ้างยกเว้น [source: reviewer] |
| C6 | สัญญาณชีพเป็นแผนอนาคต | จำกัดขอบเขต [source: participant] |
| C7 | ไม่มี auto dispatch | ความปลอดภัย [source: participant] |
| C8 | ค่าขีดแบ่งเป็นข้อเสนอ | ป้องกันอ้างคลินิก [source: reviewer] |
| C9 | ค้นคว้าเท่านั้น | ทรัพยากรจำกัด [source: participant] |
| C10 | ลบ universal negative | พิสูจน์ไม่ได้ [source: reviewer] |
| C11 | True/Intel ไม่ยืนยัน | ไม่มีแหล่งสาธารณะ [source: reviewer] |
| C12 | ลบตัวเลข LTC | ไม่ตรวจสอบ [source: reviewer] |
| C13 | ลบราคา Vayyar | ราคาเปลี่ยนได้ [source: reviewer] |
| C14 | B2C ภาษาเป็นกลาง | ป้องกันอคติ [source: participant] |
| C15 | 4 ชั้น cellular=transport | สะท้อนการออกแบบ [source: participant] |
| C16 | คำพูดตรงตามต้นฉบับ | ความถูกต้อง [source: participant] |
| C17 | ลบสถิติไม่ตรวจสอบ | ป้องกัน misinformation [source: reviewer] |
| C18 | ค่าพื้นฐาน demo ค่าเผื่อคงที่ | demo ไม่ใช่คลินิก [source: reviewer] |
| C19 | กลางคืนเป็นพฤติกรรม ไม่ใช่วินิจฉัย | ป้องกันตีความแพทย์ [source: reviewer] |
| C20 | URL เป็นจุดเริ่มค้นหา | แยกอ้างอิงกับ verify [source: reviewer] |
| C21 | ORCATECH เป็นงานเชิงสังเกต | ไม่ validate เทคโนโลยี [source: reviewer] |

*Nine decisions and eighteen corrections ensuring evidence integrity throughout.*

---

## 16. Go / No-Go

**คำแนะนำ: CONDITIONAL GO — แนะนำเริ่มพัฒนาต้นแบบแบบมีเงื่อนไข**

หลานเฝ้าตอบปัญหาชัดเจนเรื่องการดูแลผู้สูงอายุ ผูกเข้ากับโครงสร้าง อสม. และ รพ.สต. ที่มีอยู่แล้ว ทีมมีพื้นฐานตรงสาขาสำหรับเฟส 0 [source: participant]

**เงื่อนไข 7 ข้อ:**
1. เฟส 0 ต้องวัดผลจริงก่อนอ้างตัวเลข
2. ต้องหาที่ปรึกษาคลินิกก่อนกำหนดค่าขีดแบ่ง
3. ไม่อ้างว่าผ่านการพิสูจน์จนกว่าจะทดสอบจริง
4. ไม่อ้างสิทธิ์กองทุน LTC จนกว่าจะยืนยัน สปสช.
5. ไม่มีระบบสั่งการฉุกเฉินอัตโนมัติ
6. ไม่ทดสอบการล้มจริงกับผู้สูงอายุ
7. clinical, consent, funding, connectivity, safety, regulatory ต้องพร้อมก่อน pilot

สมมติฐาน A1–A12 ยอมรับเพื่อทดสอบเท่านั้น [source: participant final approval]

*Conditional go: clear problem, existing infrastructure, matched skills — seven conditions must hold.*

---

## 17. หลักฐานและแหล่งอ้างอิง

**ระบบ source tag ที่ใช้:**
- [source: participant] — ผู้เข้าร่วมระบุ
- [source: participant, verbatim] — คำพูดตรงผู้เข้าร่วม (เฉพาะหมวด 2)
- [source: research report] — Deep Research รหัส bdd5aac6
- [source: assumption] — สมมติฐาน A1–A12
- [source: entrypoint] — URL จุดเริ่มค้นหา ยังไม่ verify เนื้อหาทั้งหมด
- [correction Cn] — การแก้ไข C4–C21
- [source: reviewer] — การแก้ไขโดย reviewer

**URL Entrypoints** (จุดเริ่มค้นหา ยังไม่ verify เนื้อหาทั้งหมด):

| แหล่ง | URL |
|---|---|
| TI IWR6843 | https://www.ti.com/product/IWR6843 |
| Vayyar Care | https://vayyar.com/care |
| Walabot Home | https://walabot.com/walabot-home |
| ORCATECH | https://www.ohsu.edu/oregon-center-for-aging-and-technology |
| Apple Watch Fall Detection | https://support.apple.com/en-us/108896 |
| สปสช. | https://www.nhso.go.th |
| อย. | https://www.fda.moph.go.th |
| PDPC | https://www.pdpc.or.th |

**ช่องว่าง:** True/Intel ไม่มี URL สาธารณะ · SEMAS ที่มาไม่ยืนยัน · ORCATECH DOI ไม่ตรวจสอบ · Garmin spec ไม่ทราบ

สร้างด้วย Amazon Quick AI · ทุก claim มี source tag · สมมติฐานแยกจากหลักฐาน · demo เป็นข้อมูลจำลอง · มนุษย์ตัดสินใจทุกขั้นตอน

---

END OF CONTENT DOCUMENT v7
