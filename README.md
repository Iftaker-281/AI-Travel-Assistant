##AI Travel Assistant (n8n + MCP)

This project is an AI-powered travel assistant built in n8n. It connects a chat-facing AI Agent to a custom Model Context Protocol (MCP) server so the assistant can retrieve travel information from external APIs before making recommendations. It also includes optional Google Sheets-based travel preference storage for personalized suggestions.
Workflows Used
This project contains two n8n workflows:
1. AI Travel Assistant — the main chat workflow that receives a user's message, uses an AI Agent and Groq Chat Model, maintains simple conversation memory, and calls the Travel Info MCP Server through an MCP Client Tool.
2. Travel Info MCP Server — exposes travel-information tools through one MCP endpoint.
The exported workflow files are included in this repository.
How It Works
1. Chat Trigger receives the user's travel question.
2. AI Agent identifies the user's intent and decides which travel tools are needed.
3. Groq Chat Model generates the response using the configured system prompt.
4. Simple Memory helps maintain context during the conversation.
5. Travel Info Tools (MCP Client Tool) connects the AI Agent to the MCP server.
6. MCP Server Trigger exposes the travel tools through an HTTP Streamable MCP endpoint.
7. The MCP tools retrieve destination coordinates, weather, hotels, restaurants, attractions, and currency exchange information from external APIs.
8. Google Sheets tools can save and retrieve travel preferences, allowing recommendations to be personalized across sessions.
9. The AI Agent combines the returned tool results into a structured travel plan. If a tool fails or returns no data, the assistant should clearly say which information could not be retrieved rather than inventing it.
MCP Tools Available
Tool	Purpose	Data source / service
find_destination	Finds a destination and its coordinates	Open-Meteo Geocoding API
get_weather	Retrieves current weather information	Open-Meteo Forecast API
search_hotels	Searches accommodation-related places	OpenTripMap API
find_restaurant	Searches food and restaurant-related places	OpenTripMap API
search_attractions	Searches tourist attractions and points of interest	OpenTripMap API
get_exchange_rate	Retrieves exchange-rate information	ExchangeRate-API
save_preference	Saves or updates a user's travel preferences	Google Sheets
get_preferences	Retrieves saved preferences for a user	Google Sheets


Note: Hotel, restaurant, and attraction results depend on the data returned by the configured API. Do not treat search results as confirmed availability or booking information.
Setup Requirements
- n8n
- A Groq API key and a configured Groq Chat Model credential
- An OpenTripMap API key
- Google Sheets access and OAuth2 credentials if using preference storage
- Internet access for the external API requests
1. Import the n8n Workflows
1. Open your n8n instance.
2. Import AI Travel Assistant.json as the main chat workflow.
3. Import Travel Info MCP Server.json as the MCP server workflow.
4. Open each workflow and review its nodes and connections.
5. Configure credentials and API keys before testing.
2. Configure the MCP Server
1. Open the Travel Info MCP Server workflow.
2. Review the MCP Server Trigger node and its HTTP Streamable endpoint.
3. Configure the endpoint according to your n8n instance.
4. Activate the MCP server workflow if your setup requires activation for the production endpoint.
5. Copy the correct MCP endpoint from your own n8n instance. Do not assume a localhost URL or an endpoint ID from another installation will work.
The MCP server groups the travel tools under one endpoint to make integration and maintenance simpler.
3. Configure External APIs
Open-Meteo
- find_destination uses https://geocoding-api.open-meteo.com/v1/search.
- get_weather uses https://api.open-meteo.com/v1/forecast.
- Check the query parameters in the nodes and make sure destination coordinates are passed correctly.
OpenTripMap
- search_hotels, find_restaurant, and search_attractions use the OpenTripMap places API.
- Add your own API key in the relevant nodes or credentials.
- Check the kinds, latitude, longitude, radius, and result-limit parameters for each tool.
ExchangeRate-API
- get_exchange_rate uses the base currency requested by the AI Agent.
- Verify the endpoint and the service's current usage requirements before running the workflow.
4. Configure Google Sheets (Travel Preferences)
The optional preference tools use a Google Sheet with these columns:
user_id | budget | hotel_type | transport | diet | activities | updated_at
1. Create or open the Google Sheet for travel preferences.
2. Configure Google Sheets OAuth2 credentials in n8n.
3. Select the correct spreadsheet and worksheet in both save_preference and get_preferences.
4. Make sure user_id is used consistently to identify a user's saved preferences.
5. Test saving and retrieving a preference before relying on personalization.
Example preferences include a preferred budget, hotel type, transport, dietary requirements, and activities.
5. Configure the AI Travel Assistant
1. Open the AI Travel Assistant workflow.
2. Configure the Groq Chat Model credential.
3. Confirm that the AI Agent is connected to the chat trigger, model, memory, and Travel Info Tools MCP Client Tool.
4. Set the MCP Client Tool to use the endpoint copied from your MCP server workflow.
5. Review the system prompt. It should direct the agent to call the appropriate tools for live information, wait for required results, and avoid inventing hotels, restaurants, weather, exchange rates, prices, distances, or availability.
6. Test the workflow with a real destination and check the execution data for each tool call.
Example Requests
Try questions such as:
- Plan a 3-day trip to Cox's Bazar.
- What is the current weather in Kathmandu?
- Find attractions and restaurants near Bandarban.
- Suggest hotels for a budget trip to Sylhet.
- What is the exchange rate from USD to BDT?
- Remember that I prefer mid-range hotels and bus transport.
The tools required will depend on the user's question. International travel questions may require currency exchange information; destination-based requests may need geocoding first.
Expected Response Structure
The assistant is intended to organize travel recommendations into relevant sections, such as:
- Trip overview
- Weather
- Hotel recommendations
- Restaurants
- Top attractions
- Packing checklist
- Budget and currency
- Transportation tips
- Travel tips
- Final recommendation
Only include sections supported by the retrieved information. If a tool cannot retrieve the requested data, clearly state that limitation.
Troubleshooting
- MCP connection fails: Check that the MCP server workflow is running/active as required and that the client uses the correct endpoint.
- API request fails: Check API keys, credentials, endpoint URLs, query parameters, and any service usage limits.
- No hotels, restaurants, or attractions returned: Try another destination or verify the OpenTripMap query parameters and API key.
- Weather results are incorrect or missing: Confirm that find_destination returns the correct latitude and longitude and that these values reach get_weather.
- Preferences are not saved or retrieved: Check Google Sheets credentials, selected spreadsheet/worksheet, column headers, and user_id matching.
- The agent gives unsupported details: Review the system prompt and confirm that the relevant MCP tool is called and its output is used before the final answer.
Project Files
- AI Travel Assistant.json — main chat workflow.
- Travel Info MCP Server.json — MCP server workflow with the travel tools.
- AI TRAVEL ASSISTENT workflow.png — workflow screenshot.
- Travel Info MCP Server.png — MCP server workflow screenshot.
- MCP Server Information.docx — MCP server setup and node information.
- Project_Explanation.docx — project explanation and design notes.
- Travel Preferences.xlsx — example travel preference data.
- Recodring Vedio.mp4 — project recording/demo video.
Notes and Limitations
- This project retrieves information from external services; the quality and availability of results depend on those services and their API limits.
- The OpenTripMap results are place-search results, not verified hotel reservations or restaurant bookings.
- The Google Sheets preference feature requires its own valid credentials and spreadsheet configuration.
- A working AI response depends on the configured model provider having sufficient quota and valid credentials.
- Remove API keys, access tokens, private endpoint URLs, and other secrets from exported workflow files before sharing the repository publicly.
