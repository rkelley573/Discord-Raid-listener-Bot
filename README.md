
# Discord Bot with Twitch and Warcraft Logs Integration

This Discord bot integrates with Twitch and Warcraft Logs to provide detailed information about recent boss pulls and corresponding Twitch broadcasts.

## Setup Instructions

Follow these steps to set up the bot in your environment. I use a Windows Subsystem for Linux using Ubuntu 24.04 and Bash, so your process mary vary slightly

### 1. Clone the Repository

First, clone the repository to your local machine:

### 2. Create and Activate a Virtual Environment

    python3 -m venv myenv
### and
    source myenv/bin/activate

### 3. Install Python Packages (listed in requirements.txt)
    pip install -r requirements.txt

### 4. Setup your tokens, ID's, and secrets in the .env file
On discord you need to create a new application, setup permissions, put it in your server and get the Discord token. @Indently on youtube has a great video for this https://www.youtube.com/watch?v=UYJDKSah-Ww, huge credit to him.

On Twitch you need to make a twitch developer account and go to your developer console. Make a new application and give it a name. For the redirect URL since you will host locally you can just have https://127.0.0.1:5000/ or whatever port you want to use. Select chat bot category and then click confidential. You can then get your client ID, client secret. For the access token Twitch uses Oauth 2.0 so you can read their documentation on how to get one, but I made a POST request with 

    curl -X POST "https://id.twitch.tv/oauth2/token" \
    -H "Content-Type: application/x-www-form-urlencoded" \
    -d "client_id=YOUR_CLIENT_ID" \
    -d "client_secret=YOUR_CLIENT_SECRET" \
    -d "grant_type=client_credentials"

replacing the client Id and client secret youll get back a JSON response that includes the access token.

For Warcraftlogs, you need to go to warcraftlogs client management page and make a client. Again call it what you want, use whatever redirect URL (http://127.0.0.1:5000/callback for example) and click create. Youll then have access to your token and can find it in your personal settings page if you scroll all the way down and its listed as "V1 Client Key: "

### 5. Configuration

To configure the bot, you need to edit the `config.json` file in the project root. The following settings are available:

- **`TARGET_CHANNEL_ID`**: The ID of the Discord channel where the bot should listen. To get this, right-click the channel in Discord and select "Copy ID."
- **`GUILD_NAME`**: The name of your guild (e.g., `"Fallen Empire"`).
- **`SERVER_NAME`**: The name of your server (e.g., `"MalGanis"`).
- **`SERVER_REGION`**: The region of your server (e.g., `"US"`).
- **`COMMAND_PREFIX`**: The command prefix the bot will listen for. The default is `"!bm"`, but you can change it to something else like `"!pull"`.

Here’s an example `config.json` file:

```json
{
    "TARGET_CHANNEL_ID": "1273702725678399578",
    "GUILD_NAME": "Fallen Empire",
    "SERVER_NAME": "MalGanis",
    "SERVER_REGION": "US",
    "COMMAND_PREFIX": "!bm"
}

```
Youll also need to create a .env file to store your tokens, ID's and secrets in.
Run
    
    cp .env.example .env
and you should get a .env file where you can input your tokens, ID's and secrets. It should look like this
```
DISCORD_TOKEN = XXXXXXXXXXX
TWITCH_ACCESS_TOKEN = XXXXXXXXXXX
TWITCH_CLIENT_ID = XXXXXXXXX
TWITCH_CLIENT_SECRET = XXXXXXXXXXXXX
TWITCH_USERNAME = XXXXXXXXXX
LOGS_TOKEN = XXXXXXXXXXX
```

### 6. Running the bot
I use Windows Subsystem for Linux with Ubuntu 24.04, if you are using a bash terminal, just type python3 main.py and you should see the bot coming online in the command terminal. The bot should also now appear online in discord. Make sure you give the bot the roles required to see and post in whatever channel you want it to listen to. Now go to the channel you set it up for, use your command, and while logging and streaming, it should give you the last pull in WCL and a link to your twitch stream timestamped to that pull. Once your done streaming and logging, it will pull the last fight log and the stream associated.

### Contributions
Again, huge thanks to @Idently on youtube for help with the discord bot setup, @Jtk.21 on discord for help with the retreival of fight start times and the offset used for twitch
and OpenAI for formatting, syntax checking, and help with error checking. 

### License
This project is licensed under the MIT License - see the LICENSE file for details.
