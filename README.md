# Emerging IT

## PROJECT 1

### Project Discription
> In this project, your Raspberry Pi will be connected to the internet via RJ45 network cable. Turn your Raspberry Pi into a live data feeds weather and news server. Use Raspbian and install several packages that gives the Pi the ability to fetches live weather and news data and displays it on an Android tablet. Configure the supplied tablet so that it can access your Pi live weather and news feeds website.


1. Update Your System
```
sudo apt update
sudo apt upgrade -y
```

2. Install LEMP Server

```
sudo apt install nginx mariadb-server php-fpm php-mysql -y

```
2.1. Enable Firewall for nginx
```
sudo systemctl start nginx
sudo systemctl enable nginx
```

3. Install Python
```
sudo apt install python3-pip -y
```
Install Flask Req
```
sudo apt install python3-venv -y
python3 -m venv ~/venv
source ~/venv/bin/activate
pip install Flask requests
```
when show like this in terminal  (venv) host@rpi:~ $ 
type this
```
deactivate
```
Las w
```
sudo apt install python3-flask python3-requests
```

4. Make Directory for project
```
mkdir ~/live_data_project
cd ~/live_data_project
```
make app.py 
```
nano app.py
```

4.1. paste 
     **dont forget change your Api key!**

```
from flask import Flask, render_template
import requests

app = Flask(__name__)

# Replace this with your OpenWeatherMap API key
OPENWEATHERMAP_API_KEY = 'YOUR_OPENWEATHERMAP_API_KEY' # <--- PUT YOUR KEY HERE
NEWSAPI_KEY = 'YOUR_NEWSAPI_KEY'                      # <--- MAKE SURE THIS IS STILL CORRECT

@app.route('/')
def home():
    # Fetch Weather Data from OpenWeatherMap for London
    # We add "&units=metric" to get temperature in Celsius
    weather_url = f"http://api.openweathermap.org/data/2.5/weather?q=London,uk&appid={OPENWEATHERMAP_API_KEY}&units=metric"
    weather_response = requests.get(weather_url)
    weather_data = weather_response.json()

    # Fetch News Data
    news_url = f"https://newsapi.org/v2/top-headlines?country=us&apiKey={NEWSAPI_KEY}"
    news_response = requests.get(news_url)
    news_data = news_response.json()

    # Pass the weather data and the list of news articles to the template
    return render_template('index.html', weather=weather_data, news=news_data.get('articles', []))

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)
```
to exit and saye Ctrl + S and Ctrl + X and Enter


5. make folder for web ui
```
mkdir templates
cd templates
```
```
nano index.html
```

```
<!DOCTYPE html>
<html>
<head>
    <title>Live Data Dashboard</title>
</head>
<body>
    Welcome to <h1>Your Name : Station Number</h1> test Web Site
    <p><h2> web URL : enter the web URL here </h2></p>
    <h1>Weather Data</h1>
    <p>{{ weather }}</p>
    <h1>News Data</h1>
    <p>{{ news }}</p>
</body>
</html>
```
to exit and saye Ctrl + S and Ctrl + X and Enter


6. Check IP address
```
ip a
```
7. exit from templets directory and run app.py
```
cd ..
python3 app.py
```

And Access WEB use ip http://<ip addr>:8080
