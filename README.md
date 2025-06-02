#5th program
Execute Aggregation operations ($avg, $min,$max, $push, $addToSet etc.). students encourage to execute several queries to demonstrate various aggregation operators)

ample Collection: students
{
  "_id": 1,
  "name": "Alice",
  "class": "10A",
  "subject": "Math",
  "marks": 85
}
{
  "_id": 2,
  "name": "Bob",
  "class": "10A",
  "subject": "Math",
  "marks": 92
}
{
  "_id": 3,
  "name": "Charlie",
  "class": "10B",
  "subject": "Math",
  "marks": 78
}
{
  "_id": 4,
  "name": "David",
  "class": "10B",
  "subject": "Math",
  "marks": 88
}
{
  "_id": 5,
  "name": "Eve",
  "class": "10A",
  "subject": "Math",
  "marks": 92
}

Aggregation Queries
$avg — Average Marks per Class
db.students.aggregate([
  {
    $group: {
      _id: "$class",
      averageMarks: { $avg: "$marks" }
    }
  }
])
Output:
{ "_id": "10A", "averageMarks": 89.666... }
{ "_id": "10B", "averageMarks": 83 }









$min and $max — Minimum and Maximum Marks per Class
db.students.aggregate([
  {
    $group: {
      _id: "$class",
      minMarks: { $min: "$marks" },
    maxMarks: { $max: "$marks" }
    }
  }
])
Output:
{ "_id": "10A", "minMarks": 85, "maxMarks": 92 }
{ "_id": "10B", "minMarks": 78, "maxMarks": 88 }

$push — Push All Student Names per Class
db.students.aggregate([
  {
    $group: {
      _id: "$class",
      students: { $push: "$name" }
    }
  }
])

Output:
{ "_id": "10A", "students": ["Alice", "Bob", "Eve"] }
{ "_id": "10B", "students": ["Charlie", "David"] }




$addToSet — Get Unique Marks per Class
db.students.aggregate([
  {
    $group: {
      _id: "$class",
      uniqueMarks: { $addToSet: "$marks" }
    }
  }
])
Output:
{ "_id": "10A", "uniqueMarks": [85, 92] }
{ "_id": "10B", "uniqueMarks": [78, 88] }

Combined Example — Class Summary
db.students.aggregate([
  {
    $group: {
      _id: "$class",
      average: { $avg: "$marks" },
      highest: { $max: "$marks" },
      lowest: { $min: "$marks" },
      studentNames: { $push: "$name" },
      uniqueMarks: { $addToSet: "$marks" }
    }
  }
])

Output:
{
  "_id": "10A",
  "average": 89.67,
  "highest": 92,
  "lowest": 85,
  "studentNames": ["Alice", "Bob", "Eve"],
  "uniqueMarks": [85, 92]
}




