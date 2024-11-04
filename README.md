Step 1: Install Required Libraries
You will need the following libraries:

requests: To fetch weather data from the API.
tkinter: For creating the GUI.
Pillow: For image handling (for weather icons).
You can install the required libraries using pip:

bash

Verify

Open In Editor
Edit
Copy code
pip install requests Pillow
Step 2: Obtain OpenWeatherMap API Key
As mentioned earlier, sign up at OpenWeatherMap and get your API key.

Step 3: Create the Weather Application
Here's a complete example of a weather application using Tkinter:

python

Verify

Open In Editor
Edit
Copy code
import tkinter as tk
from tkinter import messagebox
import requests
from PIL import Image, ImageTk
import io

class WeatherApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Weather App")
        self.root.geometry("400x500")

        self.api_key = 'YOUR_API_KEY'  # Replace with your OpenWeatherMap API key

        # Create UI elements
        self.city_label = tk.Label(root, text="Enter City Name:")
        self.city_label.pack()

        self.city_entry = tk.Entry(root)
        self.city_entry.pack()

        self.get_weather_button = tk.Button(root, text="Get Weather", command=self.get_weather)
        self.get_weather_button.pack()

        self.weather_info = tk.Text(root, width=50, height=15)
        self.weather_info.pack()

        self.weather_icon = tk.Label(root)
        self.weather_icon.pack()

    def get_weather(self):
        city = self.city_entry.get()
        if city:
            self.fetch_weather_data(city)
        else:
            messagebox.showwarning("Input Error", "Please enter a city name.")

    def fetch_weather_data(self, city):
        base_url = "http://api.openweathermap.org/data/2.5/weather?"
        complete_url = f"{base_url}q={city}&appid={self.api_key}&units=metric"
        
        response = requests.get(complete_url)

        if response.status_code == 200:
            data = response.json()
            main = data['main']
            weather = data['weather'][0]
            wind = data['wind']

            temperature = main['temp']
            humidity = main['humidity']
            weather_description = weather['description']
            wind_speed = wind['speed']

            self.display_weather_info(city, temperature, humidity, weather_description, wind_speed)
            self.display_weather_icon(weather['icon'])
        else:
            messagebox.showerror("Error", "City not found or invalid API key.")

    def display_weather_info(self, city, temperature, humidity, description, wind_speed):
        self.weather_info.delete(1.0, tk.END)  # Clear previous text
        weather_data = (
            f"City: {city}\n"
            f"Temperature: {temperature}°C\n"
            f"Humidity: {humidity}%\n"
            f"Description: {description.capitalize()}\n"
            f"Wind Speed: {wind_speed} m/s\n"
        )
        self.weather_info.insert(tk.END, weather_data)

    def display_weather_icon(self, icon_code):
        icon_url = f"http://openweathermap.org/img/wn/{icon_code}@2x.png"
        response = requests.get(icon_url)
        img_data = response.content
        img = Image.open(io.BytesIO(img_data))
        img = img.resize((100, 100), Image.ANTIALIAS)
        self.weather_icon.image = ImageTk.PhotoImage(img)
        self.weather_icon.config(image=self.weather_icon.image)

if __name__ == "__main__":
    root = tk.Tk()
    app = WeatherApp(root)
    root.mainloop()
Step 4: Run the Application
Save the code in a file named weather_app.py.
Open your terminal and navigate to the directory where the file is saved.
Run the application using the command:
bash

Verify

Open In Editor
Edit
Copy code
python weather_app.py
Explanation of the Code
Imports: The necessary libraries are imported. requests is used for API calls, tkinter for the GUI, and Pillow for handling images.

WeatherApp Class:

Initializes the main window and UI elements (labels, entry field, buttons, text box for displaying weather info, and a label for the weather icon).
get_weather Method:

Fetches the city name from the entry field and calls fetch_weather_data if the input is valid.
fetch_weather_data Method:
