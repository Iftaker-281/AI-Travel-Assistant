AI Travel Assistant using MCP in n8n
An AI-powered travel assistant built with n8n, an AI Agent, and a custom Model Context Protocol (MCP) server. The assistant can call travel-information tools to retrieve destination coordinates, current weather, nearby places, exchange rates, and optional saved travel preferences.
Project Workflows
This repository includes two exported n8n workflows:
- AI Travel Assistant.json — the main chat workflow. It contains a Chat Trigger, AI Agent, Groq Chat Model, Simple Memory, and an MCP Client Tool.
- Travel Info MCP Server.json — the MCP server workflow. It exposes eight tools through an MCP Server Trigger.
How It Works
1. A user submits a travel question through the n8n chat trigger.
2. The AI Agent identifies the destination and the information needed.
3. The MCP Client Tool connects the agent to the Travel Info MCP Server.
4. The agent calls the relevant tools and uses their results to prepare a travel response.
5. Simple Memory provides conversational context within the configured memory window.
6. If Google Sheets is configured, preference tools can save and retrieve a user's travel preferences.
The workflow's system prompt instructs the agent not to invent travel information and to report when a tool cannot retrieve the requested data.
MCP Tools
Tool	Purpose	Configured service
find_destination	Finds a place and its latitude/longitude	Open-Meteo Geocoding API
get_weather	Retrieves current weather information using coordinates	Open-Meteo Forecast API
search_hotels	Searches nearby accommodation-related places	OpenTripMap API
find_restaurant	Searches nearby food and restaurant-related places	OpenTripMap API
search_attractions	Searches tourist attractions and points of interest	OpenTripMap API
get_exchange_rate	Retrieves an exchange rate for a base currency	ExchangeRate-API
save_preference	Saves or updates travel preferences	Google Sheets
get_preferences	Retrieves saved travel preferences	Google Sheets


Important limitation: Place-search results are not confirmed hotel or restaurant availability, reservations, or guaranteed prices. The information returned depends on the configured APIs and their data.
Requirements
- An n8n instance
- A Groq API key and configured Groq Chat Model credential
- An OpenTripMap API key for the places tools
- Google Sheets OAuth2 credentials and a configured spreadsheet, if using preference storage
- Access to the external APIs used by the workflows
API services may change their endpoints, authentication requirements, or free-tier limits. Check their current documentation if a request fails.
Setup
1. Import the workflows
1. Open n8n.
2. Import Travel Info MCP Server.json.
3. Review its MCP Server Trigger and tool nodes.
4. Import AI Travel Assistant.json.
5. Review the node connections and configure the credentials required by your own n8n instance.
2. Configure the MCP connection
1. Open the MCP server workflow and check its MCP Server Trigger configuration.
2. Use the MCP endpoint generated for your own n8n instance.
3. Open the main AI Travel Assistant workflow.
4. Configure the MCP Client Tool to connect to that endpoint.
5. Do not assume an exported endpoint or a localhost URL will work on another machine or hosted n8n instance.
6. Activate the server workflow if required by your chosen n8n endpoint mode, then test the connection.
3. Configure API credentials
- Groq: configure the model credential in the Groq Chat Model node.
- OpenTripMap: configure your own API key for the hotel, restaurant, and attraction search nodes.
- Google Sheets: connect Google Sheets credentials and select the correct spreadsheet and worksheet for both preference tools.
Open-Meteo endpoints are configured in the workflow for geocoding and weather. Confirm that the destination coordinates returned by find_destination are passed to the relevant tools.
4. Configure travel preferences (optional)
The preference tools expect a Google Sheet with these columns:
user_id | budget | hotel_type | transport | diet | activities | updated_at
Select the correct spreadsheet and worksheet in save_preference and get_preferences. Confirm that both tools use the same user_id matching convention. Test saving and retrieving a preference before relying on personalization.
5. Test the assistant
Try prompts such as:
- Plan a 3-day trip to Cox's Bazar.
- What is the current weather in Kathmandu?
- Find attractions and restaurants near Bandarban.
- Suggest places to stay for a trip to Sylhet.
- What is the exchange rate from USD to BDT?
- I prefer mid-range hotels and bus transport. Please save these preferences.
Check the n8n execution details to verify that the correct MCP tools ran and that the response is based on their returned data.
Example Response Sections
Depending on the user's request and the data returned by the tools, the assistant may organize its response into:
- Trip overview
- Weather information
- Accommodation-related places
- Restaurants and food places
- Tourist attractions
- Budget and currency information
- Travel tips and final recommendation
Only include information supported by the tool results. If a tool fails or returns no data, the assistant should state that limitation rather than make up details.
Troubleshooting
- MCP connection error: verify that the server workflow is available and the client uses the correct endpoint for your own n8n instance.
- Model error: check the Groq credential, selected model availability, and account quota.
- Places tools fail: check the OpenTripMap API key, query parameters, and service limits.
- Weather results missing: check that geocoding returned coordinates and those coordinates reach get_weather.
- Preferences do not persist: check Google Sheets permissions, spreadsheet/worksheet selection, column headers, and user_id matching.
- Unexpected destination in the answer: inspect the execution data and tool inputs; confirm the actual user-provided destination is passed through each step.
Repository Files
- AI Travel Assistant.json — main AI chat workflow export.
- Travel Info MCP Server.json — MCP server and travel-tool workflow export.
- AI TRAVEL ASSISTENT workflow.png — main workflow screenshot.
- Travel Info MCP Server.png — MCP server workflow screenshot.
- MCP Server Information.docx — setup and node-configuration notes.
- Project_Explanation.docx — project explanation and design notes.
- Travel Preferences.xlsx — example travel-preference spreadsheet.
- Recodring Vedio.mp4 — project demonstration recording.
Security Notes
- Never commit API keys, access tokens, passwords, or private credentials.
- Review exported workflow JSON files before publishing them in a public repository. Replace sensitive values with placeholders and rotate any key that may already have been exposed.
- A local MCP URL or endpoint identifier from one n8n installation should not be treated as a reusable public endpoint.
Project Status
The workflows and project documentation are included in this repository. The workflow requires the user's own API credentials, MCP endpoint configuration, and Google Sheets setup (if using preferences). Results and successful execution depend on those configurations and external service availability.
