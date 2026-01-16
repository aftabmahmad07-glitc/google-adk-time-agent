import datetime
import pytz
from google.adk import Agent, Tool

# Step 1: Define the Time-Fetching Tool
def get_current_time(city: str):
    """Fetches the current time for a given city."""
    # Mapping common cities to timezones
    zones = {
        "lahore": "Asia/Karachi",
        "peshawar": "Asia/Karachi",
        "new york": "America/New_York",
        "london": "Europe/London"
    }
    
    zone = zones.get(city.lower(), "UTC")
    now = datetime.datetime.now(pytz.timezone(zone))
    return f"The current time in {city} is {now.strftime('%I:%M %p')}."

# Step 2: Initialize the Agent
time_tool = Tool(fn=get_current_time)
agent = Agent(
    name="TimeBot",
    instructions="You are a helpful assistant that tells the time. Use the get_current_time tool.",
    tools=[time_tool]
)

# Step 3: Run the Agent
if __name__ == "__main__":
    # This allows you to test in the terminal
    user_query = input("Ask me the time: ")
    response = agent.run(user_query)
    print(response.text)
