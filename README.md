from flask import Flask, render_template_string, request

app = Flask(__name__)

seats = {
    "A1": "free",
    "A2": "free",
    "A3": "free",
    "A4": "free"
}

html = """
<!DOCTYPE html>
<html>
<head>
<title>Cafe Seat Booking</title>
<style>
body { text-align:center; font-family:Arial; background-color:#f0f0f0; }
h1 { color:#4CAF50; }
label {
    margin: 10px;
    padding: 10px;
    border:2px solid #4CAF50;
    border-radius:8px;
    display:inline-block;
    cursor:pointer;
}
input[type='radio'] { margin-right:5px; }
.free { background-color:#e7f5e7; }
.booked { background-color:#f5e7e7; color:#888; }
button {
    padding:10px 20px;
    background-color:#4CAF50;
    color:white;
    border:none;
    border-radius:5px;
    cursor:pointer;
}
button:hover { background-color:#45a049; }
</style>
</head>
<body>

<h1>Cafe Seat Booking System</h1>

<form method="post">
{% for seat, status in seats.items() %}
<label class="{{status}}">
<input type="radio" name="seat" value="{{seat}}" {% if status=='booked' %}disabled{% endif %}>
{{seat}} - {{status}}
</label>
{% endfor %}
<br><br>
<button type="submit">Book Seat</button>
</form>

</body>
</html>
"""

@app.route("/", methods=["GET", "POST"])
def home():
    if request.method == "POST":
        seat = request.form.get("seat")
        if seat:
            seats[seat] = "booked"
    return render_template_string(html, seats=seats)

if __name__ == "__main__":
    app.run(debug=True)