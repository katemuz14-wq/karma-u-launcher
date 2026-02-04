<!DOCTYPE html>
<html>
<head>
    <title>Office Hub</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/4.0.1/socket.io.js"></script>
</head>

<body style="margin:0; background:black; color:white; font-family:Arial;">

<div style="background:red; padding:15px; text-align:center;">
    <h2>🔥 Office Control Center</h2>
</div>

<div style="display:flex;">

    <!-- Upload area -->
    <div style="width:50%; padding:20px; border-right:2px solid red;">
        <h3 style="color:red;">Upload File</h3>

        <form id="uploadForm" method="POST" action="/upload" enctype="multipart/form-data">
            <input type="file" name="file" style="color:white;"><br><br>
            <button type="submit" style="background:red; color:white; padding:10px;">Upload</button>
        </form>

        <div id="uploadResult" style="margin-top:20px;"></div>
    </div>

    <!-- Chat area -->
    <div style="width:50%; padding:20px;">
        <h3 style="color:red;">Office Chat Room</h3>

        <div id="messages" style="height:350px; overflow-y:scroll; background:#111; padding:10px; border:1px solid red;"></div>
        <br>

        <input id="msg" placeholder="Type message..."
        style="width:70%; padding:10px; background:black; border:1px solid red; color:white;">
        
        <button id="sendBtn"
        style="padding:10px; background:red; color:white; border:none;">Send</button>
    </div>

</div>

<script>
    // File upload result
    const uploadForm = document.getElementById("uploadForm");
    const uploadResult = document.getElementById("uploadResult");

    uploadForm.onsubmit = async (e) => {
        e.preventDefault();
        let data = new FormData(uploadForm);
        let res = await fetch("/upload", { method: "POST", body: data });
        uploadResult.innerHTML = await res.text();
    };

    // Chat system
    var socket = io();
    var sendBtn = document.getElementById("sendBtn");
    var msg = document.getElementById("msg");
    var messages = document.getElementById("messages");

    sendBtn.onclick = () => {
        if (msg.value.trim() !== "") {
            socket.send(msg.value);
            msg.value = "";
        }
    };

    socket.on("message", data => {
        messages.innerHTML += "<p>" + data + "</p>";
        messages.scrollTop = messages.scrollHeight;
    });
</script>

</body>
</html>
