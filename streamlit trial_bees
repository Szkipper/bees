import pandas as pd
import plotly.express as px
import streamlit as st

# Data Set Uploaded to Streamlit
bees = pd.read_csv(r"C:\Users\amberz\OneDrive - GBD Architects Incorporated\Copy_BeeData.csv")
bees = bees.groupby(["Year", "state_code", "Affected_by"])["Pct of Colonies Impacted"].mean().reset_index()

# internet tab and page appearance
st.set_page_config(page_title="Bee Colony Collapse",
                   page_icon=":honeybee:",
                   layout="wide")

# Sidebar for Data Set
cause_of_collapse = st.sidebar.radio(
    "Cause of Collapse:", bees["Affected_by"].unique())

year = st.sidebar.slider(
    "Year:", 2015, 2019, 2015, 1)

state = st.sidebar.multiselect(
    "States:",
    options=sorted(bees["state_code"].unique()),
    default=sorted(bees["state_code"].unique()))

# how you call back to the webapp
bees_selection = bees.query("Affected_by == @cause_of_collapse & Year == @year & state_code == @state")
bees_selection1 = bees.query("Affected_by == @cause_of_collapse & Year == @year & state_code == @state")
bees_selection2 = bees.query("Year == @year")

# webapp appearance
st.title("Bee Data :honeybee:")

col1, col2, col3 = st.columns((2, 1, 1))
with col1:
    figure1 = px.bar(bees_selection, x="state_code", y="Pct of Colonies Impacted",
                     color="Affected_by",
                     color_discrete_sequence=px.colors.diverging.Fall)
    figure1.update_layout(barmode="group", xaxis_title="")
    figure1.update_layout(height=425, margin={"r": 0, "l": 0, "t": 0, "b": 0})
    st.plotly_chart(figure1,  use_container_width=True)

with col2:
    figure2 = px.pie(bees_selection2, values="Pct of Colonies Impacted", names="Affected_by",
                     title="Causes of Colony Collapse based on Year",
                     color_discrete_sequence=px.colors.diverging.Fall,
                     hole=0.4)
    figure2.update_layout(legend=dict(yanchor="top", y=1.15))
    st.plotly_chart(figure2, use_container_width=True)

with col3:
    bees_selection1.loc[bees_selection1['Pct of Colonies Impacted'] < 5, "state_code"] = "All Other States"
    figure3 = px.pie(bees_selection1, values="Pct of Colonies Impacted", names="state_code",
                     title="States Hit Hardest",
                     color_discrete_sequence=px.colors.diverging.Fall,
                     hole=0.4, opacity=1)
    figure3.update_traces(textposition='inside', textinfo='label')
    figure3.update(layout_showlegend=False)
    st.plotly_chart(figure3, use_container_width=True)

figure4 = px.choropleth(bees_selection, locations="state_code", locationmode="USA-states",
                        title="Percent of Colonies Impacted based on Year and Cause",
                        scope="usa",
                        color="Pct of Colonies Impacted",
                        color_continuous_scale="fall")
figure4.update_layout(height=700, margin={"r": 0, "l": 0, "t": 0, "b": 0})
figure4.update_geos(resolution=110)

st.plotly_chart(figure4, use_container_width=True)

# st.dataframe(bees_selection)
st.markdown("---")
