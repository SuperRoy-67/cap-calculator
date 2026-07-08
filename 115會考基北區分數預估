import streamlit as st

# 網頁初始化設定
st.set_page_config(
    page_title="115會考基北區落點計算機",
    page_icon="🎓",
    layout="centered"
)

def get_score_and_points(grade):
    # 基北區官方對照表：A++=7, A+=6, A=5, B++=4, B+=3, B=2, C=1
    mapping = {
        "A++": (7, 7), "A+": (6, 6), "A": (5, 5),
        "B++": (4, 4), "B+": (3, 3), "B": (2, 2),
        "C": (1, 1)
    }
    return mapping.get(grade, (0, 0))

# 網頁標頭與介紹
st.title("🎓 115 年國中會考成績落點計算機")
st.markdown("### 🏛️ 基北區專用（積分/積點一條龍自動估算面板）")
st.write("請在下方輸入您的各科答對題數，網頁會即時為您進行精準的加權與落點分析。")
st.divider()

# 使用者輸入區塊
st.subheader("📝 第一步：請輸入各科作答數據")
col1, col2 = st.columns(2)

with col1:
    chinese_correct = st.number_input("國文科答對題數 (0~42)", min_value=0, max_value=42, value=39)
    st.markdown("**--- 數學科加權組合 ---**")
    cq = st.number_input("數學選擇題對幾題 (0~25)", min_value=0, max_value=25, value=24)
    nq = st.number_input("數學非選題拿幾分 (0~6)", min_value=0, max_value=6, value=3)
    st.markdown("**--- 英文科加權組合 ---**")
    Er = st.number_input("英語閱讀題對幾題 (0~43)", min_value=0, max_value=43, value=43)
    El = st.number_input("英語聽力題對幾題 (0~21)", min_value=0, max_value=21, value=21)

with col2:
    social_correct = st.number_input("社會科答對題數 (0~54)", min_value=0, max_value=54, value=52)
    nature_correct = st.number_input("自然科答對題數 (0~50)", min_value=0, max_value=50, value=48)
    essay_grade = st.selectbox("寫作測驗得分 (0~6 級分)", options=[0, 1, 2, 3, 4, 5, 6], index=5)

st.divider()

# 等級與加權計算
if chinese_correct >= 40: grade_ch = "A++"
elif chinese_correct == 39: grade_ch = "A+"
elif chinese_correct >= 36: grade_ch = "A"
elif chinese_correct >= 32: grade_ch = "B++"
elif chinese_correct >= 28: grade_ch = "B+"
elif chinese_correct >= 20: grade_ch = "B"
else: grade_ch = "C"

mathscore = round((cq / 25) * 85 + (nq / 6) * 15, 1)
if 91.60 <= mathscore <= 100: grade_math = "A++"
elif mathscore >= 85.70: grade_math = "A+"
elif mathscore >= 77.10: grade_math = "A"
elif mathscore >= 68.70: grade_math = "B++"
elif mathscore >= 60.10: grade_math = "B+"
elif mathscore >= 40.60: grade_math = "B"
else: grade_math = "C"

Englishscore = round((Er / 43) * 80 + (El / 21) * 20, 2)
if 98.14 <= Englishscore <= 100: grade_English = "A++"
elif Englishscore >= 96.28: grade_English = "A+"
elif Englishscore >= 90.70: grade_English = "A"
elif Englishscore >= 82.30: grade_English = "B++"
elif Englishscore >= 72.80: grade_English = "B+"
elif Englishscore >= 40.50: grade_English = "B"
else: grade_English = "C"

if social_correct >= 52: grade_sc = "A++"
elif social_correct == 51: grade_sc = "A+"
elif social_correct >= 48: grade_sc = "A"
elif social_correct >= 41: grade_sc = "B++"
elif social_correct >= 35: grade_sc = "B+"
elif social_correct >= 23: grade_sc = "B"
else: grade_sc = "C"

if nature_correct >= 47: grade_na = "A++"
elif nature_correct == 46: grade_na = "A+"
elif nature_correct >= 43: grade_na = "A"
elif nature_correct >= 36: grade_na = "B++"
elif nature_correct >= 30: grade_na = "B+"
elif nature_correct >= 20: grade_na = "B"
else: grade_na = "C"

essay_mapping_score = {6: 1.0, 5: 0.8, 4: 0.6, 3: 0.4, 2: 0.2, 1: 0.1, 0: 0.0}
essay_score = essay_mapping_score.get(essay_grade, 0.0)

# 分數加總
sc_ch, pt_ch = get_score_and_points(grade_ch)
sc_math, pt_math = get_score_and_points(grade_math)
sc_eng, pt_eng = get_score_and_points(grade_English)
sc_sc, pt_sc = get_score_and_points(grade_sc)
sc_na, pt_na = get_score_and_points(grade_na)

total_score = sc_ch + sc_math + sc_eng + sc_sc + sc_na + essay_score
total_points = pt_ch + pt_math + pt_eng + pt_sc + pt_na

# 顯示儀表板
st.subheader("📊 第二步：即時分析結果報告")
m1, m2 = st.columns(2)
m1.metric(label="💡 基北區會考總積分 (滿分 36)", value=f"{total_score:.1f} 分")
m2.metric(label="🎯 基北區會考總積點 (滿分 35)", value=f"{total_points} 點")

st.markdown("#### 🔍 各科等級明細")
grid1, grid2, grid3 = st.columns(3)
grid1.write(f"📘 **國文**：`{grade_ch}`")
grid1.write(f"📙 **社會**：`{grade_sc}`")
grid2.write(f"📐 **數學**：`{grade_math}` *(加權 {mathscore} 分)*")
grid2.write(f"🔬 **自然**：`{grade_na}`")
grid3.write(f"🔤 **英語**：`{grade_English}` *(加權 {Englishscore} 分)*")
grid3.write(f"✍️ **寫作**：`{essay_grade} 級分`")

st.balloons()  # 每當調整分數時，網頁會自動噴出歡慶氣球動畫！
