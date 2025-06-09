# Rohit-Dbms-Project
DBMS Mini Project on Event Management System by ROHITH
=====================================
🟥 1. DATABASE CREATION (MySQL)
=====================================

-- 🔸 Create User Table
CREATE TABLE User (
  user_id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(100),
  email VARCHAR(100) UNIQUE,
  phone VARCHAR(15),
  role ENUM('Attendee', 'Organizer')
);

-- 🔸 Create Venue Table
CREATE TABLE Venue (
  venue_id INT PRIMARY KEY AUTO_INCREMENT,
  venue_name VARCHAR(100),
  location VARCHAR(100),
  capacity INT
);

-- 🔸 Create Event Table
CREATE TABLE Event (
  event_id INT PRIMARY KEY,
  event_name VARCHAR(100),
  event_date DATE,
  venue_id INT,
  organizer_id INT,
  FOREIGN KEY (venue_id) REFERENCES Venue(venue_id),
  FOREIGN KEY (organizer_id) REFERENCES User(user_id)
);

-- 🔸 Create Registration Table
CREATE TABLE Registration (
  reg_id INT PRIMARY KEY AUTO_INCREMENT,
  user_id INT,
  event_id INT,
  status ENUM('Pending', 'Confirmed'),
  FOREIGN KEY (user_id) REFERENCES User(user_id),
  FOREIGN KEY (event_id) REFERENCES Event(event_id)
);

-- 🔸 Create Payment Table
CREATE TABLE Payment (
  payment_id INT PRIMARY KEY AUTO_INCREMENT,
  user_id INT,
  event_id INT,
  amount DECIMAL(10,2),
  status ENUM('Paid', 'Pending'),
  FOREIGN KEY (user_id) REFERENCES User(user_id),
  FOREIGN KEY (event_id) REFERENCES Event(event_id)
);

-- 🔸 Create Feedback Table
CREATE TABLE Feedback (
  feedback_id INT PRIMARY KEY AUTO_INCREMENT,
  user_id INT,
  event_id INT,
  rating INT CHECK (rating BETWEEN 1 AND 5),
  comments TEXT,
  FOREIGN KEY (user_id) REFERENCES User(user_id),
  FOREIGN KEY (event_id) REFERENCES Event(event_id)
);

=====================================
🟨 2. FRONTEND CODE (HTML + CSS + JS)
=====================================

<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <title>Event Management System</title>
  <style>
    body { font-family: Arial; background: #f4f4f4; }
    header { background: #007bff; color: #fff; padding: 1rem; text-align: center; }
    nav { background: #333; padding: 0.5rem; text-align: center; }
    nav a { color: #fff; margin: 0 1rem; text-decoration: none; }
    section { padding: 2rem; display: none; }
    .active { display: block; }
    table, th, td { border: 1px solid #ccc; padding: 0.5rem; border-collapse: collapse; }
  </style>
</head>
<body>
  <header><h1>Event Management System</h1></header>
  <nav>
    <a href="#" onclick="showSection('home')">Home</a>
    <a href="#" onclick="showSection('add')">Add Event</a>
    <a href="#" onclick="showSection('view')">View Events</a>
  </nav>

  <section id="home" class="active">
    <h2>Welcome</h2>
    <p>Manage events and participants with ease.</p>
  </section>

  <section id="add">
    <h2>Add Event</h2>
    <form id="eventForm">
      <input type="text" id="eventName" placeholder="Event Name" required />
      <input type="date" id="eventDate" required />
      <input type="text" id="eventVenue" placeholder="Venue" required />
      <button type="submit">Add</button>
    </form>
  </section>

  <section id="view">
    <h2>Event List</h2>
    <table>
      <thead><tr><th>Name</th><th>Date</th><th>Venue</th></tr></thead>
      <tbody id="eventTable"></tbody>
    </table>
  </section>

  <script>
    const events = [];
    function showSection(id) {
      document.querySelectorAll('section').forEach(sec => sec.classList.remove('active'));
      document.getElementById(id).classList.add('active');
    }

    document.getElementById('eventForm').addEventListener('submit', function(e) {
      e.preventDefault();
      const name = document.getElementById('eventName').value;
      const date = document.getElementById('eventDate').value;
      const venue = document.getElementById('eventVenue').value;
      events.push({ name, date, venue });
      renderEvents();
      showSection('view');
    });

    function renderEvents() {
      const table = document.getElementById('eventTable');
      table.innerHTML = '';
      events.forEach(e => {
        table.innerHTML += `<tr><td>${e.name}</td><td>${e.date}</td><td>${e.venue}</td></tr>`;
      });
    }
  </script>
</body>
</html>

