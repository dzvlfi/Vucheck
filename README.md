# Vucheck

## Purpose

`mcp_vucheck.py` is a Python application designed to fetch and summarize security vulnerabilities (CVEs) for a specified software product and then post the summary to a designated Slack channel. It leverages the Model Context Protocol (MCP) to interact with a Slack server, a CVE details API to retrieve vulnerability information, and OpenAI's GPT model to generate concise summaries.

## Setup Instructions

Before running the application, ensure you have the following prerequisites and have configured the necessary settings:

1.  **Python Environment:** Make sure you have Python 3.7 or higher installed on your system.

2.  **Install Dependencies:** Install the required Python libraries using pip:
    ```bash
    pip install mcp openai requests
    ```

3.  **Configuration File (`config.json`):** Create a `config.json` file in the same directory as `mcp_vucheck.py` with the following structure. Replace the placeholder values with your actual credentials and IDs:
    ```json
    {
      "OPENAI_API_KEY": "YOUR_OPENAI_API_KEY",
      "SLACK_BOT_TOKEN": "YOUR_SLACK_BOT_TOKEN",
      "SLACK_TEAM_ID": "YOUR_SLACK_TEAM_ID",
      "SLACK_CHANNELID_ALL": "YOUR_SLACK_CHANNEL_ID",
      "CVE_ACCESS_TOKEN": "YOUR_CVE_DETAILS_API_TOKEN"
    }
    ```
    * `OPENAI_API_KEY`: Your OpenAI API key. You can obtain this from the OpenAI platform.
    * `SLACK_BOT_TOKEN`: Your Slack bot token. You'll need to create a Slack app and bot user to get this. Ensure the bot has the `chat:write` scope in the channels it should post to.
    * `SLACK_TEAM_ID`: Your Slack team ID.
    * `SLACK_CHANNELID_ALL`: The ID of the Slack channel where the vulnerability summaries will be posted.
    * `CVE_ACCESS_TOKEN`: Your API token for the CVE Details API (if required). Check the CVE Details API documentation for whether an API key is needed.

4.  **MCP Slack Server:** Ensure that the `@modelcontextprotocol/server-slack` MCP server is running and accessible. This application relies on it to interact with Slack. You might need to install it using npm if you haven't already:
    ```bash
    npm install -g @modelcontextprotocol/server-slack
    ```

## Usage Examples

1.  **Run the Application:** Open your terminal, navigate to the directory containing `mcp_vucheck.py` and `config.json`, and execute the script:
    ```bash
    python mcp_vucheck.py
    ```

2.  **Input Product Name:** The application will prompt you to enter the name of the software product you want to check for vulnerabilities:
    ```
    🔧 Input Product Name:
    ```
    Enter the product name (e.g., `linux kernel`) and press Enter.

3.  **Vulnerability Analysis and Slack Posting:** The script will then:
    * Connect to the MCP Slack server.
    * Query the CVE Details API for vulnerabilities related to the provided product name.
    * Print the raw CVE data received from the API.
    * Use the OpenAI GPT model to summarize the vulnerability information, including the product name, total number of CVEs, average risk score, product type, and details of the top 5 CVEs (if available).
    * Print the generated vulnerability summary in the console.
    * Post the summarized information to the Slack channel specified in your `config.json`.
    * Print a confirmation message upon successful posting to Slack.

## Limitations or Known Issues

* **CVE Details API Rate Limiting:** The application might be subject to rate limits imposed by the CVE Details API. If you make too many requests in a short period, you might encounter errors. Consider implementing error handling and delays if this becomes an issue.
* **OpenAI API Costs:** Using the OpenAI GPT-4 model incurs costs based on token usage. Be mindful of your OpenAI API usage and billing.
* **MCP Server Dependency:** This application is tightly coupled with the `@modelcontextprotocol/server-slack` server. The server must be running and configured correctly for the application to function.
* **Error Handling:** While some basic error handling is included (e.g., for API requests and Slack posting), more robust error handling and logging could be implemented for production use.
* **Limited CVE Results:** The application currently fetches only the first 10 results from the CVE Details API. For products with a large number of vulnerabilities, this might not be exhaustive.
* **GPT Summary Quality:** The quality and format of the GPT-generated summary depend on the prompt and the data returned by the CVE Details API. Improvements to the prompt might be necessary for better results.
* **Configuration Management:** The configuration is currently loaded from a `config.json` file. More sophisticated configuration management techniques could be used for larger deployments.
