### 1. Set up your environment variables:

Before running the bot, set up the necessary environment variables:

For UNIX-based systems (like Linux or macOS):
```bash
export OPENAI_API_KEY='your_openai_api_key'
export DISCORD_BOT_TOKEN='your_discord_bot_token'
```

For Windows (Command Prompt):
```bash
set OPENAI_API_KEY=your_openai_api_key
set DISCORD_BOT_TOKEN=your_discord_bot_token
```

For Windows (PowerShell):
```bash
$env:OPENAI_API_KEY = "your_openai_api_key"
$env:DISCORD_BOT_TOKEN = "your_discord_bot_token"
```

### 2. Update the bot code:

```python
import nextcord
from nextcord.ext import commands
import openai
import os

# Extract API Key and Bot Token from environment variables
openai_api_key = os.getenv('OPENAI_API_KEY')
discord_bot_token = os.getenv('DISCORD_BOT_TOKEN')

if not openai_api_key or not discord_bot_token:
    print("Ensure both OPENAI_API_KEY and DISCORD_BOT_TOKEN environment variables are set.")
    sys.exit(1)

# Initialize OpenAI and Nextcord APIs
openai.api_key = openai_api_key
intents = nextcord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix='/', case_insensitive=True, intents=intents)

@bot.event
async def on_ready():
    print(f'We have logged in as {bot.user}')

@bot.slash_command()
async def csh(interaction: nextcord.Interaction, question: str):
    """Ask a question to the Python Wizard of CSH"""
    try:
        response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo-16k",
            messages=[
                {"role": "system", "content": "You are a helpful python coder. But will also have conversations with people about any no illicit topic they would like to discuss."},
                {"role": "user", "content": question}
            ]
        )

        await interaction.response.send_message(response['choices'][0]['message']['content'].strip())
    except Exception as e:
        await interaction.response.send_message(f"Error: {e}")

# Run the bot
bot.run(discord_bot_token)
```

### Get your Bot Token, generate your OAuthURL etc... I aint gonna spoonfeed you right now...
