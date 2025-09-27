# RealTimeChatApplication

open vs code -> file -> open folder and create a new folder named "realtime_chat"
go to view -> terminal -> follow the commands 
=> mkdir server : server folder is created 
=> cd server : path is changed into server folder
=> npm init -y : initializing node.js project & creates package.json file inside server folder
=> npm install express socket.io cors : express for HTTP server socket.io for real time chat cors to allow frontend to connect.
                                        add "node_modules" and "packageloack.json" files in server folder
=> npm install -D nodemon : extra helper for development
in the "server" folder create "src" folder and even create "index.js" file in that folder.
Run the index.js code
run the following commands in the terminal
=> npm run dev : you would see message like "server running on http://localhost:3000"
then copy the link and paste it in the browser
the output would be like * Real time chat server is running *
in the terminal cd .. it comes to the previous folder
install socket.io client
=> npm install socket.io-client
note: some dependencies will be added to the file directory
create a simple "client.html" in the project root just for testing
donot come back to the server folder path
install live server for running the following commands in the terminal
=> npm install -g live-server
=> live-server
In the terminal you would be seeing a url like "http://127.0.0.1:8080"

# Client.html: 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Chat App Test</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap');

    /* ---------------- Body & Layout ---------------- */
    body {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      padding: 20px;
      font-family: 'Roboto', sans-serif;
      min-height: 100vh;
      background: linear-gradient(135deg, #f6d365, #fda085, #ff758c, #ff7eb3);
      background-size: 400% 400%;
      animation: gradientBG 15s ease infinite;
    }

    @keyframes gradientBG {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    #mainContainer {
      flex: 1;
      margin-right: 20px;
      padding: 30px;
      border-radius: 25px;
      background: rgba(255, 255, 255, 0.9);
      backdrop-filter: blur(15px);
      box-shadow: 0 15px 40px rgba(0,0,0,0.2);
      transition: transform 0.3s;
    }

    #mainContainer:hover { transform: translateY(-5px); }

    h2 { text-align: center; color: #000; font-size: 2em; margin-bottom: 30px; }

    /* ---------------- Buttons ---------------- */
    button {
      background: linear-gradient(135deg, #ff758c, #ff7eb3);
      border: none;
      color: white;
      border-radius: 12px;
      padding: 10px 20px;
      cursor: pointer;
      font-weight: 600;
      font-size: 1em;
      margin: 5px;
      transition: transform 0.3s, box-shadow 0.3s;
      box-shadow: 0 5px 15px rgba(0,0,0,0.2);
    }

    button:hover {
      transform: translateY(-3px) scale(1.05);
      box-shadow: 0 10px 20px rgba(0,0,0,0.3);
    }

    #backButton {
      background: linear-gradient(135deg, #ff4d4d, #ff6666);
    }

    #backButton:hover {
      transform: translateY(-3px) scale(1.05);
      box-shadow: 0 10px 20px rgba(255,0,0,0.3);
    }

    /* ---------------- Inputs ---------------- */
    input {
      padding: 10px 15px;
      border-radius: 15px;
      border: 1px solid rgba(0,0,0,0.3);
      outline: none;
      font-size: 1em;
      margin: 5px 0;
      background: rgba(255,255,255,0.8);
      color: #000;
      transition: all 0.3s;
    }

    input::placeholder { color: #333; }
    input:focus {
      border-color: #000;
      box-shadow: 0 0 10px rgba(0,0,0,0.3);
      background: rgba(255,255,255,1);
      color: #000;
    }

    /* ---------------- Status Message ---------------- */
    #statusMessage {
      font-weight: 500;
      color: #ff0000;
      text-align: center;
      margin: 15px 0;
      font-size: 1.1em;
    }

    /* ---------------- Room Sections ---------------- */
    #createJoinContainer, #roomActions, #chatSection {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-bottom: 20px;
    }

    hr {
      border: none;
      border-top: 2px solid rgba(0,0,0,0.3);
      margin: 20px 0;
      width: 80%;
    }

    #chatSection h3, #chatSection h4 { color: #000; margin: 5px 0; }

    /* ---------------- Chat Input ---------------- */
    #chatInputArea { display: flex; justify-content: center; width: 100%; margin-top: 15px; }
    #chatInputArea input { flex: 1; margin-right: 10px; }

    /* ---------------- Phone Box (Right Side) ---------------- */
    #phoneBox {
      width: 300px;
      height: 500px;
      border: 2px solid #333;
      border-radius: 20px;
      padding: 15px;
      background: linear-gradient(145deg, #ffffff, #e6e6e6);
      box-shadow: 0 10px 20px rgba(0,0,0,0.15);
      overflow-y: auto;
      transition: all 0.3s ease;
    }

    #phoneBox h3 { text-align: center; margin-bottom: 10px; color: #000; }

    /* ---------------- User List ---------------- */
    #userList { list-style: none; padding: 0; margin: 0; }
    #userList li {
      background: #ddd;
      margin: 5px 0;
      padding: 10px;
      border-radius: 15px;
      text-align: center;
      cursor: pointer;
      transition: all 0.3s ease;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    #userList li:hover { background: #b3d9ff; transform: translateX(5px); }
    #userList li.active { background: #2575fc; color: white; font-weight: bold; }

    .deleteBtn {
      background: #ff4d4d;
      border: none;
      padding: 5px 10px;
      border-radius: 8px;
      cursor: pointer;
      color: white;
    }

    /* ---------------- Chat Boxes ---------------- */
    .chatBox {
      margin-top: 15px;
      border-top: 1px solid #ccc;
      padding-top: 10px;
      opacity: 0;
      animation: fadeIn 0.5s forwards;
    }

    .chatBox h4 { margin: 0 0 5px 0; text-align: center; font-size: 14px; color: #000; }

    @keyframes fadeIn { to { opacity: 1; } }
    .messagesContainer { max-height: 150px; overflow-y: auto; padding: 5px; }

    .message {
      padding: 8px 12px;
      margin: 5px;
      border-radius: 15px;
      max-width: 80%;
      display: inline-block;
      clear: both;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      transform: translateY(20px);
      opacity: 0;
      animation: slideIn 0.3s forwards;
    }

    .my-message { background-color: #d1ffd6; float: right; text-align: right; animation-delay: 0.1s; }
    .other-message { background-color: #f0f0f0; float: left; text-align: left; animation-delay: 0.1s; }

    @keyframes slideIn { to { transform: translateY(0); opacity: 1; } }
  </style>
</head>
<body>
  <div id="mainContainer">
    <h2>Real Time Chat Application</h2>

    <div id="createJoinContainer">
      <button onclick="showCreateRoom()">Create Room</button>
      <button onclick="showJoinRoom()">Join Room</button>
      <button onclick="showRemoveUser()">Remove User</button>
      <button onclick="showRemoveRoom()">Remove Room</button>
    </div>

    <div id="roomActions" style="display:none;">
      <div id="createRoomDiv" style="display:none;">
        <input id="userName" placeholder="Enter your name" />
        <input id="newRoom" placeholder="Enter new room name" />
        <button onclick="createRoom()">Done</button>
      </div>
      <div id="joinRoomDiv" style="display:none;">
        <input id="joinRoomInput" placeholder="Enter existing room id" />
        <button onclick="joinExistingRoom()">Join</button>
      </div>
      <div id="removeUserDiv" style="display:none;">
        <ul id="allUsersList"></ul>
        <button onclick="cancelRemoveUser()">Cancel</button>
      </div>
      <div id="removeRoomDiv" style="display:none;">
        <ul id="allRoomsList"></ul>
        <button onclick="cancelRemoveRoom()">Cancel</button>
      </div>
    </div>

    <p id="statusMessage"></p>
    <hr>

    <div id="chatSection" style="display:none;">
      <h3>Room: <span id="currentRoom"></span></h3>
      <h4>Welcome, <span id="currentUser"></span></h4>
      <div id="chatInputArea" style="display:none;">
        <input id="message" placeholder="Enter message" />
        <button onclick="sendMessage()">Send</button>
      </div>
      <button id="joinRoomButton" style="display:none;" onclick="verifyJoinRoom()">Join Room</button>
      <button id="backButton" onclick="goBack()">Back</button>
    </div>
  </div>

  <div id="phoneBox">
    <h3>Hey !!!</h3>
    <ul id="userList"></ul>
    <div id="chatBoxes"></div>
  </div>

  <script src="https://cdn.socket.io/4.7.5/socket.io.min.js"></script>
  <script>
    const socket = io("http://localhost:3000");
    const storedRooms = JSON.parse(localStorage.getItem("createdRooms")) || [];
    const storedMembers = JSON.parse(localStorage.getItem("roomMembers")) || {};
    const createdRooms = new Set(storedRooms);
    const roomMembers = {};
    for (const [room, members] of Object.entries(storedMembers)) roomMembers[room] = new Set(members);

    let currentRoom = "";
    let currentUser = "";
    let selectedUser = "";

    function saveToLocalStorage() {
      localStorage.setItem("createdRooms", JSON.stringify([...createdRooms]));
      const membersObj = {};
      for (const room in roomMembers) membersObj[room] = [...roomMembers[room]];
      localStorage.setItem("roomMembers", JSON.stringify(membersObj));
    }

    // ---------------- Update User List ----------------
    function updateUserList() {
      const list = document.getElementById("userList");
      const chatBoxes = document.getElementById("chatBoxes");
      list.innerHTML = "";
      chatBoxes.innerHTML = "";
      if (roomMembers[currentRoom]) {
        roomMembers[currentRoom].forEach(user => {
          const li = document.createElement("li");
          li.textContent = user;
          li.onclick = () => {
            selectedUser = user;
            Array.from(list.children).forEach(c => c.classList.remove("active"));
            li.classList.add("active");
          };
          list.appendChild(li);

          const div = document.createElement("div");
          div.className = "chatBox";
          div.id = `chat-${user}`;
          const title = document.createElement("h4");
          title.textContent = user;
          div.appendChild(title);

          const messagesDiv = document.createElement("div");
          messagesDiv.className = "messagesContainer";
          messagesDiv.id = `messages-${user}`;
          div.appendChild(messagesDiv);

          chatBoxes.appendChild(div);
        });
      }
    }

    // ---------------- Show Remove User ----------------
    function showRemoveUser() {
      document.getElementById("roomActions").style.display = "block";
      document.getElementById("createRoomDiv").style.display = "none";
      document.getElementById("joinRoomDiv").style.display = "none";
      document.getElementById("removeRoomDiv").style.display = "none";
      document.getElementById("removeUserDiv").style.display = "block";

      const allUsersList = document.getElementById("allUsersList");
      allUsersList.innerHTML = "";
      for (const room in roomMembers) {
        roomMembers[room].forEach(user => {
          const li = document.createElement("li");
          li.textContent = `${user} (Room: ${room})`;

          const delBtn = document.createElement("button");
          delBtn.textContent = "🗑️";
          delBtn.className = "deleteBtn";
          delBtn.onclick = () => {
            if (confirm(`Are you sure you want to delete ${user}?`)) {
              roomMembers[room].delete(user);
              saveToLocalStorage();
              showRemoveUser();
              updateUserList();
            }
          };

          li.appendChild(delBtn);
          allUsersList.appendChild(li);
        });
      }
    }

    function cancelRemoveUser() {
      document.getElementById("removeUserDiv").style.display = "none";
      document.getElementById("roomActions").style.display = "none";
    }

    // ---------------- Show Remove Room ----------------
    function showRemoveRoom() {
      document.getElementById("roomActions").style.display = "block";
      document.getElementById("createRoomDiv").style.display = "none";
      document.getElementById("joinRoomDiv").style.display = "none";
      document.getElementById("removeUserDiv").style.display = "none";
      document.getElementById("removeRoomDiv").style.display = "block";

      const allRoomsList = document.getElementById("allRoomsList");
      allRoomsList.innerHTML = "";
      createdRooms.forEach(room => {
        const li = document.createElement("li");
        li.textContent = room;

        const delBtn = document.createElement("button");
        delBtn.textContent = "🗑️";
        delBtn.className = "deleteBtn";
        delBtn.onclick = () => {
          if (confirm(`Are you sure you want to delete room "${room}"?`)) {
            createdRooms.delete(room);
            delete roomMembers[room];
            saveToLocalStorage();
            showRemoveRoom();
            if (currentRoom === room) location.reload();
          }
        };

        li.appendChild(delBtn);
        allRoomsList.appendChild(li);
      });
    }

    function cancelRemoveRoom() {
      document.getElementById("removeRoomDiv").style.display = "none";
      document.getElementById("roomActions").style.display = "none";
    }

    // ---------------- Existing Functions ----------------
    function showCreateRoom() {
      document.getElementById("roomActions").style.display = "block";
      document.getElementById("createRoomDiv").style.display = "block";
      document.getElementById("joinRoomDiv").style.display = "none";
      document.getElementById("statusMessage").textContent = "";
    }

    function showJoinRoom() {
      document.getElementById("roomActions").style.display = "block";
      document.getElementById("createRoomDiv").style.display = "none";
      document.getElementById("joinRoomDiv").style.display = "block";
      document.getElementById("statusMessage").textContent = "";
    }

    function createRoom() {
      const roomID = document.getElementById("newRoom").value.trim();
      const userName = document.getElementById("userName").value.trim();
      if (!userName || !roomID) return alert("Enter all fields!");
      if (createdRooms.has(roomID)) return alert("Room exists!");

      createdRooms.add(roomID);
      roomMembers[roomID] = new Set([userName]);
      saveToLocalStorage();

      currentRoom = roomID;
      currentUser = userName;

      document.getElementById("statusMessage").textContent = `Room "${roomID}" created!`;
      document.getElementById("newRoom").value = "";
      document.getElementById("userName").value = "";

      socket.emit("join_room", currentRoom);
      document.getElementById("currentRoom").textContent = currentRoom;
      document.getElementById("currentUser").textContent = currentUser;
      document.getElementById("chatSection").style.display = "block";
      document.getElementById("createJoinContainer").style.display = "none";
      document.getElementById("roomActions").style.display = "none";
      document.getElementById("chatInputArea").style.display = "flex";

      updateUserList();
    }

    function joinExistingRoom() {
      const roomID = document.getElementById("joinRoomInput").value.trim();
      const userName = prompt("Enter Name:").trim();
      if (!roomID || !userName) return alert("Enter all fields!");
      if (!createdRooms.has(roomID)) return alert("Room does not exist!");

      if (!roomMembers[roomID]) roomMembers[roomID] = new Set();
      roomMembers[roomID].add(userName);
      saveToLocalStorage();

      currentRoom = roomID;
      currentUser = userName;

      document.getElementById("currentRoom").textContent = currentRoom;
      document.getElementById("currentUser").textContent = currentUser;
      document.getElementById("createJoinContainer").style.display = "none";
      document.getElementById("roomActions").style.display = "none";
      document.getElementById("chatSection").style.display = "block";
      document.getElementById("chatInputArea").style.display = "flex";

      socket.emit("join_room", currentRoom);
      updateUserList();
    }

    function sendMessage() {
      if (!selectedUser) return alert("Select a user first!");
      const msg = document.getElementById("message").value.trim();
      if (!msg) return;

      const data = { room: currentRoom, message: msg, sender: currentUser, recipient: selectedUser };
      socket.emit("send_message", data);

      displayMessage(data.sender, data.message, data.recipient === currentUser ? false : true);
      document.getElementById("message").value = "";
    }

    function displayMessage(sender, message, isOther) {
      const ul = document.getElementById(`messages-${sender}`);
      if (!ul) return;
      const div = document.createElement("div");
      div.className = isOther ? "other-message message" : "my-message message";
      div.textContent = `${message}`;
      ul.appendChild(div);
      ul.scrollTop = ul.scrollHeight;
    }

    socket.on("receive_message", (data) => {
      if (data.room === currentRoom) {
        displayMessage(data.sender, data.message, data.sender !== currentUser);
      }
    });

    function goBack() { window.location.href = "index.html"; }
  </script>
</body>
</html>












