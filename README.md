#live projects

from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__)

todos = []

@app.route("/")
def index():
    return render_template("index.html", todos=todos)

@app.route("/add", methods=["POST"])
def add_todo():
    todo = request.form["todo"]
    todos.append(todo)
    return redirect(url_for("index"))

@app.route("/delete/<int:index>")
def delete_todo(index):
    try:
        del todos[index]
    except IndexError:
        pass
    return redirect(url_for("index"))

if __name__ == "__main__":
    app.run(debug=True)


index.html

<!DOCTYPE html>
<html>
<head>
    <title>Live Todo List</title>
</head>
<body>
    <h1>Todo List</h1>
    <form action="/add" method="post">
        <input type="text" name="todo" placeholder="Add todo">
        <input type="submit" value="Add">
    </form>
    <ul>
    {% for todo in todos %}
        <li>{{ todo }} <a href="/delete/{{ loop.index0 }}">Delete</a></li>
    {% endfor %}
    </ul>
</body>
</html>
