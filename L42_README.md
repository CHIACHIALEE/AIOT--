from flask import Flask
 
app = Flask(__name__)
 
@app.route("/")
def home():
    return "Hello, Flask on Raspberry Pi 5!"
 
if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=True)


![20250102_21h46m05s_grim](https://github.com/user-attachments/assets/422abda7-5ec7-4f57-ad7c-3e5180da12fc)


