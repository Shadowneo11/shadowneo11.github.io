``` index.js

const express = require('express');
const path = require('path');
const app = express();
const PORT = process.env.PORT || 3000;

// Middleware to parse JSON and URL-encoded bodies
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Serve the homepage
app.get('/', (req, res) => {
    res.sendFile(path.join(__dirname, 'index.html'));
});

// The /echo endpoint for all request methods (GET, POST, etc.)
app.all('/echo', (req, res) => {
    res.json({
        method: req.method,
        headers: req.headers,
        query: req.query,
        body: req.body
    });
});

// Start the server
app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
}); ```

``` index.html

<!DOCTYPE html>
<html lang="en">
        <head>
                <meta charset="UTF-8">
                <meta name="viewport" content="width=device-width, initial-scale=1.0">
                <title>My Home Page</title>
                <script src="https://cdn.tailwindcss.com"></script>
        </head>
<body class="bg-gray-100 flex items-center justify-center h-screen">
        <div class="text-center bg-white p-10 rounded-lg shadow-lg">
        <h1 class="text-4xl font-bold mb-4">Welcome to My Home Page</h1>
        <p class="text-gray-600">This page is under construction. More to come!</p>
        </div>
</body>
</html> ```

``` Status/Service LOG

[Unit]
Description=Node.js Hello App
After=network.target

[Service]
# IMPORTANT: Replace 'opc' with your actual non-root username if different
User=opc
WorkingDirectory=/home/opc/node-hello-app
# This command executes the index.js file using the node interpreter
ExecStart=/usr/bin/node /home/opc/node-hello-app/index.js
Restart=always
StandardOutput=syslog
StandardError=syslog

[Install]
WantedBy=multi-user.target
[opc@g-w-instance ~]$ sudo systemctl status node-hello-app.service
● node-hello-app.service - Node.js Hello App
     Loaded: loaded (/etc/systemd/system/node-hello-app.service; enabled; preset: disabled)
     Active: active (running) since Thu 2025-10-09 20:19:52 GMT; 34min ago
   Main PID: 298914 (node)
      Tasks: 11 (limit: 2359)
     Memory: 4.1M
        CPU: 554ms
     CGroup: /system.slice/node-hello-app.service
             └─298914 /usr/bin/node /home/opc/node-hello-app/index.js

Oct 09 20:19:52 g-w-instance systemd[1]: Started Node.js Hello App.
Oct 09 20:19:53 g-w-instance node[298914]: Server is running on http://localhost:3000
Oct 09 20:42:03 g-w-instance node[298914]: Error: Parse Error: Invalid method encountered ```
