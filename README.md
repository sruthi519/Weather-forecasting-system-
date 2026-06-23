# Weather Forecasting System

## Overview

Weather Forecasting System is a Java-based application that retrieves real-time weather information using the OpenWeatherMap API. The application allows users to enter a city name and view current weather details such as temperature, humidity, weather conditions, wind speed, sunrise time, and sunset time.

The project demonstrates API integration, HTTP communication, JSON data processing, and object-oriented programming concepts in Java.

## Features

* Fetch real-time weather data from OpenWeatherMap API
* Search weather information by city name
* Display current temperature
* Show "Feels Like" temperature
* Display humidity percentage
* Show weather conditions
* Display wind speed
* Show sunrise and sunset timings
* Handle invalid city names gracefully

## Technologies Used

* Java
* OpenWeatherMap API
* HTTPURLConnection
* BufferedReader
* Object-Oriented Programming (OOP)

## Project Structure

```text
Weather-Forecasting-System/
│
├── WeatherApp.java
├── README.md
```

## How It Works

1. User enters a city name.
2. The application sends a request to the OpenWeatherMap API.
3. Weather data is received in JSON format.
4. The JSON response is parsed manually.
5. Weather information is displayed in a structured format.

## Sample Output

```text
===== Real-Time Weather Report =====
Location    : Chennai
Temperature : 32°C
Feels Like  : 36°C
Humidity    : 75%
Condition   : Clear Sky
Wind Speed  : 4.5 m/s
Sunrise     : 1719360000
Sunset      : 1719405000
====================================
```

## Installation and Execution

### Compile

```bash
javac WeatherApp.java
```

### Run

```bash
java WeatherApp
```

## Learning Outcomes

* API Integration in Java
* HTTP Request Handling
* JSON Data Processing
* Exception Handling
* Object-Oriented Design
* Real-Time Data Retrieval

## Future Enhancements

* Graphical User Interface (GUI)
* Multi-city weather comparison
* Weather forecast for upcoming days
* Automatic location detection
* Weather alerts and notifications

## Author

Developed as a Java project to explore real-time weather data retrieval, API integration, and object-oriented programming concepts.

