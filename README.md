# AI Travel Assistant (n8n + Groq + MCP + Google Sheets)

## Project Overview

The AI Travel Assistant is an automation project built with n8n, Groq, MCP (Model Context Protocol), and Google Sheets. It helps users find travel information such as destinations, weather, hotels, restaurants, tourist attractions, and currency exchange rates.

The assistant can also save and retrieve travel preferences using Google Sheets.

## Workflows Included

- **AI Travel Assistant:** Handles user queries and connects the AI model with travel information tools.
- **Travel Info MCP Server:** Provides tools for retrieving travel information and managing user preferences.

## How It Works

1. The user sends a travel-related question to the AI Travel Assistant.
2. The Groq Chat Model processes the request.
3. The assistant uses the appropriate MCP tools to retrieve relevant information.
4. The assistant returns the results to the user.
5. Google Sheets stores and retrieves travel preferences when required.

## MCP Tools

| Tool | Purpose |
|---|---|
| `find_destination` | Finds information about travel destinations. |
| `get_weather` | Retrieves weather information. |
| `search_hotels` | Searches for hotel information. |
| `find_restaurant` | Finds restaurant information. |
| `search_attractions` | Searches for tourist attractions. |
| `get_exchange_rate` | Retrieves currency exchange rates. |
| `save_preference` | Saves travel preferences. |
| `get_preferences` | Retrieves saved travel preferences. |

## 1. Configure the Groq Chat Model

Configure the Groq Chat Model in n8n and connect it to the AI Travel Assistant workflow.

Add the required Groq API credentials to enable the model to process user requests.

## 2. Configure the MCP Server

Import the `Travel Info MCP Server.json` workflow into n8n.

Configure the MCP server and connect its endpoint to the AI Travel Assistant workflow. Ensure the required MCP tools are available.

## 3. Configure Travel Information APIs

Configure the required API credentials and endpoints used by the travel information tools.

Ensure that the services used for weather, destination searches, attractions, and currency exchange are configured correctly before testing.

## 4. Configure Google Sheets Preferences

Connect Google Sheets to store and retrieve travel preferences.

Configure the required Google account credentials and spreadsheet access. Ensure the spreadsheet structure matches the fields expected by the workflow.

## How to Test the Assistant

1. Import both workflow JSON files into n8n.
2. Configure the Groq credentials and required API access.
3. Connect and activate the MCP server.
4. Configure the Google Sheets integration.
5. Start the AI Travel Assistant and send a travel-related query.
6. Verify the returned information and test the preference-saving tools.

## Example Queries

- Find tourist attractions in Cox's Bazar.
- What is the weather like at my destination?
- Search for hotels and restaurants in a destination.
- Convert an amount from one currency to another.
- Save my travel preferences.
- Retrieve my saved travel preferences.

## Project Files

- `AI Travel Assistant.json` — AI Travel Assistant workflow.
- `Travel Info MCP Server.json` — MCP server workflow.
- `AI TRAVEL ASSISTENT workflow.png` — AI workflow screenshot.
- `Travel Info MCP Server.png` — MCP server screenshot.
- `MCP Server Information.docx` — MCP server documentation.
- `Project_Explanation.docx` — Project explanation.
- `Travel Preferences.xlsx` — Travel preferences spreadsheet.
- `Recodring Vedio.mp4` — Project recording.

## Notes and Assumptions

- Configure your own API credentials and service endpoints before running the workflows.
- Keep API keys, access tokens, and other credentials private.
- Search results do not guarantee current availability, prices, or confirmed bookings.
- Use your own active n8n MCP endpoint when connecting the workflows.
