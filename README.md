from flask import Flask
import os
import datetime
import subprocess

app = Flask(__name__)

@app.route('/htop')
def htop():
    # Get system username
    username = os.getenv("USER") or os.getenv("USERNAME") or "Unknown"

    # Get current time in IST (Indian Standard Time)
    ist_time = datetime.datetime.utcnow() + datetime.timedelta(hours=5, minutes=30)
    formatted_time = ist_time.strftime("%Y-%m-%d %H:%M:%S IST")

    # Get top command output (first 10 lines for brevity)
    try:
        top_output = subprocess.check_output("top -b -n 1 | head -10", shell=True, text=True)
    except Exception as e:
        top_output = str(e)

    return f"""
    <h1>System Information</h1>
    <p><strong>Name:</strong> Komal Sood</p>
    <p><strong>Username:</strong> {username}</p>
    <p><strong>Server Time:</strong> {formatted_time}</p>
    <pre>{top_output}</pre>
    """

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5001)

