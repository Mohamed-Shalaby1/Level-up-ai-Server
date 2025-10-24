Level Up AI Server
------------------
Simple Node/Express server that proxies requests to OpenAI to classify user activities into one of four categories
and return an objective points score.

Files:
- index.js : main server code
- package.json : dependencies & start script
- .gitignore : ignoring node_modules and .env

Quick deploy to Render (recommended)
-----------------------------------
1. Create a new GitHub repository (e.g., level-up-ai-server) and push these files.
2. Sign up / login to https://render.com and connect your GitHub account.
3. Create a new Web Service:
   - Select the repo you pushed.
   - Build Command: npm install
   - Start Command: npm start
   - Plan: Free (default)
4. In the Service settings on Render, add an Environment Variable:
   - Key: OPENAI_API_KEY
   - Value: (your OpenAI secret key starting with sk-...)
5. Deploy. When ready, Render gives you a public URL like https://your-app.onrender.com
6. Use POST https://your-app.onrender.com/analyze with JSON body: { "text": "your activity description" }

Usage from Flutter (replace URL with your deployed URL)
------------------------------------------------------
Example Dart function (add http package in pubspec):
```dart
import 'dart:convert';
import 'package:http/http.dart' as http;

Future<Map<String, dynamic>> analyzeActivity(String text) async {
  final url = Uri.parse('https://your-app.onrender.com/analyze');
  final resp = await http.post(url, headers: {'Content-Type':'application/json'}, body: jsonEncode({'text': text}));
  if (resp.statusCode != 200) throw Exception('Server error: ${resp.body}');
  return jsonDecode(resp.body) as Map<String, dynamic>;
}
```

Security notes
--------------
- Never put your OPENAI_API_KEY inside Flutter app code or commit it to Git.
- Keep it as an environment variable on Render (or similar).
- Consider adding simple auth (e.g., a secret token) to prevent abuse if you plan to share the app publicly.
