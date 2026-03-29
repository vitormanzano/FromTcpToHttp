# CH9: Binary Data

as we've covered, HTTP is a text-based protocol, but that doesn't mean it can't send binary data. In fact, HTTP is quite good at sending binary data, and it does so by using the Content-Type header to indicate the type of data being sent. For example, if you're sending an image, you might use Content-Type: image/png, and if you're sending a video, you might use Content-Type: video/mp4.

That way, the client knows how to interpret the body. Rather than reading it as raw text, it can expect a specific format of video data, for example.

Assignment
The beauty of our current abstraction is that we already have all the tools we need to respond with binary data! Let's add a handler that responds with a video.

Use curl to download this video file:
mkdir assets
curl -o assets/vim.mp4 https://storage.googleapis.com/qvault-webapp-dynamic-assets/lesson_videos/vim-vs-neovim-prime.mp4

Add the assets folder to .gitignore, so you don't accidentally commit it.
Add a new handler that responds to the GET /video endpoint with the video file. Use the Content-Type: video/mp4 header. I used os.ReadFile to just read the whole file into memory (probably not the most efficient, but works for our demo).
Navigate to http://localhost:42069/video in your browser... does it work?

HTTP 2 and 3
First of all, great job! You've made it through the TCP to HTTP course!

My goal with this course is to dispel magic. Not the magic of discovering something new, the enjoyment of a new technology or programming language, the ability to see a problem from a new angle. No, magic that makes you feel like you just might not be capable enough. You are capable.
That said, you've learned a whole lot about HTTP/1.1, but I'd be remiss if I didn't just leave you with a brief teaser of HTTP 2 and 3, because you will see them around, and they're very different from HTTP/1.1. However, it's notable that a lot of the internet still runs on HTTP/1.1.

HTTP/2
Some key differences include:

It's a binary protocol rather than text-based. This makes it more efficient and less error-prone, but also requires more steps to debug, typically.
HTTP/2 uses multiplexing, which allows multiple requests and responses to be sent over a single connection at the same time. This reduces latency and improves performance.
Uses header compression to save on header bandwidth.
Allows for server push, which lets the server send resources to the client before they are requested.
HTTP/3
Built on QUIC instead of TCP. QUIC is a transport layer protocol that runs over UDP, which allows for faster connection establishment and improved performance.
Mandates encryption at the HTTP protocol level (HTTP/1.1 is unencrypted by default, meaning it requires HTTPS to be secure).


