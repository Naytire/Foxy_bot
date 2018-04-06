var tmi = require ('tmi.js');

var options = {
    options: {
      debug: true
    },
    connection: {
      cluster: "aws",
      reconnect: true
    },
    identity: {
        username: "Foxy_bot",
        password: "oauth:9erjw6ubi4b6zsp0yf3iwtzueup2wj"
    },
    channels: [naytire_lb]
};

var client = new tmi.client(options);
client.connect();

// User join to chat
client.on("join", function (channel, username, self) {
   client.action("naytire_lb", username + " , glad to see you!");
});

// Timeout function
function timeOut(message) {
    splitMSG = message.split(" ");
    timeoutUserName = splitMSG[1];
    timeoutDuration = splitMSG[2];
    client.timeout("naytire_lb", timeoutUserName, timeoutDuration);
    client.action('naytire_lb', timeoutUserName + ' now u have timeout mode! Duration: ' + timeoutDuration);
}

// Split command
function splitMessage(message) {
    if ((message.indexOf('!to')) !== -1){
        timeOut(message);
    } else if ((message.indexOf('!clear')) !== -1){
        client.clear("naytire_lb");
    }

}

// Commands
client.on('chat', function (channel, username, message, self) {

    if(username.username === "naytire_lb"){
        // Admin commands
        splitMessage(message);
    } else {
        // Users commands
        switch (message) {
            case "!fb":
                client.action('naytire_lb', 'your-fb-link-here');
                break;
            case "!twt":
                client.action('naytire_lb', 'your-twitter-link-here');
                break;
            default:
        }
    }
});
