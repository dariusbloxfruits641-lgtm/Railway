import discord
from discord import app_commands
from datetime import timedelta

class MyClient(discord.Client):
    def __init__(self):
        intents = discord.Intents.default()
        intents.members = True
        super().__init__(intents=intents)
        self.tree = app_commands.CommandTree(self)

    async def setup_hook(self):
        await self.tree.sync()

client = MyClient()

@client.tree.command(name="ban", description="Bannir un membre")
@app_commands.checks.has_permissions(ban_members=True)
async def ban(interaction: discord.Interaction, membre: discord.Member, raison: str = "Aucune raison"):
    await membre.ban(reason=raison)
    await interaction.response.send_message(
        f"{membre.mention} a été banni. Raison : {raison}"
    )

@client.tree.command(name="mute", description="Rendre un membre muet temporairement")
@app_commands.checks.has_permissions(moderate_members=True)
async def mute(interaction: discord.Interaction, membre: discord.Member, minutes: int):
    await membre.timeout(timedelta(minutes=minutes))
    await interaction.response.send_message(
        f"{membre.mention} a été mute pendant {minutes} minute(s)."
    )

@client.tree.command(name="avertir", description="Avertir un membre")
@app_commands.checks.has_permissions(manage_messages=True)
async def avertir(interaction: discord.Interaction, membre: discord.Member, raison: str):
    await interaction.response.send_message(
        f"⚠️ {membre.mention} a reçu un avertissement.\nRaison : {raison}"
    )

client.run("MTUxMDAzMzI5OTk0NTU1NDEwMA.Gk9bUH.O4C_Tia7K3QjE1zL_gAB5akTt0h5yUmDJphvIA")
