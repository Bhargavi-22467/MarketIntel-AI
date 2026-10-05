import os
import json
import pandas as pd
import streamlit as st
from dotenv import load_dotenv
from openai import OpenAI


# ============================================================
# 1. LOAD ENVIRONMENT VARIABLES
# ============================================================

load_dotenv()

api_key = os.getenv("OPENAI_API_KEY")


# ============================================================
# 2. PAGE CONFIGURATION
# ============================================================

st.set_page_config(
    page_title="MarketIntel AI",
    page_icon="📊",
    layout="wide"
)


# ============================================================
# 3. CUSTOM CSS
# ============================================================

st.markdown(
    """
    <style>

    .main-title {
        font-size: 42px;
        font-weight: 700;
        text-align: center;
        margin-bottom: 5px;
    }

    .subtitle {
        text-align: center;
        font-size: 18px;
        margin-bottom: 25px;
    }

    .dashboard-card {
        padding: 20px;
        border-radius: 12px;
        border: 1px solid rgba(128,128,128,0.25);
        margin-bottom: 15px;
    }

    .score-title {
        font-size: 15px;
        font-weight: 600;
    }

    .score-value {
        font-size: 32px;
        font-weight: 700;
    }

    .swot-box {
        padding: 18px;
        border-radius: 12px;
        border: 1px solid rgba(128,128,128,0.25);
        min-height: 180px;
    }

    .small-text {
        font-size: 14px;
    }

    </style>
    """,
    unsafe_allow_html=True
)


# ============================================================
# 4. HEADER
# ============================================================

st.markdown(
    '<div class="main-title">📊 MarketIntel AI</div>',
    unsafe_allow_html=True
)

st.markdown(
    '<div class="subtitle">'
    'AI-Powered Market Research & Competitive Intelligence Dashboard'
    '</div>',
    unsafe_allow_html=True
)

st.divider()


# ============================================================
# 5. API KEY CHECK
# ============================================================

if not api_key:

    st.error(
        "OpenAI API key not found. Please create a .env file "
        "and add your OPENAI_API_KEY."
    )

    st.stop()


client = OpenAI(api_key=api_key)


# ============================================================
# 6. SIDEBAR
# ============================================================

with st.sidebar:

    st.header("📊 MarketIntel AI")

    st.write(
        """
        Analyze your market, customers, competitors,
        opportunities and business risks using AI.
        """
    )

    st.divider()

    st.subheader("Dashboard Sections")

    st.write("📈 Market Overview")
    st.write("👥 Customer Insights")
    st.write("🏆 Competitor Intelligence")
    st.write("⚖️ SWOT Analysis")
    st.write("🚀 Opportunities")
    st.write("🎯 Strategies")
    st.write("⚠️ Risks")
    st.write("💡 Recommendations")


# ============================================================
# 7. USER INPUT
# ============================================================

st.header("🔎 Business Information")

col1, col2 = st.columns(2)

with col1:

    business_name = st.text_input(
        "Business / Product Name",
        placeholder="Example: FreshBite"
    )

    industry = st.text_input(
        "Industry",
        placeholder="Example: Food Delivery"
    )

    target_market = st.text_input(
        "Target Customers",
        placeholder="Example: College students and young professionals"
    )

with col2:

    location = st.text_input(
        "Target Location",
        placeholder="Example: Hyderabad, India"
    )

    competitors = st.text_input(
        "Main Competitors",
        placeholder="Example: Swiggy, Zomato"
    )

    business_goal = st.text_input(
        "Business Goal",
        placeholder="Example: Increase customer acquisition"
    )


product_description = st.text_area(
    "Describe your product or business",
    placeholder=(
        "Explain what your business offers, what problem it solves "
        "and what makes it different."
    ),
    height=130
)


st.divider()


# ============================================================
# 8. GENERATE BUTTON
# ============================================================

generate_button = st.button(
    "🚀 Generate Market Intelligence Dashboard",
    use_container_width=True
)


# ============================================================
# 9. AI ANALYSIS
# ============================================================

