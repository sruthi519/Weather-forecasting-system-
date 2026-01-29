import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Scanner;

// Holds weather details
class WeatherData {
    String city;
    String temperature;
    String feelsLike;
    String humidity;
    String condition;
    String windSpeed;
    String sunrise;
    String sunset;

    public WeatherData(String city, String temperature, String feelsLike, String humidity,
                       String condition, String windSpeed, String sunrise, String sunset) {
        this.city = city;
        this.temperature = temperature;
        this.feelsLike = feelsLike;
        this.humidity = humidity;
        this.condition = condition;
        this.windSpeed = windSpeed;
        this.sunrise = sunrise;
        this.sunset = sunset;
    }
}

// Fetches weather data from OpenWeatherMap
class WeatherFetcher {
    private final String apiKey;

    public WeatherFetcher(String apiKey) {
        this.apiKey = apiKey;
    }

    public String fetchWeatherJSON(String location) {
        StringBuilder response = new StringBuilder();
        try {
            String urlString = "http://api.openweathermap.org/data/2.5/weather?q="
                    + location + "&units=metric&appid=" + apiKey;
            URL url = new URL(urlString);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("GET");

            if (conn.getResponseCode() == 404) {
                System.out.println("City not found! Please check the spelling.");
                return "";
            }

            try (BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()))) {
                String inputLine;
                while ((inputLine = in.readLine()) != null) {
                    response.append(inputLine);
                }
            }
        } catch (java.io.IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
        return response.toString();
    }
}

// Parses JSON manually (no libraries)
class WeatherParser {
    private static String extractValue(String json, String key) {
        String searchKey = "\"" + key + "\":";
        int start = json.indexOf(searchKey);
        if (start == -1) return "N/A";
        start += searchKey.length();
        if (json.charAt(start) == '"') {
            start++;
            int end = json.indexOf("\"", start);
            return json.substring(start, end);
        } else {
            int end = json.indexOf(",", start);
            if (end == -1) end = json.indexOf("}", start);
            return json.substring(start, end);
        }
    }

    private static String extractDescription(String json) {
        String key = "\"description\":\"";
        int start = json.indexOf(key);
        if (start == -1) return "N/A";
        start += key.length();
        int end = json.indexOf("\"", start);
        return json.substring(start, end);
    }

    public static WeatherData parseWeather(String json) {
        if (json.isEmpty()) return new WeatherData("N/A", "N/A", "N/A", "N/A", "N/A", "N/A", "N/A", "N/A");

        String city = extractValue(json, "name");
        String temp = extractValue(json, "temp");
        String feelsLike = extractValue(json, "feels_like");
        String humidity = extractValue(json, "humidity");
        String condition = extractDescription(json);
        String windSpeed = extractValue(json, "speed");
        String sunrise = extractValue(json, "sunrise");
        String sunset = extractValue(json, "sunset");

        return new WeatherData(city, temp, feelsLike, humidity, condition, windSpeed, sunrise, sunset);
    }
}

// Displays weather
class WeatherDisplay {
    public void show(WeatherData data) {
        System.out.println("\n===== Real-Time Weather Report =====");
        System.out.println("Location    : " + data.city);
        System.out.println("Temperature : " + data.temperature + "°C");
        System.out.println("Feels Like  : " + data.feelsLike + "°C");
        System.out.println("Humidity    : " + data.humidity + "%");
        System.out.println("Condition   : " + data.condition);
        System.out.println("Wind Speed  : " + data.windSpeed + " m/s");
        System.out.println("Sunrise     : " + data.sunrise + " (Unix time)");
        System.out.println("Sunset      : " + data.sunset + " (Unix time)");
        System.out.println("====================================");
    }
}

// Main App
public class WeatherApp {
    public static void main(String[] args) {
        try (Scanner scanner = new Scanner(System.in)) {
            System.out.print("Enter location: ");
            String location = scanner.nextLine();
            String apiKey = "2083b47e6be792ab79697a80e9ca33df";
            WeatherFetcher fetcher = new WeatherFetcher(apiKey);
            String json = fetcher.fetchWeatherJSON(location);

            WeatherData data = WeatherParser.parseWeather(json);
            WeatherDisplay display = new WeatherDisplay();
            display.show(data);
        }
    }
}# Weather-forecasting-system-
