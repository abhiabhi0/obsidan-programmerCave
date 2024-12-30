- Strategy for limiting network traffic 
- Puts a cap on the number of requests in a certain timeframe 
- Helps prevent malicious bot activity and reduce strain on web servers 
- Not a complete solution for managing bot activity

### Bot Attacks Stopped by Rate Limiting: 
- Brute force attacks 
- Denial of Service (DoS) and Distributed Denial of Service (DDoS) attacks 
- Web scraping 
- API overuse

### How Rate Limiting Works: 
- Runs within an application, not on the web server 
- Based on tracking IP addresses and time between requests 
- If too many requests from a single IP within the timeframe, requests are temporarily blocked

### Rate Limiting for APIs: 
- Limits the number of API calls per hour or day by each unique user 
- Prevents overuse of APIs by third-party developers 
- Protects against malicious bot attacks that could crash or overload the service