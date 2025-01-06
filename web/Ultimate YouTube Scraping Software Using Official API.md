
# Ultimate YouTube Scraping Software Using Official API

![YouTube Scraper Overview](https://niuimg.niucores.com/wp-content/uploads/2024/08/4210636841376411496.png)

Are you looking to scrape YouTube search results efficiently? I’ve developed a Python-based scraping software that utilizes YouTube's **official API** to provide accurate and stable results. This tool is user-friendly, even for non-programmers, and comes with a GUI for quick use—no Python installation or coding knowledge required!

---

## Key Features of the Software

This scraping tool extracts data directly from YouTube using the official API, ensuring reliability and speed. It gathers 14 essential fields for each video:

- **Search Keyword**
- **Page Number**
- **Video Title**
- **Video ID**
- **Video Link**
- **Publish Date**
- **Duration**
- **Channel Name**
- **Channel ID**
- **Channel Link**
- **View Count**
- **Like Count**
- **Comment Count**
- **Video Description**

### Why Use This Tool?

- **No programming required:** Double-click to start using—perfect for beginners.
- **Accurate results:** Built using YouTube’s API for stable performance.
- **Convenient GUI:** Designed for easy data scraping without code changes.

---

## Software Screenshots

- **User Interface**  
  ![Software Interface](https://niuimg.niucores.com/wp-content/uploads/2024/08/6890105233238366371.png)

- **Scraped Results**  
  Screenshot 1:  
  ![Results Example 1](https://niuimg.niucores.com/wp-content/uploads/2024/08/6217767024290488281.png)  

  Screenshot 2:  
  ![Results Example 2](https://niuimg.niucores.com/wp-content/uploads/2024/08/4165660364773856347.png)  

  Screenshot 3:  
  ![Results Example 3](https://niuimg.niucores.com/wp-content/uploads/2024/08/3075604543231114645.png)

---

## Advertisement: Enhance Your Web Scraping Efficiency

Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more.  
👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Software Demo

For a step-by-step guide on how to use the software, check out the demo video. This is especially useful for beginners who want to see the software in action:  
👉 [View the full demo here](https://www.niucores.com/?golink=aHR0cHM6Ly93d3cuY25ibG9ncy5jb20vbWFzaHVrdWkvcC8xODE3NjkxOC95dGJfc2VhcmNoX3ZpZGVvX3Rvb2wjMTItJUU2JUJDJTk0JUU3JUE0JUJBJUU4JUE3JTg2JUU5JUEyJTkx)

---

## API Integration: Code Overview

### Step 1: Calling the Search API

The software uses YouTube’s Search API to fetch initial results. Below is an example of the JSON response returned by the API:

![Search API JSON Example](https://niuimg.niucores.com/wp-content/uploads/2024/08/1218926000816100588.png)

The API request is structured as follows:

```python
# Search API endpoint
url = 'https://youtube.googleapis.com/youtube/v3/search'

# Headers to mimic a browser request
self.headers = {
    "Accept": "*/*",
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"
}

# Parameters for the request
params = {
    'part': 'snippet',
    'maxResults': '25',
    'q': search_keyword,
    'key': self.API_KEY,
    'pageToken': pageToken,
    'order': self.sort_by,
    'publishedBefore': str(self.end_date) + 'T00:00:00Z',
    'publishedAfter': str(self.start_date) + 'T00:00:00Z',
}
```

### Step 2: Calling the Video Details API

Once the search results are retrieved, the Video Details API is used to fetch additional details about each video. Example of the returned JSON:

![Details API JSON Example](https://niuimg.niucores.com/wp-content/uploads/2024/08/6322088111367335448.png)

API request example:

```python
# Video Details API endpoint
url = 'https://youtube.googleapis.com/youtube/v3/videos?part=snippet%2CcontentDetails%2Cstatistics&id={}&key={}'.format(video_id, self.API_KEY)

# Sending the request
response = requests.post(url, headers=self.headers)
# Parsing the JSON response
json_data = response.json()
```

---

## Saving Scraped Data to CSV

Data is saved incrementally to prevent loss in case of interruptions:

```python
# Save data to CSV
with open(self.result_file, 'a+', encoding='utf_8_sig', newline='') as f:
    writer = csv.writer(f)
    writer.writerow(
        [search_keyword, page, title, videoId, video_url, create_time, duration, channelTitle,
         channelId, channel_url, viewCount, likeCount, commentCount, desc])
self.tk_show('CSV saved successfully: ' + self.result_file)
```

---

## API Key Setup

An **API Key** is required to access YouTube’s API. Follow this detailed guide to obtain and configure your API key:  
👉 [How to Set Up YouTube Data API v3](https://www.niucores.com/?golink=aHR0cHM6Ly93d3cuY25ibG9ncy5jb20vbWFzaHVrdWkvcC8xODE3Mzg3OA==)

Once obtained, place the key in your `config.json` file as shown below:

![Config File Example](https://niuimg.niucores.com/wp-content/uploads/2024/08/4000804504639396691.png)

---

## User Interface Highlights

The software’s GUI is built with **Tkinter** and offers the following features:

### Main Window

```python
# Create main window
root = tk.Tk()
root.title('YouTube Scraper v1.0 | Python Solutions')
root.minsize(width=850, height=650)
root.iconbitmap('icon.ico')
```

### Input Fields

```python
# Search keyword input
tk.Label(root, text='Search Keyword:').place(x=30, y=90)
entry_kw = tk.Text(root, width=70, height=2)
entry_kw.place(x=125, y=90)
```

### Logs Display

```python
# Logging section
tk.Label(root, text='Logs:').place(x=30, y=280)
log_frame = tk.Frame(width=780, height=260)
log_frame.place(x=30, y=310)
```

---

## Advanced Features: Logging and Debugging

A comprehensive logging module is included to quickly identify and resolve issues:

```python
def get_logger(self):
    self.logger = logging.getLogger(__name__)
    formatter = '[%(asctime)s-%(filename)s][%(funcName)s-%(lineno)d]--%(message)s'
    self.logger.setLevel(logging.DEBUG)

    # Console logging
    sh = logging.StreamHandler()
    log_formatter = logging.Formatter(formatter, datefmt='%Y-%m-%d %H:%M:%S')

    # Log file settings
    info_file_name = time.strftime("%Y-%m-%d") + '.log'
    case_dir = r'./logs/'
    info_handler = TimedRotatingFileHandler(filename=case_dir + info_file_name, 
                                            when='MIDNIGHT', 
                                            interval=1, 
                                            backupCount=7, 
                                            encoding='utf-8')
```

---

## Conclusion

With this software, scraping YouTube search results is now accessible to everyone, regardless of programming expertise. Whether you’re conducting market research, gathering video data, or analyzing trends, this tool provides all the necessary features for seamless scraping.

👉 [Start your free trial with ScraperAPI to elevate your scraping capabilities!](https://bit.ly/Scraperapi)
