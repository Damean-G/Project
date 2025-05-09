# CreekWise: An AI-Powered Trail Companion

**CreekWise** is an AI-enhanced web application designed to improve the trail experience at Coyote Creek Trail in Santa Clara County. The app was developed to address declining trail usage caused by safety concerns, habitat degradation, and limited ecological awareness. By providing real-time information, interactive maps, and educational tools, CreekWise empowers visitors to make safer, more informed visits while fostering environmental stewardship.

## 🌟 Key Features
- **Interactive Trail Maps:** Explore six unique maps of Coyote Creek Trail, each highlighting different trail sections and features.
- **Live Weather Integration:** Displays real-time weather conditions at the trailhead for informed trip planning.
- **Trail Popularity & Traffic Score:** AI-based metric showing how busy each trail segment is, factoring in time of day and weather conditions.
- **Real-Time Data Updates:** Automatically refreshes trail conditions and weather for current, actionable insights.
- **User-Friendly Interface:** Built with Streamlit for accessible, mobile-friendly navigation and information display.

## 👩‍💻 My Contributions
- Developed the **map section** with six different interactive trail maps
- Created the **popularity and traffic scoring system** linked to time of day and weather data
- Integrated **live weather data API** into the app for real-time updates
- Conducted **multiple rounds of app testing** to ensure reliability and usability
- Maintained project files and ensured team met deadlines with quality deliverables

## 🛠️ Tech Stack
- **Frontend:** Streamlit
- **Backend/AI:** OpenAI API (GPT, DALL·E), text-to-speech tools
- **Data Integration:** Live weather API, Santa Clara County trail data & map images
- **Other Tools:** GitHub Copilot, Python

## 🎥 Demo / Resources
- [Link to Demo Video] *(https://drive.google.com/file/d/1QjQ9p8zIiyrz7kD6yHE9szU2R0tFpwT4/view?resourcekey)*
- [GitHub Repository] *(https://github.com/NickShein222/AI-For-Social-Goods)*
- [Presentation Slides] *(https://docs.google.com/presentation/d/1TOzlv1AE6_LkzH26q4tJC53cAwZL-MKZOB_FGAaeBvs/edit?usp=sharing)*
- [Project Code] *(https://docs.google.com/document/d/1gmHgy9an0OhJ6Bg2E8Y3bLrDExRoBzvybj_rEIgkiz4/edit?usp=sharing)*

---

## 🚀 About the Project
This project was completed as part of the AI for Social Good course at San Jose State University, Spring 2025. It showcases the integration of AI tools to solve real-world social and environmental challenges through technology.

---

## Coding Contributions
# --- Trail Segments ---
st.markdown('<h2 id="trail-map">Trail Segments</h2>', unsafe_allow_html=True)


trail_data = {
    "Map of William Street to Phelan Avenue": {"image": "images/segment1.png", "risk": 4},
    "Map of Highway 237 Bikeway to Montague Expressway": {"image": "images/segment2.png", "risk": 6},
    "Map of Tully Road to Yerba Buena Road": {"image": "images/segment3.png", "risk": 5},
    "Map of Yerba Buena Road to County jurisdiction": {"image": "images/segment4.png", "risk": 7},
    "Map of Alignment from Berryessa BART to BRT Santa Clara Street": {"image": "images/segment5.png", "risk": 8},
    "Map of 'Odette Morrow Trail'": {"image": "images/segment6.png", "risk": 6},
}


left_col, right_col = st.columns([1, 2])
with left_col:
    st.subheader("📍 Choose a Trail Segment")
    selected_trail = st.selectbox("Select a trail segment:", list(trail_data.keys()))
segment = trail_data[selected_trail]
with right_col:
    st.image(segment["image"], use_container_width=True, caption=selected_trail)


st.divider()


crowd_score = min(round((crowd_level + segment_popularity) / 2), 10)


# --- Live Weather API Fetch for Santa Clara, CA ---
api_key = "283c59d61c54b7d0c71676885db453f8"
weather_url = f"http://api.openweathermap.org/data/2.5/weather?q=Santa Clara&units=imperial&appid={api_key}"


try:
    weather_data = requests.get(weather_url).json()


    temperature = weather_data["main"]["temp"]

# --- Popularity & Comfort Score Section ---
st.header("🧠 Trail Comfort & Popularity Score")

# Segment popularity score (based on known risks)
segment_popularity = {
   "Map of William Street to Phelan Avenue": 4,
   "Map of Highway 237 Bikeway to Montague Expressway": 6,
   "Map of Tully Road to Yerba Buena Road": 5,
   "Map of Yerba Buena Road to County jurisdiction": 7,
   "Map of Alignment from Berryessa BART to BRT Santa Clara Street": 8,
   "Map of 'Odette Morrow Trail'": 6
}

# Live crowd prediction based on time of day
hour_of_day = datetime.datetime.now().hour
if 7 <= hour_of_day <= 10:
    crowd_level = 3  # morning low
elif 11 <= hour_of_day <= 16:
    crowd_level = 8  # afternoon peak
else:
    crowd_level = 5  # evening moderate

# Weather impact score (based on live data you fetched earlier)
weather_score = 0
if temperature >= 85:
    weather_score += 2
if "Rain" in description or "Showers" in description:
    weather_score += 3
if wind_speed >= 15:
    weather_score += 1

# Calculate overall score
popularity = segment_popularity.get(selected_trail, 5)
overall_score = crowd_level + popularity + weather_score

# Normalize for progress bar (0-30)
progress_normalized = min(max(overall_score, 0), 30)

# Display comfort score dynamically
if overall_score >= 18:
    st.error(f"🔥 Comfort & Safety Score: {overall_score}/30 – Hot, crowded, and risky conditions today.")
elif overall_score >= 12:
    st.warning(f"⚠️ Comfort & Safety Score: {overall_score}/30 – Conditions are moderate.")
else:
    st.success(f"✅ Comfort & Safety Score: {overall_score}/30 – Great time for a hike!")

# Animated progress bar
progress_bar = st.progress(0)
for percent_complete in range(progress_normalized + 1):
    progress_bar.progress(percent_complete / 30)






