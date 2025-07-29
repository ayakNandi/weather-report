# weather-report
import requests

def get_weather_report(city_name, api_key):
    """
    Fetches the current weather report for a given city.

    Args:
        city_name (str): The name of the city.
        api_key (str): Your OpenWeatherMap API key.

    Returns:
        str: A formatted weather report or an error message.
    """
    base_url = "http://api.openweathermap.org/data/2.5/weather"
    params = {
        "q": city_name,
        "appid": api_key,
        "units": "metric"  # For temperature in Celsius
    }

    try:
        response = requests.get(base_url, params=params)
        response.raise_for_status()  # Raise an exception for HTTP errors
        data = response.json()

        if data["cod"] == 200:
            main_data = data["main"]
            weather_data = data["weather"][0]
            wind_data = data["wind"]

            temperature = main_data["temp"]
            humidity = main_data["humidity"]
            pressure = main_data["pressure"]
            description = weather_data["description"]
            wind_speed = wind_data["speed"]

            report = (
                f"Weather Report for {city_name}:\n"
                f"Temperature: {temperature}°C\n"
                f"Description: {description.capitalize()}\n"
                f"Humidity: {humidity}%\n"
                f"Pressure: {pressure} hPa\n"
                f"Wind Speed: {wind_speed} m/s"
            )
            return report
        else:
            return f"Error: {data['message']}"
    except requests.exceptions.RequestException as e:
        return f"Network error: {e}"
    except KeyError:
        return "Error: Could not parse weather data. Check city name or API key."

if _name_ == "_main_":
    # Replace 'YOUR_API_KEY' with your actual OpenWeatherMap API key
    # You can get one by signing up at openweathermap.org
    api_key = "YOUR_API_KEY"
    city = input("Enter the city name: ")

    if api_key == "YOUR_API_KEY":
        print("Please replace 'YOUR_API_KEY' with your actual OpenWeatherMap API key.")
    else:
        weather_report = get_weather_report(city, api_key)
        print(weather_report)
