const yt = require('ytdl-core');

client.on('message', message => {
  var args = message.content.split (" ");
  if (message.content.startsWith('++play')) {
    const voiceChannel = message.member.voiceChannel;
    if (!voiceChannel) {
      return message.reply("Please be in a voice channel first!");
    }
    voiceChannel.join()
     .then(connection => {
        yt.getInfo(args[1], (err, info) => {
          if (err) {
            connection.disconnect();
            return message.reply("Couldn't find that video");
          }
        const name = info.title;
        const stream = yt.downloadFromInfo(info, { filter: "audioonly" });
        const dispatcher = connection.playStream(stream)
          .once("start", () => {
            message.reply(`Now playing ${name}`);
          })
          .once("end", () => {
            connection.disconnect();
          });
      });
    });
  }
});

//If there any errors please tell me!
