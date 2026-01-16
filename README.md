# php


<?php
session_start();

if (!isset($_SESSION['todos'])) {
    $_SESSION['todos'] = [];
}

/* Add task */
if (isset($_POST['add']) && trim($_POST['task']) !== '') {
    $_SESSION['todos'][] = [
        'text' => htmlspecialchars($_POST['task']),
        'done' => false
    ];
}

/* Row actions */
if (isset($_POST['index'])) {
    $i = (int) $_POST['index'];

    if (isset($_POST['toggle'])) {
        $_SESSION['todos'][$i]['done'] = !$_SESSION['todos'][$i]['done'];
    }

    if (isset($_POST['update'])) {
        $_SESSION['todos'][$i]['text'] = htmlspecialchars($_POST['text']);
    }

    if (isset($_POST['delete'])) {
        array_splice($_SESSION['todos'], $i, 1);
    }
}
?>
<!DOCTYPE html>
<html>
<head>
<title>To-Do List</title>
<style>
/* Outer gradient (unchanged) */
body {
    margin: 0;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: "Segoe UI", Arial, sans-serif;

    background: linear-gradient(135deg,
        #574964,
        #9F8383,
        #C8AAAA,
        #FFDAB3
    );
}

/* Card now looks like ruled notebook paper */
.card {
    position: relative;
    width: 700px;
    padding: 40px 32px 32px 70px;
    border-radius: 18px;

    background-color: #fffdf9;
    background-image:
        linear-gradient(to bottom, rgba(87,73,100,0.15) 1px, transparent 1px),
        linear-gradient(to right, rgba(159,131,131,0.15) 1px, transparent 1px);
    background-size: 100% 34px, 34px 100%;

    box-shadow: 0 25px 45px rgba(0,0,0,0.35);
}

.card::before {
    content: "";
    position: absolute;
    left: 38px;
    top: 0;
    bottom: 0;
    width: 2px;
    background: rgba(159,131,131,0.5);
}

h2 {
    text-align: center;
    color: #574964;
    margin-bottom: 28px;
}


.add {
    display: flex;
    gap: 12px;
    margin-bottom: 26px;
    padding: 16px;
    border-radius: 12px;
    background: #FFDAB3;
}

.add input {
    flex: 1;
    padding: 14px;
    border-radius: 10px;
    border: 2px solid #9F8383;
    font-size: 15px;
}

.add button {
    background: #574964;
    color: #fff;
    border: none;
    padding: 14px 22px;
    border-radius: 10px;
    font-size: 15px;
    cursor: pointer;
}


table {
    width: 100%;
    border-collapse: collapse; /* true table grid */
}


th {
    text-align: left;
    font-size: 18px;
    color: #574964;
    
    padding-bottom: 6px;
}

td {
    background: #FFFFFF00;
    padding: 14px;
    border-radius: 0;
    border: 2px solid #9F8383; 	
}

/* Task input */
.task {
    width: 100%;
    border: none;
    background: transparent;
    font-weight:bold;
    font-size: 16px;
    color: #574964;
}

.task.done {
    text-decoration: line-through;
    opacity: 0.6;
}

/* Buttons */
.btn {
    background: none;
    border: none;
    cursor: pointer;
    font-size: 18px;
    color: #574964;
}
</style>
</head>
<body>

<div class="card">
<h2>To-Do List</h2>

<form method="post" class="add">
    <input type="text" name="task" placeholder="Add new task">
    <button name="add">Add</button>
</form>

<table>
<thead>
<tr>
    <th>Done</th>
    <th>Task</th>
    <th>Update</th>
    <th>Delete</th>
</tr>
</thead>
<tbody>

<?php foreach ($_SESSION['todos'] as $i => $t): ?>
<form method="post">
<tr>
    <td>
        <input type="checkbox" name="toggle"
               onchange="this.form.submit()"
               <?= $t['done'] ? 'checked' : '' ?>>
    </td>

    <td>
        <input class="task <?= $t['done'] ? 'done' : '' ?>"
               name="text"
               value="<?= $t['text'] ?>">
    </td>

    <td>
        <button class="btn" name="update">✔</button>
    </td>

    <td>
        <button class="btn" name="delete">✕</button>
    </td>

    <input type="hidden" name="index" value="<?= $i ?>">
</tr>
</form>
<?php endforeach; ?>

</tbody>
</table>

</div>

</body>
</html>
