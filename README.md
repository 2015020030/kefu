<!DOCTYPE html>
<html>
<head>
    <script src="https://code.jquery.com/jquery-1.9.1.js"></script>
    <script src="http://simplewebrtc.com/latest.js"></script>
    <script>
 
        var webrtc = new SimpleWebRTC({
            // the id/element dom element that will hold "our" video
            localVideoEl: 'localVideo',
            // the id/element dom element that will hold remote videos
            remoteVideosEl: 'remoteVideos',
            // immediately ask for camera access
            autoRequestMedia: true,
            //url:'http://111.172.238.250:8888'
            nick: '太阳城集团'
        });
 
 
 
        // we have to wait until it's ready
        webrtc.on('readyToCall', function () {
            // you can name it anything
            webrtc.joinRoom('room1');
 
            // Send a chat message
            $('#send').click(function () {
                var msg = $('#text').val();
                webrtc.sendToAll('chat', { message: msg, nick: webrtc.config.nick });
                $('#messages').append('<br>You:<br>' + msg + '\n');
                $('#text').val('');
            });
        });
 
        //For Text Chat ------------------------------------------------------------------
        // Await messages from others
        webrtc.connection.on('message', function (data) {
            if (data.type === 'chat') {
                console.log('chat received', data);
                $('#messages').append('<br>' + data.payload.nick + ':<br>' + data.payload.message+ '\n');
            }
        });
        
    </script>
    <style>
        #remoteVideos video {
            height: 150px;
        }
 
        #localVideo {
            height: 150px;
        }
    </style>
</head>
<body>
    <textarea id="messages" rows="5" cols="20"></textarea><br />
    <input id="text" type="text" />
    <input id="send" type="button" value="send" /><br />
    <video id="localVideo"></video>
    <div id="remoteVideos"></div>
</body>
</html>