if generate_button:

    # --------------------------------------------------------
    # INPUT VALIDATION
    # --------------------------------------------------------

    if not business_name:
        st.warning("Please enter the business/product name.")
        st.stop()

    if not industry:
        st.warning("Please enter the industry.")
        st.stop()

    if not target_market:
        st.warning("Please enter the target customers.")
        st.stop()

    if not product_description:
        st.warning("Please describe your product or business.")
        st.stop()


    # --------------------------------------------------------
    # AI PROMPT
    # --------------------------------------------------------

    prompt = f"""
You are an expert business development and market intelligence
analyst.

Analyze the following business:

Business Name:
{business_name}

Industry:
{industry}

Target Customers:
{target_market}

Target Location:
{location}

Competitors:
{competitors}

Business Goal:
{business_goal}

Product Description:
{product_description}

Return ONLY valid JSON.

Do not include markdown.
Do not include ```json.
Do not include explanations outside the JSON.

The JSON must have EXACTLY this structure:

{{
    "executive_summary": "short summary",

    "scores": {{
        "market_opportunity": 0,
        "competitive_position": 0,
        "customer_potential": 0,
        "growth_potential": 0,
        "business_risk": 0
    }},

    "market_overview": {{
        "industry_outlook": "text",
        "key_factors": [
            "factor 1",
            "factor 2",
            "factor 3",
            "factor 4"
        ]
    }},

    "customer_segments": [
        {{
            "segment": "segment name",
            "description": "description",
            "needs": "main needs",
            "pain_points": "main pain points",
            "potential_score": 0
        }},
        {{
            "segment": "segment name",
            "description": "description",
            "needs": "main needs",
            "pain_points": "main pain points",
            "potential_score": 0
        }},
        {{
            "segment": "segment name",
            "description": "description",
            "needs": "main needs",
            "pain_points": "main pain points",
            "potential_score": 0
        }}
    ],

    "competitors": [
        {{
            "name": "competitor name",
            "strengths": "strengths",
            "weaknesses": "weaknesses",
            "threat_level": 0,
            "competitive_note": "short note"
        }}
    ],

    "swot": {{
        "strengths": [
            "item 1",
            "item 2",
            "item 3"
        ],
        "weaknesses": [
            "item 1",
            "item 2",
            "item 3"
        ],
        "opportunities": [
            "item 1",
            "item 2",
            "item 3"
        ],
        "threats": [
            "item 1",
            "item 2",
            "item 3"
        ]
    }},

    "market_opportunities": [
        {{
            "opportunity": "opportunity name",
            "description": "description",
            "score": 0
        }},
        {{
            "opportunity": "opportunity name",
            "description": "description",
            "score": 0
        }},
        {{
            "opportunity": "opportunity name",
            "description": "description",
            "score": 0
        }},
        {{
            "opportunity": "opportunity name",
            "description": "description",
            "score": 0
        }}
    ],

    "growth_strategies": [
        "strategy 1",
        "strategy 2",
        "strategy 3",
        "strategy 4",
        "strategy 5"
    ],

    "business_risks": [
        {{
            "risk": "risk name",
            "severity": 0,
            "mitigation": "how to reduce this risk"
        }},
        {{
            "risk": "risk name",
            "severity": 0,
            "mitigation": "how to reduce this risk"
        }},
        {{
            "risk": "risk name",
            "severity": 0,
            "mitigation": "how to reduce this risk"
        }}
    ],

    "recommendations": [
        "recommendation 1",
        "recommendation 2",
        "recommendation 3",
        "recommendation 4",
        "recommendation 5"
    ]
}}

IMPORTANT:

All score values must be integers from 0 to 100.

The analysis should be an AI-based assessment, not a claim of
real-time market research.

Do not claim to have accessed live websites, private databases,
financial reports or real-time competitor information.

If competitor information is uncertain, clearly treat it as an
AI assessment.
"""


    # --------------------------------------------------------
    # CALL OPENAI
    # --------------------------------------------------------

    with st.spinner("🤖 AI is analyzing the market..."):

        try:

            response = client.responses.create(
                model="gpt-6-luna",
                input=prompt
            )

            raw_output = response.output_text.strip()

            # Remove accidental markdown fences
            if raw_output.startswith("```json"):
                raw_output = raw_output[7:]

            if raw_output.startswith("```"):
                raw_output = raw_output[3:]

            if raw_output.endswith("```"):
                raw_output = raw_output[:-3]

            raw_output = raw_output.strip()

            data = json.loads(raw_output)

        except json.JSONDecodeError:

            st.error(
                "The AI returned an invalid JSON response. "
                "Please try generating the analysis again."
            )

            st.stop()

        except Exception as error:

            st.error("Unable to generate the AI analysis.")

            st.code(str(error))

            st.stop()


    # ========================================================
    # 10. DASHBOARD HEADER
    # ========================================================

    st.success("✅ Market intelligence generated successfully!")

    st.header("📊 Market Intelligence Dashboard")

    st.write(
        f"### {business_name}"
    )

    st.write(
        data["executive_summary"]
    )


    # ========================================================
    # 11. KPI CARDS
    # ========================================================

    st.subheader("📌 Business Intelligence Scores")

    scores = data["scores"]

    c1, c2, c3, c4, c5 = st.columns(5)

    with c1:
        st.metric(
            "Market Opportunity",
            f'{scores["market_opportunity"]}/100'
        )

    with c2:
        st.metric(
            "Competitive Position",
            f'{scores["competitive_position"]}/100'
        )

    with c3:
        st.metric(
            "Customer Potential",
            f'{scores["customer_potential"]}/100'
        )

    with c4:
        st.metric(
            "Growth Potential",
            f'{scores["growth_potential"]}/100'
        )

    with c5:
        st.metric(
            "Business Risk",
            f'{scores["business_risk"]}/100'
        )


    st.divider()


    # ========================================================
    # 12. MARKET OVERVIEW
    # ========================================================

    st.header("📈 Market Overview")

    market = data["market_overview"]

    st.subheader("Industry Outlook")

    st.write(market["industry_outlook"])

    st.subheader("Key Market Factors")

    for factor in market["key_factors"]:
        st.write(f"• {factor}")


    st.divider()


    # ========================================================
    # 13. CUSTOMER INSIGHTS
    # ========================================================

    st.header("👥 Customer Intelligence")

    customer_data = data["customer_segments"]

    customer_df = pd.DataFrame(customer_data)

    st.dataframe(
        customer_df[
            [
                "segment",
                "description",
                "needs",
                "pain_points",
                "potential_score"
            ]
        ],
        use_container_width=True,
        hide_index=True
    )

    st.subheader("Customer Segment Potential")

    chart_data = customer_df[
        ["segment", "potential_score"]
    ].set_index("segment")

    st.bar_chart(chart_data)


    st.divider()


    # ========================================================
    # 14. COMPETITOR INTELLIGENCE
    # ========================================================

    st.header("🏆 Competitor Intelligence")

    competitor_data = data["competitors"]

    if competitor_data:

        competitor_df = pd.DataFrame(competitor_data)

        st.dataframe(
            competitor_df,
            use_container_width=True,
            hide_index=True
        )

        st.subheader("Competitor Threat Levels")

        threat_chart = competitor_df[
            ["name", "threat_level"]
        ].set_index("name")

        st.bar_chart(threat_chart)

    else:

        st.info(
            "No competitors were provided. Add competitors to "
            "receive competitor intelligence."
        )


    st.divider()


    # ========================================================
    # 15. SWOT ANALYSIS
    # ========================================================

    st.header("⚖️ SWOT Analysis")

    swot = data["swot"]

    col1, col2 = st.columns(2)

    with col1:

        st.subheader("💪 Strengths")

        for item in swot["strengths"]:
            st.success(item)

    with col2:

        st.subheader("⚠️ Weaknesses")

        for item in swot["weaknesses"]:
            st.warning(item)


    col3, col4 = st.columns(2)

    with col3:

        st.subheader("🚀 Opportunities")

        for item in swot["opportunities"]:
            st.info(item)

    with col4:

        st.subheader("🔥 Threats")

        for item in swot["threats"]:
            st.error(item)


    st.divider()


    # ========================================================
    # 16. MARKET OPPORTUNITIES
    # ========================================================

    st.header("🚀 Market Opportunities")

    opportunities = data["market_opportunities"]

    opportunity_df = pd.DataFrame(opportunities)

    st.dataframe(
        opportunity_df,
        use_container_width=True,
        hide_index=True
    )

    opportunity_chart = opportunity_df[
        ["opportunity", "score"]
    ].set_index("opportunity")

    st.bar_chart(opportunity_chart)


    st.divider()


    # ========================================================
    # 17. GROWTH STRATEGIES
    # ========================================================

    st.header("📈 Growth Strategies")

    for index, strategy in enumerate(
        data["growth_strategies"],
        start=1
    ):

        st.info(
            f"**Strategy {index}:** {strategy}"
        )


    st.divider()


    # ========================================================
    # 18. BUSINESS RISKS
    # ========================================================

    st.header("⚠️ Business Risk Analysis")

    risk_data = data["business_risks"]

    risk_df = pd.DataFrame(risk_data)

    st.dataframe(
        risk_df,
        use_container_width=True,
        hide_index=True
    )

    risk_chart = risk_df[
        ["risk", "severity"]
    ].set_index("risk")

    st.bar_chart(risk_chart)


    st.divider()


    # ========================================================
    # 19. RECOMMENDATIONS
    # ========================================================

    st.header("💡 AI Strategic Recommendations")

    for index, recommendation in enumerate(
        data["recommendations"],
        start=1
    ):

        st.success(
            f"**{index}.** {recommendation}"
        )


    st.divider()


    # ========================================================
    # 20. DOWNLOAD DATA
    # ========================================================

    st.header("📥 Export Analysis")

    download_data = json.dumps(
        data,
        indent=4
    )

    st.download_button(
        label="Download Market Intelligence Data",
        data=download_data,
        file_name=(
            f"{business_name}_market_intelligence.json"
        ),
        mime="application/json",
        use_container_width=True
    )