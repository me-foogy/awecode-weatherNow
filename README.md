# WEATHER APP PROJECT
___
A simple vue project that uses vue concepts and fetches the weather data from a third party api.
___
# Tech STACK
- Vue
- API (Weather Api)
___
# Data Flow
- Calls the api to fetch the lat and lon data from the name of place
- Sends the lat and lon data to child component (RightContainer)
- Child component calls the api to get the weather details right now at the specified location and displays it
- Child component again calls the api to get the forecast deatils and sends then to the DailyForecast Component
- DailyForecast Component displays it

___
## Live Link
https://awecode-weather-now.vercel.app/