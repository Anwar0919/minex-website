.auth-panel {
  max-width: 600px;
  margin: 25px auto;
  padding: 24px;
  background: #101a2b;
  border: 1px solid #22314a;
  border-radius: 18px;
}
.auth-panel input {
  width: 100%;
  padding: 13px;
  margin: 8px 0;
  border-radius: 10px;
  border: 1px solid #22314a;
  background: #080d18;
  color: white;
}
.auth-panel button {
  padding: 12px 18px;
  margin: 8px 5px 8px 0;
  border: 0;
  border-radius: 10px;
  background: #63e6a5;
  color: #07130e;
  font-weight: bold;
}
<script>
const getUsers = () =>
  JSON.parse(localStorage.getItem("minexUsers") || "{}");

function signup() {
  const name = document.getElementById("username").value.trim();
  const pass = document.getElementById("password").value;
  const msg = document.getElementById("authMessage");

  if (!name || !pass) {
    msg.textContent = "Username aur password enter karein.";
    return;
  }

  const users = getUsers();

  if (users[name]) {
    msg.textContent = "Username already exists.";
    return;
  }

  users[name] = { password: pass, points: 0, lastCheckin: "" };
  localStorage.setItem("minexUsers", JSON.stringify(users));

  msg.textContent = "Account created! Ab Login karein.";
}

function login() {
  const name = document.getElementById("username").value.trim();
  const pass = document.getElementById("password").value;
  const users = getUsers();

  if (!users[name] || users[name].password !== pass) {
    document.getElementById("authMessage").textContent =
      "Username ya password ghalat hai.";
    return;
  }

  localStorage.setItem("minexCurrentUser", name);
  showDashboard();
}

function showDashboard() {
  const name = localStorage.getItem("minexCurrentUser");
  const users = getUsers();

  if (!name || !users[name]) return;

  document.getElementById("auth").hidden = true;
  document.getElementById("dashboard").hidden = false;
  document.getElementById("memberName").textContent = name;
  document.getElementById("points").textContent = users[name].points;
}

function checkin() {
  const name = localStorage.getItem("minexCurrentUser");
  const users = getUsers();

  if (!name || !users[name]) return;

  const today = new Date().toISOString().slice(0, 10);

  if (users[name].lastCheckin === today) {
    document.getElementById("checkinMessage").textContent =
      "Aaj ka check-in already complete hai.";
    return;
  }

  users[name].points += 10;
  users[name].lastCheckin = today;

  localStorage.setItem("minexUsers", JSON.stringify(users));

  document.getElementById("checkinMessage").textContent =
    "Check-in successful! 10 demo points added.";
  showDashboard();
}

function logout() {
  localStorage.removeItem("minexCurrentUser");
  document.getElementById("auth").hidden = false;
  document.getElementById("dashboard").hidden = true;
}

showDashboard();
</script>
