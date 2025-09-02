// filename: server.js

const express = require("express");
const bodyParser = require("body-parser");
const app = express();
const PORT = 5000;

app.use(bodyParser.json());

// Temporary in-memory database
let students = [
  { id: 1, name: "Om", age: 20 },
  { id: 2, name: "Aarav", age: 22 },
];

// ✅ GET all students
app.get("/students", (req, res) => {
  res.json(students);
});

// ✅ POST new student
app.post("/students", (req, res) => {
  const newStudent = {
    id: students.length + 1,
    name: req.body.name,
    age: req.body.age,
  };
  students.push(newStudent);
  res.status(201).json(newStudent);
});

// ✅ PUT update student by ID
app.put("/students/:id", (req, res) => {
  const { id } = req.params;
  const { name, age } = req.body;
  const student = students.find((s) => s.id === parseInt(id));

  if (!student) return res.status(404).json({ message: "Student not found" });

  student.name = name || student.name;
  student.age = age || student.age;

  res.json(student);
});

// ✅ DELETE student by ID
app.delete("/students/:id", (req, res) => {
  const { id } = req.params;
  students = students.filter((s) => s.id !== parseInt(id));
  res.json({ message: "Student deleted successfully" });
});

// Start server
app.listen(PORT, () => {
  console.log(`🚀 Server running on http://localhost:${PORT}`);
});
