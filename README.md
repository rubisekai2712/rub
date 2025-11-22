import requests,json,time,os,threading 
from flask import Flask, jsonify, request
app = Flask(__name__)
PORT = int(os.environ.get("PORT", 5000))
login_data = {
    "username": "rub2712",
    "email": "",
    "password": {
        "value": "nguyen3452", 
        "repeat": ""
    }
}
sub_url = "https://www.mcserverhost.com/api/servers/1d1d65c4/subscription" 
def run_automation():
    session = requests.Session()
    while True:
        response = session.post("https://www.mcserverhost.com/api/login", headers={'Content-Type': 'application/json'}, data=json.dumps(login_data))
        if response.status_code == 200:
            session.post(sub_url, headers={'Content-Type': 'application/json'}, data=json.dumps({}))
        elif response.status_code == 406:
            print("Sai tài kho?n ho?c m?t kh?u r?i!")
        elif response.status_code == 403:
            print("IP c?a b?n có th? dã b? ch?n b?i website")
        else:
            print(f"L?i {response.status_code} - {response.text}")
        time.sleep(3000)
@app.route('/')
def home():
    return "Renew dang ch?y."
if __name__ == '__main__':
    au= threading.Thread(target=run_automation)
    au.daemon = True
    au.start()
    app.run(host='0.0.0.0', port=PORT)
