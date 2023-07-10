const Discord = require('discord.js');
const client = new Discord.Client();

const token = 'YOUR_DISCORD_API_TOKEN'; // Replace with your Discord API token
const server1Id = '1078918824629370890'; // Replace with the ID of Server 1
const server2Id = '1127825385283923989'; // Replace with the ID of Server 2

client.on('ready', () => {
  console.log(`Logged in as ${client.user.tag}`);
});

client.on('guildMemberAdd', async (member) => {
  if (member.guild.id !== server2Id) return; // Check if the user joined Server 2

  try {
    const server1 = client.guilds.cache.get(server1Id); // Get Server 1 from cache
    const server1Member = server1.members.cache.get(member.user.id); // Get the member's data in Server 1

    if (!server1Member) {
      console.log(`Member not found in Server 1: ${member.user.tag}`);
      return;
    }

    server1Member.roles.cache.forEach((role) => {
      const correspondingRole = member.guild.roles.cache.find(
        (r) => r.name === role.name
      );
      if (correspondingRole) {
        member.roles.add(correspondingRole);
      }
    });

    console.log(`Assigned roles for ${member.user.tag}`);
  } catch (error) {
    console.error('Error assigning roles:', error);
  }
});

client.login(token);

