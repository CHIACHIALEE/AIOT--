from flask import Flask
 
app = Flask(__name__)
 
@app.route("/")
def home():
    return "Hello, Flask on Raspberry Pi 5!"
 
if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=True)



![20250102_22h20m12s_grim](https://github.com/user-attachments/assets/9561c014-93ca-4dfc-959d-bcd9cb308d67)
