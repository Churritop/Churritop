const { GatewayIntentBits, Client, EmbedBuilder, Collection } = require('discord.js');
const client = new Client({
    intents: [
        GatewayIntentBits.Guilds,
        GatewayIntentBits.GuildMembers,
        GatewayIntentBits.GuildMessages,
        GatewayIntentBits.MessageContent
    ]
});
const config = require(`${process.cwd()}/config.json`);
const fs = require('fs');
const { fileURLToPath } = require('url');
require('colors');

client.on('ready', () => {
    console.log('¡Bot Iniciado!'.blue)
})
client.login(config.token)

client.on('messageCreate', async (message) => {
    if(message.author.bot || !message.guild || message.channel.type === 'dm') return;

    var prefix = config.prefix

    if(!message.content.startsWith(prefix)) return;

    const args = message.content.slice(prefix.length).trim().split(/ +/g);
    const command = args.shift().toLowerCase();
 
    client.color = config.color;

    let cmd  = cliient.commands.find((c) => c.name === command || (c.alias && c.alias.includes(command)));
    if(cmd) {
        cmd.run(client, message, args);
    }
})

client.commands = new Collection();

fs.readdirSync('./commands').forEach((dir) => {
    const commnads = fs.readdirSync(`./comands/${dir}/`).filter((File) => file.endsWith('js'));
    for (let file of commands) {
        let command = require(`./commands/${dir}/${file}/`);
        console.log(`Comandos cargados - ${file} cargado`.blue)
        client.commnads.set(command.name, command);
    }
});
